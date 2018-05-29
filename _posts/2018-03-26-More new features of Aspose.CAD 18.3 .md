---
title: More new features of Aspose.CAD 18.3 
layout: post
tags: csharp, cad, convert
---


Besides two new features I've already covered in articles, such as: <a href="https://dev.to/nnevod/new-option-to-set-default-line-caps-for-asposecad-for-net--4a0l">New option to set default line caps</a> and <a href="https://dev.to/nnevod/exporting-particular-entities-from-cad-files-using-asposecad-for-net--13bn">Exporting particular entities from CAD files</a>, Aspose.CAD 18.3 also has the following new features:

Support for saving into DXF file.
Support for adding raster image during export of DWG image
Support for adding text during export of DWG image.

## Support for saving into DXF file:
This is simple, just load DXF file, modify the loaded <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage">CadImage</a> object and call <a href="https://apireference.aspose.com/net/cad/aspose.cad.datastreamsupporter/save/methods/2">Save</a> on it with output file name with DXF extension. (File format is selected automatically based on extension both for loading and saving).
```csharp
using System;
using System.Collections.Generic;
using Aspose.CAD;
using Aspose.CAD.FileFormats.Cad;
using Aspose.CAD.FileFormats.Cad.CadConsts;
using Aspose.CAD.FileFormats.Cad.CadObjects;
using Aspose.CAD.ImageOptions;

namespace DxfDemo
{
    class Program
    {
        static void Main()
        {
            string sourceFilePath =  "example.dxf";
            using (CadImage cadImage = (CadImage)Image.Load(sourceFilePath))
            {
                // do any updates to cadImage there

                //Save to resulting DXF
                cadImage.Save("modified.dxf");
            }
        }
    }
}
```
That's it.


## Support for adding raster image during export of DWG image:
You can add a raster image into an already loaded DWG/DXF image (i.e. a <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage">CadImage</a> object) and then export the whole image as raster or PDF:
```csharp
using System;
using System.Collections.Generic;
using Aspose.CAD;
using Aspose.CAD.FileFormats.Cad;
using Aspose.CAD.FileFormats.Cad.CadConsts;
using Aspose.CAD.FileFormats.Cad.CadObjects;
using Aspose.CAD.ImageOptions;

namespace DxfDemo
{
    class Program
    {
        static void Main()
        {
            string dwgPathToFile =  "Drawing11.dwg";
            CadImage cadImage1 = (CadImage)Image.Load(dwgPathToFile);
            // using (Image image = ImageLoader.Load(dwgPathToFile))
            {
                //Setup image data
                CadRasterImageDef cadRasterImageDef = new CadRasterImageDef();
                cadRasterImageDef.ObjectHandle = "A3B4";
                //Raster image file has to be in the same folder as DWG
                cadRasterImageDef.FileName = "road-sign-custom.png";

                //Setup location of image being added within the CAD image 
                CadRasterImage cadRasterImage = new CadRasterImage();
                cadRasterImage.ImageDefReference = "A3B4";
                cadRasterImage.InsertionPoint.X = 26.77;
                cadRasterImage.InsertionPoint.Y = 22.35;
                cadRasterImage.DisplayFlags = 7;
                cadRasterImage.ImageSizeU = 640;
                cadRasterImage.ImageSizeV = 562;
                cadRasterImage.UVector.X = 0.0061565450840500831;
                cadRasterImage.UVector.Y = 0;
                cadRasterImage.VVector.X = 0;
                cadRasterImage.VVector.Y = 0.0061565450840500822;
                cadRasterImage.ClippingState = 0;
                cadRasterImage.ClipBoundaryVertexList.Add(new Cad2DPoint(-0.5, 0.5));
                cadRasterImage.ClipBoundaryVertexList.Add(new Cad2DPoint(639.5, 561.5));

                //Add image description entity to CAD image
                CadImage cadImage = (CadImage)cadImage1;
                cadImage.BlockEntities["*Model_Space"].AddEntity(cadRasterImage);

                //Add image data (raster itself) to CAD image
                List<CadBaseObject> list = new List<CadBaseObject>(cadImage.Objects);
                list.Add(cadRasterImageDef);
                cadImage.Objects = list.ToArray();

                //Export the modified image
                PdfOptions pdfOptions = new PdfOptions();
                CadRasterizationOptions cadRasterizationOptions = new CadRasterizationOptions();
                pdfOptions.VectorRasterizationOptions = cadRasterizationOptions;
                cadRasterizationOptions.DrawType = CadDrawTypeMode.UseObjectColor;
                cadRasterizationOptions.CenterDrawing = true;
                cadRasterizationOptions.PageHeight = 1600;
                cadRasterizationOptions.PageWidth = 1600;
                cadRasterizationOptions.Layouts = new string[] { "Model" };
                cadImage1.Save("export2.pdf", pdfOptions);
            }
        }
    }
}
```

## Support for adding text during export of DWG image:
Likewise, you can add text into loaded image for it to appear in exported raster or PDF image. It's a bit simplier to do as you don't have to provide data and metadata separately:
```csharp
using System;
using System.Collections.Generic;
using Aspose.CAD;
using Aspose.CAD.FileFormats.Cad;
using Aspose.CAD.FileFormats.Cad.CadConsts;
using Aspose.CAD.FileFormats.Cad.CadObjects;
using Aspose.CAD.ImageOptions;

namespace DxfDemo
{
    class Program
    {
        static void Main()
        {
            string dwgPathToFile =  "Drawing11.dwg";
            CadImage cadImage1 = (CadImage)Image.Load(dwgPathToFile);
            // using (Image image = ImageLoader.Load(dwgPathToFile))
            {
                //Only have to setup the text entity
                CadText cadText = new CadText();
                cadText.StyleType = "Standard";
                cadText.DefaultValue = "Some custom text";
                cadText.ColorId = 256;
                cadText.LayerName = "0";
                cadText.FirstAlignment.X = 47.90;
                cadText.FirstAlignment.Y = 5.56;
                cadText.TextHeight = 0.8;
                cadText.ScaleX = 0.0;

                //Add text to model space block
                CadImage cadImage = (CadImage)cadImage1;
                cadImage.BlockEntities["*Model_Space"].AddEntity(cadText);

                //Export modified image
                PdfOptions pdfOptions = new PdfOptions();
                CadRasterizationOptions cadRasterizationOptions = new CadRasterizationOptions();
                pdfOptions.VectorRasterizationOptions = cadRasterizationOptions;
                cadRasterizationOptions.DrawType = CadDrawTypeMode.UseObjectColor;
                cadRasterizationOptions.CenterDrawing = true;
                cadRasterizationOptions.PageHeight = 1600;
                cadRasterizationOptions.PageWidth = 1600;
                cadRasterizationOptions.Layouts = new string[] { "Model" };
                cadImage1.Save("Drawing11_with_text.pdf", pdfOptions);
            }
        }
    }
}
```

That's it!

For more examples please visit the <a href="https://github.com/aspose-cad">Aspose.CAD GitHub</a> page. There's also <a href="https://twitter.com/AsposeCAD">Twitter</a> and <a href="https://www.facebook.com/AsposeCAD">Facebook</a> pages for news on Aspose.CAD.

<a href="https://products.aspose.com/cad">And this is the main Aspose.CAD product page.</a>
