
pdfCropMargins
==============

The pdfCropMargins program is a command-line application to automatically crop
the margins of PDF files.  Cropping the margins can make it easier to read the
pages of a PDF document -- whether the document is printed or displayed on a
screen -- because the display fonts are larger.  This program is similar to the
Perl script pdfcrop except with many more options.

Features
========

- Automatically detects the margins and can crop a given percentage of them.
- Can crop all the pages to the same size to give a uniform appearance (such as
  in two-up).
- Works on Linux, Windows, Cygwin, and OS X/Darwin.
- Works with either the pdftoppm program or with Ghostscript.
- A version of pdftoppm for Windows is included.
- Renders and analyzes page images to find the bounding boxes, which allows it
  to deal with noisy scanned PDFs.
- Ghostscript can optionally be used to find the bounding boxes directly.
- Can automatically apply a Ghostscript repair operation to attempt to fix
  corrupt PDF files.
- Crops rotated pages according to their appearance in the document viewer.
- Can crop pages uniformly based on the nth smallest crop values, which helps
  with noisy images or documents where a few pages have unwanted markings in
  their margins.
- Can uniformly crop even and odd pages separately.
- Can automatically run a document previewer on the output file.
- The format of automatically-generated output-file names is easily
  modifiable.
- Preserves document catalog information such as outlines if possible.
- Implements a rudimentary 'undo' capability by default. 
- Can deal with at least simple cases of password-encrypted files.
- Works with Python 2 and Python 3.

Requirements and installing
===========================

Requirements
------------

This program depends on either the Ghostscript program or the pdftoppm
program being installed (and locatable) on the system.  For Window users an
older version of a pdftoppm binary is packaged with the program and will be
used as a fallback if no other program can be found.
   
* **Ghostscript**

   Ghostscript is standard in most Linux distributions, and is easy to install
   on Windows and in Cygwin.  The Windows install page is `located here
   <http://www.ghostscript.com/download/gsdnld.html>`_, and the non-commercial
   GPL version should work fine for most people.

*  **pdftoppm**

   The pdftoppm program is standard in most Linux distributions and is easy to
   install in Cygwin.  It is currently part of the Poppler PDF tools, so that
   package should be installed on Linux and Cygwin.

   In Windows pdftoppm is not as easy to install, but a collection of PDF tools
   `found here <http://www.foolabs.com/xpdf/download.html>`_ includes pdftoppm.

Installing
----------

The easiest way to install pdfCropMargins is to install using pip::

   pip install pdfCropMargins

The ``pip`` program should be automatically installed along with Python.  On
Windows you can run ``python -m ensurepip`` if it is not, or install from the
repos in Linux.

The pdfCropMargins program can also be run directly from its source code
directory tree.  In that case the PyPDF2 package must also be installed and
available in the Python distribution that will be used to run pdfCropMargins.
If Ghostscript is unavailable as a system command then the Pillow imaging
package is also necessary in the Python distribution.  It is also needed for
certain advanced features which use explicit image analysis, so it is a good
idea to install it in Python.  Assuming the dependencies are satisfied, run the
program as ``bin/pdfCropMargins.py``, where the path is relative to the top of
the source distribution.

Running
=======

After installation via pip the program can be run with a command such as::

   pdf-crop-margins -v -s -u your-file.pdf

For help, run::

   pdf-crop-margins -h | more

On Windows you may need to explicitly put the Python distribution's ``Scripts``
directory into your environment ``PATH`` in order to avoid having to use the
full pathname.

To diagnose unexpected crops, try running with the ``-v`` verbose argument.  It
will tell you the page with the smallest crop amount for a margin.  Running
without the ``-u`` or ``-s`` options will crop each page separately, so you can
see which pages might be causing problems (such as pages with noise near the
borders or margin text).  To get cropped pages all the same size be sure to use
both ``-s`` to make pages the same size and ``-u`` to crop each page by the
same amount.  Sometimes a small pre-crop is needed (``-ap`` or ``-ap4``) to get
past small, unwanted markings near the edges of pages.

Documentation
=============

.. In vim use this to get output:
       :read !pdf-crop-margins -h

To see the documentation, run::

   pdf-crop-margins -h | more

