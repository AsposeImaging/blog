---
title: Introduction to Aspose.CAD, Part 3
layout: post
tags: csharp, cad, convert, export
---

Most cases when <a href="https://products.aspose.com/cad/">Aspose.CAD</a> has to be used are probably associated with AutoCAD's files, such as DWG and DXF format. As such, I'm going to describe how can Aspose.CAD work with DWG and DXF formats.

Here are the previous articles:

<a href="https://dev.to/nnevod/introduction-to-asposecad-library-361h">Part 1</a>
<a href="https://dev.to/nnevod/introduction-to-asposecad-part-2-2kgf">Part 2</a>



Let's start from a simple and common thing: selection of particular layout or layer. Aspose.CAD supports enumeration of both layers and layouts and selection of one or several particular layers or layouts for export. Let's see!

## Getting a list of layout names
Take look at this snippet:
```csharp
            using (Aspose.CAD.Image image = Aspose.CAD.Image.Load(sourceFilePath))
            {
                Aspose.CAD.FileFormats.Cad.CadImage cadImage = (Aspose.CAD.FileFormats.Cad.CadImage)image;

                Aspose.CAD.FileFormats.Cad.CadLayoutDictionary layouts = cadImage.Layouts;
                foreach (Aspose.CAD.FileFormats.Cad.CadObjects.CadLayout layout in layouts.Values)
                {
                    Console.WriteLine("Layout " + layout.LayoutName);
                }
            }
```
What's happening there? When you load an DXF or DWG file, it is represented as an instance of a subclass of <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage">CadImage</a>, which has specific properties related to these file formats. As such, you cast your image to CadImage to get access to them.

Then there's <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage/properties/layouts">Layouts</a> property in CadImage, which is a specific <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadlayoutdictionary">dictionary</a> for <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadobjects/cadlayout">CadLayout</a> objects. Extract the CadLayout objects from it as from any <a href="https://msdn.microsoft.com/en-us/library/9dhwsays">IDictionary</a>, and here you have access to layout's properties, including its <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadobjects/cadlayout/properties/layoutname">name</a>.

## Selection of layout for export
This, like any setup of export process, is done via setting up an instance of <a href="https://apireference.aspose.com/net/cad/aspose.cad/imageoptionsbase">ImageOptionsBase</a>'s descendants, such as <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/bmpoptions">BmpOptions</a>, <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/pngoptions">PngOptions</a>, <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/pdfoptions">PdfOptions</a> etc. - let's just call them image export options, more specifically in this case, by setting up an instance of <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions">CadRasterizationOptions</a> which is assigned to image export options's <a href="https://apireference.aspose.com/net/cad/aspose.cad/imageoptionsbase/properties/vectorrasterizationoptions">VectorRasterizationOptions</a> property.
```csharp
            using (Aspose.CAD.Image image = Aspose.CAD.Image.Load("in.dwg"))
            {


                // Create an instance of PdfOptions
                Aspose.CAD.ImageOptions.PdfOptions pdfOptions = new Aspose.CAD.ImageOptions.PdfOptions();

                // Create an instance of CadRasterizationOptions and set its various properties
                Aspose.CAD.ImageOptions.CadRasterizationOptions rasterizationOptions = new Aspose.CAD.ImageOptions.CadRasterizationOptions();
                rasterizationOptions.PageWidth = 1600;
                rasterizationOptions.PageHeight = 1600;
                // Specify desired layout name
                rasterizationOptions.Layouts = new string[] { "Layout1" };


                // Set the VectorRasterizationOptions property
                pdfOptions.VectorRasterizationOptions = rasterizationOptions;
                

                image.Save("out.pdf", pdfOptions);                
            }
```
As you can see, the process is the same that is outlined in <a href="https://dev.to/nnevod/introduction-to-asposecad-library-361h">Part 1</a> of this article series, the only change is use of <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/layouts">Layouts</a> property. Setting it will result in export of only these layouts which names are specified in Layouts property. By default it will use Model layout, so if you wish to revert to that layout, just set the property to null.

## Getting a list of layer names
This is very similar to how it's done for layouts with only a small difference:
```csharp
            using (Aspose.CAD.Image image = Aspose.CAD.Image.Load(sourceFilePath))
            {
                Aspose.CAD.FileFormats.Cad.CadImage cadImage = (Aspose.CAD.FileFormats.Cad.CadImage)image;

                Aspose.CAD.FileFormats.Cad.CadLayersList layers = cadImage.Layers;
                foreach (string layerName in layers.GetLayersNames())
                {
                    Console.WriteLine("Layer " + layerName);
                }
            }
```
As you can see, the only difference is that <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage/properties/layers">Layers</a> property is not a Dictionary, but an <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadlayerslist">CadLayersList</a> and it allows getting a List of layer name as strings directly. 

## Selection of particular layer for export
Again, this is very similar to working with layouts.
```csharp


                // Create an instance of PdfOptions
                Aspose.CAD.ImageOptions.PdfOptions pdfOptions = new Aspose.CAD.ImageOptions.PdfOptions();

                // Create an instance of CadRasterizationOptions and set its various properties
                Aspose.CAD.ImageOptions.CadRasterizationOptions rasterizationOptions = new Aspose.CAD.ImageOptions.CadRasterizationOptions();
                rasterizationOptions.PageWidth = 1600;
                rasterizationOptions.PageHeight = 1600;
                // Specify desired layout name
				// Most of the CAD drawings have a layer by name 0, you may specify any name
                rasterizationOptions.Layers.Add("0");


                // Set the VectorRasterizationOptions property
                pdfOptions.VectorRasterizationOptions = rasterizationOptions;
                

                image.Save("out.pdf", pdfOptions);   
```
The difference is that <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/layers">Layers</a> property of CadRasterizationOptions is a <a href="http://msdn2.microsoft.com/en-us/library/6sh2ey19">List</a>, and by default, when the list is empty, all layers will be exported.

For now, that's all, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-cad">Aspose.CAD GitHub</a> page. There's also <a href="https://twitter.com/Asposecad">Twitter</a> and <a href="https://www.facebook.com/AsposeCAD">Facebook</a> pages for news on Aspose.CAD.
