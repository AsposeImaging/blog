---
title: New option to set default line caps for Aspose.CAD for .NET
layout: post
tags: csharp,cad,convert
---

Aspose.CAD for .NET is a library that  allows conversion of CAD files, for example, AutoCAD DWG and DXF files, to PDF and Raster images, without use of AutoCAD or any other software.

The 18.2 version of Aspose.CAD for .NET, due to be released on 27 February, 2018, allows setting default line cap style for lines with undefined line cap styles, which makes exported images look better in some cases.

Here is the example code to load a DWG file and save it as PDF with default line cap style set to Flat.


```csharp
using System;
using System.Drawing.Drawing2D;
using Aspose.CAD;
using Aspose.CAD.FileFormats.Cad;
using Aspose.CAD.ImageOptions;

namespace LineCapDemo
{
    class Program
    {
        static void Main()
        {
            string fileName = "TestFile.dwg"; 
            try 
            { 
                CadImage cadImage = (CadImage)Image.Load(fileName); 
                CadRasterizationOptions rasterizationOptions = new CadRasterizationOptions(); 
                rasterizationOptions.PageWidth = cadImage.Width * 100; 
                rasterizationOptions.PageHeight = cadImage.Height * 100; 

                PdfOptions pdfOptions = new PdfOptions(); 
				
				//This is the new option for setting default line 
                //cap style added in Aspose.CAD for .NET 18.2 
                rasterizationOptions.PenOptions = new PenOptions 
                { 
                    StartCap = LineCap.Flat, 
                    EndCap = LineCap.Flat 
                }; 
				
                pdfOptions.VectorRasterizationOptions = rasterizationOptions; 
                string outPath1 = fileName + "_flat" + ".pdf"; 
                Console.WriteLine("output path: " + outPath1); 
                cadImage.Save(outPath1, pdfOptions); 
            } 
            catch (Exception ex) 
            { 
                Console.WriteLine(ex.Message); 
            } 
        }
    }
}

```

StartCap and EndCap are of System.Drawing.Drawing2D.LineCap Enum type.

Default line cap styles are supported for all output file formats, namely PDF, PNG, BMP, GIF, JPEG2000, JPEG, PSD, TIFF, WMF. If you won't specify a default cap style, the system will use its own default line cap style, which is different for different cases.

The complete Aspose.CAD for .NET API reference is located <a href="https://apireference.aspose.com/net/cad/">here</a>.
