---
title: Converting RGB TIFF to CMYK TIFF with Aspose.Imaging 18.3
layout: post 
tags: csharp, image, processing, convert
---

In <a href="https://products.aspose.com/imaging/">Aspose.Imaging</a> 18.3 an new option has appeared which may be useful to some: an ability to convert from RGB color space to CMYK color space.
To save a image in CMYK color space, you have to use CMYK-capable file format (In the example, it's TIFF), set it up to use CMYK color space ( By creating <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions">TiffOptions</a> with a TiffLzwCmyk value of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff.enums/tiffexpectedformat">TiffExpectedFormat</a> enum in the example) and provide a color profile mapping to CMYK color space. 

Here is the complete example in C# using Aspose.Imaging for .NET 18.3:

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
            string sourceFilePath = "testTileDeflate.tif";
            string outputFilePath = "testTileDeflate Cmyk Icc.tif";
            string cmykProfilePath = "RSWOP.ICM";


            //Load the output color profile
            MemoryStream cmykProfile;
            using (FileStream stream = new FileStream(cmykProfilePath, FileMode.Open))
            {
                byte[] bytes = new byte[stream.Length];
                stream.Read(bytes, 0, (int)stream.Length);
                cmykProfile = new MemoryStream(bytes);
            }

            //Set up save file format parameters
            TiffOptions options = new TiffOptions(TiffExpectedFormat.TiffLzwCmyk);
            options.IccProfile = cmykProfile;


            //Load and save
            using (Image image = Image.Load(sourceFilePath))
            {
                image.Save(outputFilePath, options);
            }
        }
    }
}


```

As seen, conversion is very simple to perform using Aspose.Imaging.

For more examples please visit the <a href="https://github.com/aspose-imaging">Aspose.Imaging GitHub</a> page. There's also <a href="https://twitter.com/AsposeImaging">Twitter</a> and <a href="https://www.facebook.com/pg/AsposeImaging-1702883649750052">Facebook</a> pages for news on Aspose.Imaging.
