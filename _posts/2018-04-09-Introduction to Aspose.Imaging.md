---
title: Introduction to Aspose.Imaging
layout: post 
tags: csharp, image, raster, convert
---

<a href="https://products.aspose.com/imaging/">Aspose.Imaging</a> is a software library for <a href="https://products.aspose.com/imaging/net">.Net</a> and <a href="https://products.aspose.com/imaging/java">Java</a> that provides an API for conversion of raster images between different formats and also allows their modification and creation. There is also a <a href="https://products.aspose.com/imaging/sharepoint">version for Sharepoint</a>. It requires no additional software to work.

While Aspose.Imaging is a commercial library, it allows free evaluation, which offers all the functionality and can readily be used for experimenting, but only loads files of limited complexity and watermarks exported images. Besides paying upfront, one can use metered, pay-per-use license.

I'm going to write a series of articles covering key points of working with Aspose.Imaging using <a href="https://products.aspose.com/imaging/net">Aspose.Imaging for .NET</a>, starting from the most basic things such as loading an image and saving it to a different format to more specific image manipulations.

In this article I will describe the following operations that can be performed with Aspose.Imaging:

1. Simple loading and saving of an image
2. Creating image
3. Drawing on an image

## Simple loading and saving of an image
Take a look at the example code:
```csharp
            // 1 Load an image through file path location or stream
            using (Image image = Image.Load( "Sample1.png"))
            {
                //2 Create an instance of TiffOptions while specifying desired format Passsing TiffExpectedFormat.TiffJpegRGB will set the compression to Jpeg and BitsPerPixel according to the RGB color space.
                TiffOptions options = new TiffOptions(TiffExpectedFormat.TiffJpegRgb);
                //3 Save the image with specified options
                image.Save(dataDir + "SampleTiff_out.tiff", options);
            }
```

The process of loading and saving images consists of three steps, correspondingly numbered in comments in example:
1) Load the image
2) Set up processing options
3) Save the image using specified processing options.

Let's take a bit deeper look into the steps.
1) Loading is the simplest step, just call static <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image/methods/load/index">Load</a> method of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image">Image</a> class specifying path to your image file or a <a href="https://msdn.microsoft.com/en-us/library/system.io.stream(v=vs.110).aspx">Stream</a> of your image data. The image format will be determined automatically from its contents.
2) The main step where all your code will be. Let's skip to the simple third step and return here later.
3) Call <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/datastreamsupporter/methods/save/index">Save</a> method on your Image instance, specifying output path or Stream and processing options as in instance of a descendant of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/imageoptionsbase">ImageOptionsBase</a> class. The output image format will be determined from particular type of processing options object and particular format's settings will be derived from its' corresponding fields. As such, you see that everything is really done in step 2.

So, take a deeper dive into step 2.
There are several classes that descend from <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/imageoptionsbase">ImageOptionsBase</a> and can be used to set up output image format: <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/bmpoptions">BmpOptions</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/gifoptions">GifOptions</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/jpeg2000options">Jpeg2000Options</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/jpegoptions">JpegOptions</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/pngoptions">PngOptions</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/psdoptions">PsdOptions</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/svgoptions">SvgOptions</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/webpoptions">WebPOptions</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions">TiffOptions</a>. Each of these classes provides setup for saving image to corresponding format. Most of them are absolutely straightforward to use - just create an instance using parameterless constructor and you're done, any other setup is optional. Only two formats are different: with TiffOptions, you have to specify compression format and bit depth for constructor by providing a value from <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff.enums/tiffexpectedformat">TiffExpectedFormat</a> enum, and with SvgOptions you have to also set your SvgOptions instance's <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/imageoptionsbase/properties/vectorrasterizationoptions">VectorRasterizaionOptions</a> property with an instance of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/svgrasterizationoptions">SvgRasterizationOptions class and set that SvgRasterizationOptions instance's <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/vectorrasterizationoptions/properties/pageheight">PageHeight</a> and <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/vectorrasterizationoptions/properties/pagewidth">PageWidth</a> properties. As SVG is a vector format, it has concept of page dimension, not just a matrix of X*Y pixels. The raster image embedded into SVG page is then will be scaled during display to match specified page size.

