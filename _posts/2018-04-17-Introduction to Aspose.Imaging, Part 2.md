---
title: Introduction to Aspose.Imaging, Part 2
layout: post
tags: csharp, raster, image, convert
---

In this part I continue to write about more generic features of <a href="https://products.aspose.com/imaging/">Aspose.Imaging</a>

<a href="https://dev.to/nnevod/introduction-to-asposeimaging-8jd">Part 1</a>


## Drawing an image onto another image
The aforementioned <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/graphics">Graphics</a> class also could be used to insert one <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image">Image</a> into another. It can be used to insert signature or watermark into image. This example shows how straightforward it is to do.
```csharp
            // Create an instance of image options and set its various properties
            JpegOptions imageOptions = new JpegOptions();

            // Create an instance of FileCreateSource and assign it to Source property
            imageOptions.Source = new FileCreateSource("Two_images_result_out.bmp", false);

            // Create an instance of Image and define canvas size
            using (var image = Image.Create(imageOptions, 600, 600))
            {
                // Create and initialize an instance of Graphics, Clear the image surface with white color 
                var graphics = new Graphics(image);
                graphics.Clear(Color.White);
				// And draw another images onto it
                graphics.DrawImage(Image.Load(dataDir + "sample_1.bmp"), 0, 0, 600, 300);
                graphics.DrawImage(Image.Load(dataDir + "File1.bmp"), 0, 300, 600, 300);
                image.Save();
            }
```

## Rotation of drawn objects
Whatever you are drawing onto image can be transposed, scaled and rotated, much like how it can be done in GDI+. This can be used for arbitrary drawing, for watermark insertion, etc.
For example, let's try to draw sample_1.bmp from previous example diagonally. Replace the line where it is inserted with the following lines:
```csharp
				// Get size of image we're inserting into
                SizeF sz = graphics.Image.Size;
                // Create an object of Matrix class for transformation
                Matrix matrix = new Matrix();

                // First a translation to center of image we're inserting into                
                matrix.Translate(sz.Width / 2, sz.Height / 2);
				// Then a rotation				
                matrix.Rotate(-45.0f);

                // Set the Transformation through Matrix
                graphics.Transform = matrix;
				
				//Insert the image to be rotated
                graphics.DrawImage(Image.Load(dataDir + "sample_1.bmp"), 0, 0, 600, 300);
				
				// Drop the transform so next inserted image will be inserted as is.
				graphics.Transform = null;
				
```
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging/matrix">Matrix</a> class is, again, an replacement for <a href="https://msdn.microsoft.com/en-us/library/system.drawing.drawing2d.matrix(v=vs.110).aspx">System.Drawing.Matrix</a> class.

## Multipage export in single image and multiple images
For cases when images of multipage formats, such as DjVu, are used as a source there is an option to specify which pages have to be exported. In case when output format also supports multiple images in one file, such as TIFF, one can specify several pages at once. Take a look at the example:
```csharp
            using (DjvuImage image = (DjvuImage)Image.Load( "Sample.djvu"))
            {
                // Create an instance of TiffOptions with preset options and IntRange and initialize it with range of pages to be exported
                TiffOptions exportOptions = new TiffOptions(TiffExpectedFormat.TiffDeflateBw);
				//Specify the pages to export
                IntRange range = new IntRange(0, 2);

                // Initialize an instance of DjvuMultiPageOptions while passing instance of IntRange and  Call Save method while passing instance of TiffOptions
                exportOptions.MultiPageOptions = new DjvuMultiPageOptions(range);
                image.Save(dataDir + "ConvertRangeOfDjVuPages_out.tif", exportOptions);
            }
```
The most important points here are <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/multipageoptions">MultiPageOptions</a> property of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions">TiffOptions</a> (<a href="https://apireference.aspose.com/net/imaging/aspose.imaging/imageoptionsbase">ImageOptionsBase</a>, actually, so they are valid for all formats), <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/djvumultipageoptions">DjvuMultiPageOptions</a> class used to set that property, and <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/intrange">IntRange</a> class, which is essentially a list of integers, used as list of page numbers there.
For cases when output format doesn't support multiple pages,  one has to use an IntRange instance with only one page specified, separately for each page.
```csharp
            using (DjvuImage image = (DjvuImage)Image.Load(dataDir + "Sample.djvu"))
            {
                // Create an instance of BmpOptions and Set BitsPerPixel for resultant images
                BmpOptions exportOptions = new BmpOptions();
                exportOptions.BitsPerPixel = 32;

                // Create an instance of IntRange and initialize it with range of pages to be exported
                IntRange range = new IntRange(0, 2);
                int counter = 0;
				
                // Save each page in separate file, as BMP do not support layering
                foreach (var i in range.Range)
                {
                    exportOptions.MultiPageOptions = new DjvuMultiPageOptions(range.GetArrayOneItemFromIndex(counter));
                    image.Save(dataDir + string.Format("{0}_out.bmp", counter++), exportOptions);
                }
            }
```
Here, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/intrange/methods/getarrayoneitemfromindex">GetArrayOneItemFromIndex</a> method of IntRange returns value from an element at specified index, wrapping it into an array. This is done for an example, as DjvuMultiPageOptions also has constructor that accepts a single Int32 value as a single page number.

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-imaging">Aspose.Imaging GitHub</a> page. There's also <a href="https://twitter.com/Asposeimaging">Twitter</a> and <a href="https://www.facebook.com/AsposeImaging">Facebook</a> pages for news on Aspose.Imaging.
