---
title: Introduction to Aspose.CAD library
layout: post
tags: csharp, java, cad, convert
---

<a href="https://products.aspose.com/cad">Aspose.CAD</a> is a standalone software library for .Net and Java platforms that reads CAD files such as DWG, DXF, DNG, IFC, STL files and can export their contents to PDF files and raster images. It requires no additional software to work - no need for AutoCAD or ReadDWG, it works by itself.

It also allows partial export of files,  exporting only specific entities or layers from DWG and DXF files, substituting colors or fonts specified in CAD files during export process, and export of 3D entities as well.

While Aspose.CAD is a commercial library, it allows free evaluation, which offers all the functionality and can readily be used for experimenting, but only loads files of limited complexity and watermarks exported images. Besides paying upfront, one can use metered, pay-per-use license.

I'm going to write a series of articles covering key points of working with Aspose.CAD using <a href="https://products.aspose.com/cad/net">Aspose.CAD for .NET</a>, starting from the most basic things such as loading CAD files and exporting images to more specific aspects.

So, let's begin with the most straightforward case: 

## Loading CAD file and exporting image

The process would consist of three main steps:
1) Load CAD file.
2) Set up exporting settings.
3) Save the image.

### Load CAD file
First step is very straightforward. Just call the static <a href="https://apireference.aspose.com/net/cad/aspose.cad.image/load/methods/2">Load</a> method of <a href="https://apireference.aspose.com/net/cad/aspose.cad/image">Image</a> class passing the file path to it:
```csharp

            string dwgPathToFile =  "files/cad.dwg";
            Image cadImage1 = Image.Load(dwgPathToFile);
```
The Load method will determine the file format from file extension and create corresponding object of Image's subclass. No hassle there!

Now I'll skip to the third step, as it's equally simple, and then return to the second step.

### Save the image
Call the <a href="https://apireference.aspose.com/net/cad/aspose.cad.image/save/methods/3">Save</a> method on your loaded Image, passing file path to save and <a href="https://apireference.aspose.com/net/cad/aspose.cad/imageoptionsbase">ImageOptionsBase</a> object and it's done.
```csharp
                cadImage1.Save("cad.pdf", pdfOptions);

```
The output image format will be determined from particular subclass of ImageOptionsBase, of which pdfOptions is an instance. There are separate classes for all output formats. Let's dive deeper in the second step.


### Set up exporting options
This is where the main work is done from library user's standpoint. You select the output format by creating an object of corresponding subclass of ImageOptionsBase.
There are the following subclasses of ImageOptionsBase:
<a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/pdfoptions">PdfOptions</a>, <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/bmpoptions">BmpOptions</a>, <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/gifoptions">GifOptions</a>, <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/jpegoptions">JpegOptions</a>,<a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/pngoptions">PngOptions</a>, <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/psdoptions">PsdOptions</a>, <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/tiffoptions">TiffOptions</a> and some <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/">more</a>. Each provides setup for corresponding output file format. Then set it up by setting its properties and then pass it to the third step. 
The most important property would be <a href="https://apireference.aspose.com/net/cad/aspose.cad/imageoptionsbase/properties/vectorrasterizationoptions">VectorRasterizationOptions</a> property, which we should set with an instance of <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions">CadRasterizationOptions</a> class. This class specifies how your CAD image should be rendered - width, height in pixels, wether CAD content should be centered, background color and override color for objects, etc. In its simplest form this step could be done the following way:
```csharp

                PdfOptions pdfOptions = new PdfOptions();
                CadRasterizationOptions rasterizationOptions= new CadRasterizationOptions();
                pdfOptions.VectorRasterizationOptions = rasterizationOptions;

                rasterizationOptions.PageHeight = 1600;
                rasterizationOptions.PageWidth = 1600;
                rasterizationOptions.DrawType = CadDrawTypeMode.UseObjectColor;
```
So we create an PdfOptions instance which obviously sets up the output into PDF file format, create new CadRasterizationOptions to set up how the CAD image should be rendered and set PdfOptions's VectorRasterizationOptions property with it, then specify output image height and width (
By default, your CAD image will be stretched/shrunk to fit to specified output page size, maintaining aspect ratio) and let the rasterization use colors specified in the CAD file.
After this setup, we're ready for step three.

Now you know how to export a CAD file to an raster image or PDF file using Aspose.CAD for .NET. <a href="https://products.aspose.com/cad/java">Java</a> version of Aspose.CAD is essentially identical as well.

Here's the complete example:
```csharp
using System;
using System.Collections.Generic;
using Aspose.CAD;
using Aspose.CAD.ImageOptions;

namespace ConsoleExampleAsposeCAD
{
    class Program
    {
        static void Main()
        {
            
            string dwgPathToFile =  "files/cad.dwg";
            Image cadImage1 = Image.Load(dwgPathToFile);

            PdfOptions pdfOptions = new PdfOptions();
            CadRasterizationOptions rasterizationOptions= new CadRasterizationOptions();
            pdfOptions.VectorRasterizationOptions = rasterizationOptions;

            rasterizationOptions.PageHeight = 1600;
            rasterizationOptions.PageWidth = 1600;
            rasterizationOptions.DrawType = CadDrawTypeMode.UseObjectColor;

            cadImage1.Save("cad.pdf", pdfOptions);            

           
        }
    }
}

```
Just create a new console application, copy the example content into Program.cs, install Aspose.CAD for .NET from NuGet and see for yourself. 


## Rotating and flipping image
With Aspose.CAD it's very easy to export CAD image not only as it is, but in rotated or mirriored form. Just get back to the second step of the first part, and tweak it a little:
```csharp

                PdfOptions pdfOptions = new PdfOptions();
                //Here it is, the rotation and flip!
                pdfOptions.Rotation = RotateFlipType.Rotate270FlipY;
                //^^^^
                CadRasterizationOptions rasterizationOptions= new CadRasterizationOptions();
                pdfOptions.VectorRasterizationOptions = rasterizationOptions;

                rasterizationOptions.PageHeight = 1600;
                rasterizationOptions.PageWidth = 1600;
                rasterizationOptions.DrawType = CadDrawTypeMode.UseObjectColor;
```
As seen, the <a href="https://apireference.aspose.com/net/cad/aspose.cad/imageoptionsbase/properties/rotation">Rotation</a> property accepts an value from <a href="https://apireference.aspose.com/net/cad/aspose.cad/rotatefliptype">RotateFlipType</a> enum. It lists all combinations of rotation in 90 degree steps and mirroring along X and Y axes -either or both at once. The Rotation property is availible for all subclasses of ImageOptionsBase, meaning it is supported for any output format.

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-cad">Aspose.CAD GitHub</a> page. There's also <a href="https://twitter.com/AsposeCAD">Twitter</a> and <a href="https://www.facebook.com/AsposeCAD">Facebook</a> pages for news on Aspose.CAD.
