---
title: Introduction to Aspose.CAD, Part 6
layout: post
tags: csharp, cad, convert
---
<a href="https://products.aspose.com/cad">Aspose.CAD</a> allows one to render a DWG file to raster image with a font other than specified in the file. 

Here are the previous articles:

<a href="https://dev.to/nnevod/introduction-to-asposecad-library-361h">Part 1</a>

<a href="https://dev.to/nnevod/introduction-to-asposecad-part-2-2kgf">Part 2</a>

<a href="https://dev.to/nnevod/introduction-to-asposecad-part-3-1mhf">Part 3</a>

<a href="https://dev.to/nnevod/introduction-to-asposecad-part-4-ma8">Part 4</a>

<a href="https://dev.to/nnevod/introduction-to-asposecad-part-5-4ff6">Part 5</a>



## Font substitution
Fonts for text in DWG files are specified in style table objects, represented in Aspose.CAD by a <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadtables/cadstyletableobject/">CadStyleTableObject</a> class. Collection of these objects are present in <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage/properties/styles">Styles</a> property of <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage">CadImage</a> instance. Styles have names, which can be used to differentiate them. There's an example on how to change font used by a class with particular name:

```csharp
using (Aspose.CAD.FileFormats.Cad.CadImage cadImage = (Aspose.CAD.FileFormats.Cad.CadImage)Aspose.CAD.Image.Load(sourceFilePath))
{
    // Iterate over the items of CadStyleDictionary
    foreach (CadStyleTableObject style in cadImage.Styles)
    {
        if (style.StyleName == "Roman")
        {
            // Specify the font for one particular style
            style.PrimaryFontName = "Arial";
        }
    }
}
```


For now, that's all, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-cad">Aspose.CAD GitHub</a> page. There's also <a href="https://twitter.com/Asposecad">Twitter</a> and <a href="https://www.facebook.com/AsposeCAD">Facebook</a> pages for news on Aspose.CAD.