The output of that command follows::

   Usage: pdf-crop-margins [-h] [-o OUTFILE_NAME] [-v] [-p PCT]
                           [-p4 PCT PCT PCT PCT] [-a BP] [-a4 BP BP BP BP]
                           [-ap BP] [-ap4 BP BP BP BP] [-u] [-m INT]
                           [-m4 INT INT INT INT] [-mp INT] [-s] [-ms INT] [-e]
                           [-g PAGESTR] [-t BYTEVAL] [-nb INT] [-ns INT] [-gs]
                           [-gsr] [-x DPI] [-y DPI] [-b [m|c|t|a|b]]
                           [-f [m|c|t|a|b]] [-r] [-A] [-gsf] [-nc] [-pv PROG]
                           [-mo] [-q] [-nco] [-pf] [-sc STR] [-su STR] [-ss STR]
                           [-pw PASSWD] [-spr FLOAT:FLOAT] [-dcb STR] [-dcw STR]
                           [-i] [-pdl] [-gsp PATH] [-ppp PATH]
                           PDF_FILE [PDF_FILE ...]

   Description:

        A command-line application to crop the margins of PDF files. Cropping
        the margins can make it easier to read the pages of a PDF document --
        whether the document is printed or displayed on a screen -- because the
        display fonts are larger. Margin-cropping is also sometimes useful when
        a PDF file is included in a document as a graphic.

        By default 10% of the existing margins will be retained; the rest will
        be eliminated. There are many options which can be set, however,
        including the percentage of existing margins to retain.

        Here is a simple example of cropping a file named document.pdf and
        writing the cropped output-document to a file named
        croppedDocument.pdf:

           pdf-crop-margins document.pdf -o croppedDocument.pdf

        If no destination is provided a filename will be automatically
        generated from the name of the source file (see below).

        The pdfCropMargins program works by changing the page sizes which are
        stored in the PDF file (and are interpreted by programs like Acrobat
        Reader). Both the CropBox and the MediaBox are set to the newly-
        computed cropped size. After this the view of the document in most
        programs will be the new, cropped view.

        When cropping a file not produced by the pdfCropMargins program the
        default is also to save the intersection of the MediaBox and any
        existing CropBox in the ArtBox. This saves the "usual" view of the
        original document in programs like Acrobat Reader. Subsequent crops of
        a file produced by pdfCropMargins do not by default alter the ArtBox.
        This allows for an approximate "restore to original margin-sizes"
        option ('--restore') which simply copies the saved values back to the
        MarginBox and CropBox. Note, though, that this assumes the ArtBox is
        unused (it is rarely used, and this feature can be turned off with the
        -A option).

        These defaults are designed to reduce the number of copies of a
        document which need to be saved. This is especially useful if
        annotations, highlighting, etc., are added to the document. If a
        document is cropped twice with this program it still stores the
        original margin settings. At least an approximate version of the
        original document's margin-formatting can be recovered by using the '--
        restore' option. Programs which change the "Producer" string in the PDF
        may interfere with this feature.

        Below are several examples using more of the command-line options, each
        applied to an input file called doc.pdf. The output filename is
        unspecified in these examples, so the program will automatically
        generate the filename (or an output filename can always be explicitly
        provided):

        Crop doc.pdf so that all the pages are set to the same size and the
        cropping amount is uniform across all the pages (this gives a nice two-up
        appearance).  The default of retaining 10% of the existing margins is
        used.  Note carefully that '-u' only makes the amount to be cropped uniform
        for each page; if the pages do not have the same size to begin with they
        will not have the same size afterward unless the '-s' option is also used.

           pdf-crop-margins -u -s doc.pdf

        Crop each page of doc.pdf individually (i.e., not uniformly), keeping 50%
        of the existing margins.

           pdf-crop-margins -p 50 doc.pdf

        Crop doc.pdf uniformly, keeping 50% of the left margin, 20% of the bottom
        margin, 40% of the right margin, and 10% of the top margin.

           pdf-crop-margins -u -p4 50 20 40 10 doc.pdf

        Crop doc.pdf retaining 20% of the margins, and then reduce the right page
        margins only by an absolute 12 points.

           pdf-crop-margins -p 20 -a4 0 0 12 0 doc.pdf

        Pre-crop the document by 5 points on each side before computing the
        bounding boxes.  Then crop retaining 50% of the computed margins.  This
        can be useful for difficult documents such as scanned books with page-edge
        noise or other "features" inside the current margins.

           pdf-crop-margins -ap 5 -p 50 doc.pdf

        Crop doc.pdf, re-naming the cropped output file doc.pdf and backing
        up the original file in a file named backup_doc.pdf.

           pdf-crop-margins -mo -pf -su "backup" doc.pdf

        Crop the margins of doc.pdf to 120% of their original size, increasing the
        margins.  Use Ghostscript to find the bounding boxes (in general this is
        often faster if Ghostscript is available and no rendering operations are
        needed).

           pdf-crop-margins -p 120 -gs doc.pdf

        Crop the margins of doc.pdf ignoring the 10 largest margins on each edge
        (over the whole document).  This is especially good for noisy documents
        where all the pages have very similar margins, or when you want to ignore
        marginal annotations which only occur on a few pages.

           pdf-crop-margins -m 10 doc.pdf

        Crop doc.pdf, launch the acroread viewer on the cropped output, and then
        query as to whether or not to rename the cropped file doc.pdf and back up
        the original file as doc_uncropped.pdf.

           pdf-crop-margins -mo -q doc.pdf

        Crop pages 1-100 of doc.pdf, cropping all even pages uniformly and all odd
        pages uniformly.

           pdf-crop-margins -g 1-100 -e doc.pdf

        Try to restore doc.pdf to its original margins, assuming it was cropped
        with pdfCropMargins previously.  Note that the default output filename is
        still named doc_cropped.pdf, even though it is the recovered file.

           pdf-crop-margins -r doc.pdf

        There are many different ways to use this program. After finding a
        method which works well for a particular task or workflow pattern it is
        often convenient to make a simple shell script (batch file) which
        invokes the program with those particular options and settings. Simple
        template scripts for Bash and Windows are packaged with the program, in
        the bin directory.

        When printing a document with closely-cropped pages it may be necessary
        to use options such as "Fit to Printable Area". It may also be
        necessary to fine-tune the size of the retained margins if the edges of
        the text are being cut off.

        Sometimes a PDF file is corrupted or non-standard to the point where
        the routines used by this program raise an error and exit. In that case
        it can sometimes help to repair the PDF file before attempting to crop
        it. If it is readable by Ghostscript then the following command will
        often repair it sufficiently:

           gs -o repaired.pdf -sDEVICE=pdfwrite -dPDFSETTINGS=/prepress corrupted.pdf

        This command can also be used to convert some PostScript (.ps) files to
        PDF. In Windows the executable would be something like "gswin32c.exe"
        rather than "gs". The option '--gsFix' (or '-gsf') will automatically
        attempt to apply this fix, provided Ghostscript is available. See the
        description of that option for more information.

        The pdfCropMargins program handles rotated pages (such as pages in
        landscape mode versus portrait mode) as follows. All rotated pages are
        un-rotated as soon as they are read in. All the cropping is then
        calculated. Finally, as the crops are applied to the pages, the
        rotation is re-applied. This may give unexpected results in documents
        which mix pages at different rotations, especially with the '--uniform'
        or '--samePageSize' options. The arguments of all the options which
        take four arguments, one for each margin, are shifted so the left,
        bottom, right, and top margins correspond to the screen appearance
        (regardless of any internal rotation).

        All the command-line options to pdfCropMargins are described below. The
        following definition is useful in precisely defining what several of
        the options do. Let the delta values be the absolute reduction lengths,
        in points, which are applied to each original page to get the final
        cropped page. There is a delta value for each margin, on each page. In
        the usual case where all the margin sizes decrease, all the deltas are
        positive. A delta value can, however, be negative (when percentRetain >
        100 or when a negative absolute offset is used). When a delta value is
        negative the corresponding margin size will increase.
      

   Positional arguments:

     PDF_FILE              The pathname of the PDF file to crop. Use quotes
                           around any file or directory name which contains a
                           space. If no filename is given for the cropped PDF
                           output file via the '-o' flag then a default output
                           filename will be generated. By default it is the same
                           as the source filename except that the suffix ".pdf"
                           is replaced by "_cropped.pdf", overwriting by default
                           if the file already exists. The file will be written
                           to the working directory at the time when the program
                           was run. If the input file has no extension or has an
                           extension other than '.pdf' or '.PDF' then the suffix
                           '.pdf' will be appended to the existing (possibly-
                           null) extension. Globbing of wildcards is performed on
                           Windows systems.


   Optional arguments:

     -h, --help            Show this help message and exit.

     -o OUTFILE_NAME, --outfile OUTFILE_NAME
                           An optional argument specifying the pathname of a file
                           that the cropped output document should be written to.
                           By default any existing file with the same name will
                           be silently overwritten. If this option is not given
                           the program will generate an output filename from the
                           input filename. (By default "_cropped" is appended to
                           the input filename before the file extension. If the
                           extension is not '.pdf' or '.PDF' then '.pdf' is
                           appended to the extension). Globbing of wildcards is
                           performed on Windows systems.

     -v, --verbose         Print more information about the program's actions and
                           progress. Without this switch only warning and error
                           messages are printed to the screen.

     -p PCT, --percentRetain PCT
                           Set the percent of margin space to retain in the
                           image. This is a percentage of the original margin
                           space. By default the percent value is set to 10.
                           Setting the percentage to 0 gives a tight bounding
                           box. Percent values greater than 100 increase the
                           margin sizes from their original sizes, and negative
                           values decrease the margins even more than a tight
                           bounding box.

     -p4 PCT PCT PCT PCT, -pppp PCT PCT PCT PCT, --percentRetain4 PCT PCT PCT PCT
                           Set the percent of margin space to retain in the
                           image, individually for the left, bottom, right, and
                           top margins, respectively. The four arguments should
                           be percent values.

     -a BP, --absoluteOffset BP
                           Decrease each margin size by an absolute floating
                           point offset value, to be subtracted from each
                           margin's size. The units are big points, bp, which is
                           the unit used in PDF files. There are 72 bp in an
                           inch. A single bp is approximately equal to a TeX
                           point, pt (with 72.27pt in an inch). Negative values
                           are allowed; positive numbers always decrease the
                           margin size and negative numbers always increase it.
                           Absolute offsets are always applied after any
                           percentage change operations.

     -a4 BP BP BP BP, -aaaa BP BP BP BP, --absoluteOffset4 BP BP BP BP
                           Decrease the margin sizes individually with four
                           absolute offset values. The four floating point
                           arguments should be the left, bottom, right, and top
                           offset values, respectively. See the '--
                           absoluteOffset' option for information on the
                           units.

     -ap BP, --absolutePreCrop BP
                           This option is like '--absoluteOffset' except that the
                           changes are applied before any bounding box
                           calculations (or any other operations). The argument
                           is the same, in units of bp. This is essentially
                           equivalent to first cropping the document retaining
                           100% of the margins but applying an absolute offset
                           and then doing any other operations on that pre-
                           cropped file.

     -ap4 BP BP BP BP, --absolutePreCrop4 BP BP BP BP
                           This is the same as '--absolutePreCrop' except that
                           four separate arguments can be given. The four
                           floating point arguments should be the left, bottom,
                           right, and top absolute pre-crop values,
                           respectively.

     -u, --uniform         Crop all the pages uniformly. This forces the
                           magnitude of margin-cropping (absolute, not relative)
                           to be the same on each page. This option is applied
                           after all the delta values have been calculated for
                           each page, individually. Then all the left-margin
                           delta values, for each page, are set to the smallest
                           left-margin delta value over every page. The bottom,
                           right, and top margins are processed similarly. Note
                           that this effectively adds some margin space (relative
                           to the margins obtained by cropping pages
                           individually) to some of the pages. If the pages of
                           the original document are all the same size then the
                           cropped pages will again all be the same size. The '--
                           samePageSize' option can also be used in combination
                           with this option to force all pages to be the same
                           size after cropping.

     -m INT, --uniformOrderStat INT
                           Choosing this option implies the '--uniform' option,
                           but the smallest delta value over all the pages is no
                           longer chosen. Instead, for each margin the nth
                           smallest delta value (with n numbered starting at
                           zero) is chosen over all the pages. The argument is
                           the integer n, for example '-m 4'. Choosing n to be
                           half the number of pages gives the median delta value.
                           This option is useful for cropping noisy scanned PDFs
                           which have a common margin size on most of the pages,
                           or for ignoring annotations which only appear in the
                           margins of a few pages. This option essentially causes
                           the program to ignores the n largest tight-crop
                           margins when computing common delta values over all
                           the pages. Increasing n always either increases the
                           cropping amount or leaves it unchanged. Some trial-
                           and-error may be needed to choose the best number.
                           Using '-m 1' tends to work well with arXiv papers
                           (which have a date in the margin of the first
                           page).

     -m4 INT INT INT INT, -mmmm INT INT INT INT, --uniformOrderStat4 INT INT INT INT
                           This option is the same as '--uniformOrderStat' (or
                           '-m') except that separate values are specified for
                           each margin individually. The margins are ordered as
                           left, bottom, right, and top.

     -mp INT, --uniformOrderPercent INT
                           This option is the same as '--uniformOrderStat' except
                           that the order number n is automatically set to a
                           given percentage of the number of pages which are set
                           to be cropped (either the full number or the ones set
                           with '--pages'). This option overrides '--
                           uniformOrderStat' if both are set. The argument is a
                           float percent value; rounding is done to get the final
                           order-number. Setting the percent to 0 is equivalent
                           to n=1, setting the percent to 100 is equivalent to
                           setting n to the full number of pages, and setting the
                           percent to 50 gives the median (for odd numbers of
                           pages).

     -s, --samePageSize    Set all the page sizes to be equal. This option only
                           has an effect when the page sizes are different. The
                           pages sizes are set to the size of the union of all
                           the page regions, i.e., to the smallest bounding box
                           which contains all the pages. This operation is always
                           done before any others (except '--absolutePreCrop').
                           The cropping is then done as usual, but note that any
                           margin percentages (such as for '--percentRetain') are
                           now relative to this new, possibly larger, page size.
                           The resulting pages are still cropped independently by
                           default, and will not necessarily all have the same
                           size unless '--uniform' is also selected to force the
                           cropping amounts to be the same for each page. If
                           pages are selected with '--pages' then this option is
                           only applied to those selected pages.

     -ms INT, --samePageSizeOrderStat INT
                           Choosing this option implies the '--samePageSize'
                           option, but the calculations for each edge of the
                           smallest bounding box ignore the largest (or smallest
                           for left and bottom edges) n values. The argument is
                           the nonnegative number n. Each edge is calculated
                           independently. This is an order statistic for
                           selecting the uniform size to make the pages. Note
                           that this will cut off parts of some pages if n>0.

     -e, --evenodd         Crop all the odd pages uniformly, and all the even
                           pages uniformly. The largest amount of cropping that
                           works for all the pages in each group is chosen. If
                           the '--uniform' ('-u') option is simultaneously set
                           then the vertical cropping will be uniform over all
                           the pages and only the horizontal cropping will differ
                           between even and odd pages.

     -g PAGESTR, -pg PAGESTR, --pages PAGESTR
                           Apply the cropping operation only to the selected
                           pages. The argument should be a list of the usual form
                           such as "2-4,5,9,20-30". The page-numbering is assumed
                           to start at 1. Ordering in the argument list is
                           unimportant, negative ranges are ignored, and pages
                           falling outside the document are ignored. Note that
                           restore information is always saved for all the pages
                           (in the ArtBox) unless '--noundosave' is selected.

     -t BYTEVAL, --threshold BYTEVAL
                           Set the threshold for determining what is background
                           space (white). The value can be from 0 to 255, with
                           191 the default (75 percent). This option may not be
                           available for some configurations since the PDF must
                           be internally rendered as an image of pixels. In
                           particular, it is ignored when '--gsBbox' is selected.
                           By default, any pixel value over 191 is considered to
                           be background (white).

     -nb INT, --numBlurs INT
                           When PDF files are explicitly rendered to image files,
                           apply a blur operation to the resulting images this
                           many times. This can be useful for noisy images.

     -ns INT, --numSmooths INT
                           When PDF files are explicitly rendered to image files,
                           apply a smoothing operation to the resulting images
                           this many times. This can be useful for noisy
                           images.

     -gs, --gsBbox         Use Ghostscript to find the bounding boxes for the
                           pages. The alternative is to explicitly render the PDF
                           pages to image files and calculate bounding boxes from
                           the images. This method tends to be much faster, but
                           it does not work with scanned PDF documents. It also
                           does not allow for choosing the threshold value,
                           applying blurs, etc. Any resolution options are passed
                           to the Ghostscript bbox device. This option requires
                           that Ghostscript be available in the PATH as
                           "gswin32c.exe" or "gswin64c.exe" on Windows, or as
                           "gs" on Linux. When this option is set the PIL image
                           library for Python is not required.

     -gsr, --gsRender      Use Ghostscript to render the PDF pages to images. By
                           default the pdftoppm program will be preferred for the
                           rendering, if it is found. Note that this option has
                           no effect if '--gsBbox' is chosen, since then no
                           explicit rendering is done.

     -x DPI, --resX DPI    The x-resolution in dots per inch to use when the
                           image is rendered to find the bounding boxes. The
                           default is 150. Higher values produce more precise
                           bounding boxes.

     -y DPI, --resY DPI    The y-resolution in dots per inch to use when the
                           image is rendered to find the bounding boxes. The
                           default is 150. Higher values produce more precise
                           bounding boxes.

     -b [m|c|t|a|b], --boxesToSet [m|c|t|a|b]
                           By default the pdfCropMargins program sets both the
                           MediaBox and the CropBox for each page of the cropped
                           PDF document to the new, cropped page size. This
                           default setting is usually sufficient, but this option
                           can be used to select different PDF boxes to set. The
                           option takes one argument, which is the first letter
                           (lowercase) of a type of box. The choices are MediaBox
                           (m), CropBox (c), TrimBox (t), ArtBox (a), and
                           BleedBox (b). This option overrides the default and
                           can be repeated multiple times to set several box
                           types.

     -f [m|c|t|a|b], --fullPageBox [m|c|t|a|b]
                           By default the program first (before any cropping is
                           calculated) sets the MediaBox and CropBox of each page
                           in (a copy of) the document to its MediaBox
                           intersected with its CropBox. This ensures that the
                           cropping is relative to the usual document-view in
                           programs like Acrobat Reader. This essentially defines
                           what is assumed to be the full size of pages in the
                           document, and all cropping is then performed relative
                           to that full-page size. This option can be used to
                           alternately use the MediaBox, the CropBox, the
                           TrimBox, the ArtBox, or the BleedBox in defining the
                           full-page size. The option takes one argument, which
                           is the first letter (lowercase) of the type of box to
                           use. If the option is repeated then the intersection
                           of all the box arguments is used. Only one choice is
                           allowed in combination with the '-gs' option since
                           Ghostscript does its own internal rendering when
                           finding bounding boxes. The default with '-gs' is the
                           CropBox.

     -r, --restore         By default, whenever this program crops a file for the
                           first time it saves the MediaBox intersected with the
                           CropBox as the new ArtBox (since the ArtBox is rarely
                           used). The Producer metadata is checked to see if this
                           was the first time. If so, the ArtBox for each page is
                           simply copied to the MediaBox and the CropBox for the
                           page. This restores the earlier view of the document,
                           such as in Acrobat Reader (but does not completely
                           restore the previous condition in cases where the
                           MediaBox and CropBox differed or the ArtBox had a
                           previous value). Options such as '-u' which do not
                           make sense in a restore operation are ignored. Note
                           that as far as default filenames the operation is
                           treated as just another crop operation (the default-
                           generated output filename still has a "_cropped.pdf"
                           suffix). The '--modifyOriginal' option (or its query
                           variant) can be used with this option.

     -A, --noundosave      Do not save any restore data in the ArtBox. This
                           option will need to be selected if the document
                           actually uses the ArtBox for anything important (which
                           is rare). Note that the '--restore' operation will not
                           work correctly for the cropped document if this option
                           is included in the cropping command. (The program does
                           not currently check for this when doing a restore.)

     -gsf, --gsFix         Attempt to repair the input PDF file with Ghostscript
                           before it is read-in with PyPdf. This requires that
                           Ghostscript be available. (See the general description
                           text above for the actual command that is run.) This
                           can also be used to automatically convert some
                           PostScript files (.ps) to PDF for cropping. The
                           repaired PDF is written to a temporary file; the
                           original PDF file is not modified. The original
                           filename is treated as usual as far as automatic name-
                           generation, the '--modify-original' option, and so
                           forth. This option is often helpful if the program
                           hangs or raises an error due to a corrupted PDF file.
                           Note that when re-cropping a file already cropped by
                           pdfCropMargins this option is probably not be
                           necessary, and if it is used in a re-crop (at least
                           with current versions of Ghostscript) it will reset
                           the Producer metadata which the pdfCropMargins program
                           uses to tell if the file was already cropped by the
                           program (the '--restore' option will then restore to
                           the previous cropping, not the original cropping). So
                           this option is not recommended as something to use by
                           default unless you encounter many corrupted PDF files
                           and do not need to restore back to the original
                           margins.

     -nc, --noclobber      Never overwrite an existing file as the output
                           file.

     -pv PROG, --preview PROG
                           Run a PDF viewer on the cropped PDF output. The viewer
                           process is run in the background. The viewer is
                           launched after pdfCropMargins has finished all the
                           other. The only exception is when the '--
                           queryModifyOriginal' option is also selected. In that
                           case the viewer is launched before the query so that
                           the user can look at the output before deciding
                           whether or not to modify the original. (Note that
                           answering 'y' will then move the file out from under
                           the running viewer; close and re-open the file before
                           adding annotations, highlighting, etc.) The single
                           argument should be the path of the executable file or
                           script to run the chosen viewer. The viewer is assumed
                           to take exactly one argument, a PDF filename. For
                           example, on Linux the Acrobat Reader could be chosen
                           with /usr/bin/acroread or, if it is in the PATH,
                           simply acroread. A shell script or batch file wrapper
                           can be used to set any additional options for the
                           viewer.

     -mo, --modifyOriginal
                           This option moves (renames) the original file to a
                           backup filename and then moves the cropped file to the
                           original filename. Thus it effectively modifies the
                           original file and makes a backup copy of the original,
                           unmodified file. The backup filename for the original
                           document is always generated from the original
                           filename; any prefix or suffix which would be added by
                           the program to generate a filename (by default a
                           "_cropped" suffix) is modified accordingly (by default
                           to "_uncropped"). The '--usePrefix', '--
                           stringUncropped', and '--stringSeparator' options can
                           all be used to customize the generated backup
                           filename. This operation is performed last, so if a
                           previous operation fails the original document will be
                           unchanged. Be warned that running pdfCropMargins twice
                           on the same source filename will modify the original
                           file; the '-noclobberOriginal' option can be used to
                           avoid this.

     -q, --queryModifyOriginal
                           This option selects the '--modifyOriginal' option, but
                           queries the user about whether to actually do the
                           final move operation. This works well with the '--
                           preview' option: if the preview looks good you can opt
                           to modify the original file (keeping a copy of the
                           original). If you decline then the files are not
                           swapped (and are just as if the '--modifyOriginal'
                           option had not been set).

     -nco, --noclobberOriginal
                           If the '--modifyOriginal' option is selected, do not
                           ever overwrite an existing file as the backup copy for
                           the original file. This essentially does the move
                           operations for the '--modifyOriginal' option in
                           noclobber mode, and prints a warning if it fails. On
                           failure the result is exactly as if the '--
                           modifyOriginal' option had not been selected. This
                           option is redundant if the ordinary '--noclobber'
                           option is also set.

     -pf, --usePrefix      Prepend a prefix-string when generating default file
                           names rather than appending a suffix-string. The same
                           string value is used, either the default or the one
                           set via the '--stringCropped' or '--stringUncropped'
                           option. With the default values for the other options
                           and no output file specified, this option causes the
                           cropped output for the input file "document.pdf" to be
                           written to the file named "cropped_document.pdf"
                           (instead of to the default filename
                           "document_cropped.pdf").

     -sc STR, --stringCropped STR
                           This option can be used to set the string which will
                           be appended (or prepended) to the document filename
                           when automatically generating the output filename for
                           a cropped file. The default value is "cropped".

     -su STR, --stringUncropped STR
                           This option can be used to set the string which will
                           be appended (or prepended) to the document filename
                           when automatically generating the output filename for
                           the original, uncropped file. The default value is
                           "uncropped".

     -ss STR, --stringSeparator STR
                           This option can be used to set the separator string
                           which will be used when appending or prependeding
                           string values to automatically generate filenames. The
                           default value is "_".

     -pw PASSWD, --password PASSWD
                           Specify a password to be used to decrypt an encrypted
                           PDF file. Note that decrypting with an empty password
                           is always tried, so this option is only needed for
                           non-empty passwords. The resulting cropped file will
                           not be encrypted, so use caution if important data is
                           involved.

     -spr FLOAT:FLOAT, --setPageRatios FLOAT:FLOAT
                           Force all the cropped page ratios to equal the set
                           ratio. All crops are calculated and applied as usual,
                           but either the left and right margins will be
                           increased equally or else the top and bottom margins
                           will be increased equally in order to make the ratio
                           of width to height equal the set value. Margins are
                           only ever increased. The format for the ratio is
                           either a string width-to-height ratio such as '4.5:3'
                           or else a floating point number like '0.75' which is
                           the width divided by the height.

     -dcb STR, --docCatBlacklist STR
                           Data associated with the full document, such as
                           outlines, bookmarks, and modes, is saved in the
                           document catalog of the PDF file. By default it is all
                           copied over to the cropped document if possible (with
                           some exceptions, e.g., "/Pages" will be modified by
                           cropping). If this is not wanted, or if it causes
                           problems for a document, this option specifies a
                           blacklist of document catalog items that will never be
                           copied. The arguments should be passed as a single,
                           quoted, whitespace-separated string, for example
                           "/Outlines /PageMode /OpenAction". The special value
                           "ALL" blacklists everything. An empty string
                           blacklists nothing, and is the default. As an example,
                           it can be useful to blacklist "/OpenAction" if on
                           opening the PDF it does something like zooming which
                           is not desired. Blacklisting `/PageMode` can also be
                           useful to, for example, not open the outline by
                           default. Running in verbose mode '-v' will show which
                           document catalog items are and are not being copied
                           for a document.

     -dcw STR, --docCatWhitelist STR
                           See the '--docCatBlacklist' option. This is just a
                           whitelist that essentially works the same way. The
                           whitelist takes precedence over the blacklist. It
                           specifies a list of items which will always be copied
                           over even if they are in the blacklist. Useful
                           combined with setting the blacklist to "ALL" if you
                           only want one or two of the items. The default value
                           is the empty string, which whitelists nothing and so
                           only the blacklist is used. Setting to "ALL"
                           guarantees that everything possible is copied over.

     -i, --showImages      When explicitly rendering PDF files to image files,
                           display the inverse image files that are used to find
                           the bounding boxes. Useful for debugging and for
                           choosing some of the other parameters (such as the
                           threshold). This option requires a default external
                           viewer program selected by the Pillow image
                           manipulation package (xv on Unix, and usually Paint on
                           Windows).

     -pdl, --pdftoppmLocal
                           Use a locally-packaged pdftoppm executable rather than
                           the system version. This option is only available on
                           Windows machines; it is ignored otherwise. By default
                           the first pdftoppm executable found in the directories
                           in the PATH environment variable is used. On Windows
                           the program will revert to this option if PDF image-
                           rendering is required and no system pdftoppm or
                           Ghostscript executable can be found. The locally-
                           packaged pdftoppm executable is a few years old, but
                           for page-cropping it only needs to get the margins
                           right.

     -gsp PATH, --ghostscriptPath PATH
                           Pass in a pathname to the ghostscript executable that
                           the program should use. No globbing is done. Useful
                           when the program is in a nonstandard location.

     -ppp PATH, --pdftoppmPath PATH
                           Pass in a pathname to the pdftoppm executable that the
                           program should use. No globbing is done. Useful when
                           the program is in a nonstandard location.


   The pdfCropMargins program is Copyright (c) 2014 by Allen Barker.  Released
   under the GNU GPL license, version 3 or later.