That is enough for the basic loading and saving of an image, you don't need anything more. Just check out the complete example with specified namespaces etc. below.
```csharp
using System;
using System.Collections.Generic;
using System.Text;
using System.IO;
using Aspose.Imaging.FileFormats.Tiff;
using Aspose.Imaging.ImageOptions;
using Aspose.Imaging;
using Aspose.Imaging.FileFormats.Tiff.Enums;
using Aspose.Imaging.Sources;

namespace ImagingExample
{
    class Program
    {
        static void Main(string[] args)
        {
            using (Image image = Image.Load("Sample1.png"))
            {
                //2 Create an instance of TiffOptions while specifying desired format Passsing TiffExpectedFormat.TiffJpegRGB will set the compression to Jpeg and BitsPerPixel according to the RGB color space.
                TiffOptions options = new TiffOptions(TiffExpectedFormat.TiffJpegRgb);
                //3 Save the image with specified options
                image.Save( "SampleTiff_out.tiff", options);
            }
        }
    }
}



```
Just create a new console application project for .Net framework and replace the code in Program.cs with that one.

## Creating an image.
With Aspose.Imaging you can not only load an existing image, but also create it anew and fill it in several different ways. Take a look at the example:
```csharp
			// 1. Create image options
			BmpOptions ImageOptions = new BmpOptions();
            ImageOptions.BitsPerPixel = 24;

            // 2. Create an instance of FileCreateSource and assign it to Source property
            ImageOptions.Source = new FileCreateSource(dataDir + "DrawImagesUsingCoreFunctionality_out.bmp", false);

            // 3. Create an instance of RasterImage and Get the pixels of the image by specifying the area as image boundary
            using (RasterImage rasterImage = (RasterImage)Image.Create(ImageOptions, 500, 500))
            {
				//4. Create the contents of the image
                Color[] pixels = rasterImage.LoadPixels(rasterImage.Bounds);
                for (int index = 0; index < pixels.Length; index++)
                {
                    // Set the indexed pixel color to yellow
                    pixels[index] = Color.Yellow;
                }

                //4.5 Apply the pixel changes to the image 
                rasterImage.SavePixels(rasterImage.Bounds, pixels);
				//5. And save all changes.
                rasterImage.Save();
            }
```
Image creation begins from creating an instance of one of the aforementioned ImageOptionsBase descendant classes. 

Then you have to specify a file associated with image, as we don't load an image from existing source. 

In third step you create the <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage">RasterImage</a> instance using created options and specifying image width and height in pixels. RasterImage allows raster manipulation.

Fourth step is where you do any modification of image you want to do. In the example, a way to perform direct per-pixel color manipulation is shown. There is also another way, which I'll explain a bit later.

Step 4.5 is named so because calling <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage/methods/savepixels">SavePixels</a> is specific to such an approach to image manipulation, it directly writes the color array to image content, the "pixels" array is not the image content as is, but is a copy.

Finally, in step 5 image is saved. As image source had been provided with associated file path and the image is created with an option set from the start, there is no need to specify anything.

## Drawing on an image
Here is another way to manipulate the image's contents in Aspose.Imaging API. Let's modify the previous example:
```csharp
                using (Image image = Image.Create(saveOptions, 100, 100))
                {
                    // Create and initialize an instance of Graphics class and clear Graphics surface
                    Graphics graphic = new Graphics(image);
                    graphic.Clear(Color.Yellow);

                    // Initializes the instance of PEN class with black color and width
                    Pen BlackPen = new Pen(Color.Black, 3);
                    float startX = 10;
                    float startY = 25;
                    float controlX1 = 20;
                    float controlY1 = 5;
                    float controlX2 = 55;
                    float controlY2 = 10;
                    float endX = 90;
                    float endY = 25;

                    // Draw a Bezier shape by specifying the Pen object having black color and co-ordinate Points and save all changes.
                    graphic.DrawBezier(BlackPen, startX, startY, controlX1, controlY1, controlX2, controlY2, endX, endY);
                    image.Save();
                }
```
Here we see the use of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/graphics">Aspose.Imaging.Graphics</a> class. Its API is essentially the same as API of <a href="https://msdn.microsoft.com/en-us/library/system.drawing.graphics(v=vs.110).aspx">System.Drawing.Graphics</a> class and supports the same operations, so you'll feel yourself familiar with it. You can draw Bezier curves, lines, arcs, etc. this way. To save the image, just call Save on your image instance - Graphics works directly on image content so no need for additional save call.

That's all for now, stay tuned!
For more examples please visit the <a href="https://github.com/aspose-imaging">Aspose.Imaging GitHub</a> page. There's also <a href="https://twitter.com/AsposeImaging">Twitter</a> and <a href="https://www.facebook.com/AsposeImaging-1702883649750052">Facebook</a> pages for news on Aspose.Imaging.
