= PDF To Image =

The PDF To Image module provides automatic conversion of
uploaded PDF files to images.
It can be used either to create a snapshot of the front page,
or to generate a gallery of images from each page in the document.

The module is implemented as field widget where PDFs are uploaded to.
It places generated images into a Image Field on the same content type.

== Requirements ==

  File field, Image field and Imagemagick modules.
  ImageMagick toolkit to be available on server via command-line interface.

== Installation ==

Once the module is enabled, check your site status at /admin/reports/status
You should see a message that will tell you if your system is ready to run.
If not, you need to check the requirements, and the ImageMagick settings
at admin/settings/imagmagick.
See the ImageMagick project docs for troubleshooting that.

If ImageMagick appears to be available but still does not convert PDFs, it
could be it wasn't installed with 'Ghostscript' libraries or other required
dependencies. You'l have to go to the ImageMagick forums for help with that.

== Configuration ==

- First, add an image field on your chosen content type. This is where the
  generated images wil be stored.
- Set the allowed fields to 1 if you just want a cover page, direct number to
  store only some count of pages or 'unlimited' if you want all pages
  to be generated.
- Next add a filefield to your chosen content type and choose
  'pdf_to_image' as the renderer.
- When configuring this field, you will be required to link the uploaded file
  field with the target image field.
- You can add imagecache handling to the image field rendering as normal to
  adjust the size of the results and how they display on the page.

== Processing ==

Processing of PDF may take some time.
Larger documents use the 'batch' process to generate each page.

== Use of ImageMagick ==

Actual processing is perfomed using ImageAPI's ImageMagick toolkit.
GD is not supported.

=== Dev notes ===

The only candidate function from ImageAPI which may perform PDF to image
convertion is _imageapi_imagemagick_convert(). Unfortunately, it can't pass
arguments to 'convert' tool of ImageMagick *before* source file specification,
which is needed to change default density of 100x100dpi.
On this reason, a custom functionis included in the module's source:

function pdf_to_image_generate_page($params, $page_number = 0) {
  . . .
}
