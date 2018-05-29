---
title: Introduction to Aspose.CAD, Part 5
layout: post
tags: charp, cad, convert, file
---

In this part I will again continue to describe <a href="https://products.aspose.com/cad/">Aspose.CAD</a> features specific to DWG\DXF files, as these file formats are the most common use for Aspose.CAD.


Here are the previous articles:

<a href="https://asposecad.github.io/Introduction-to-Aspose.CAD-library/">Part 1</a>

<a href="https://asposecad.github.io/Introduction-to-Aspose.CAD-Part-2/">Part 2</a>

<a href="https://asposecad.github.io/Introduction-to-Aspose.CAD-Part-3">Part 3</a>

<a href="https://asposecad.github.io/Introduction-to-Aspose.CAD-Part-4">Part 4</a>


## Export of 3D objects
To export 3D objects from DWG\DXF file, just set <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/typeofentities">TypeOfEntities</a> property of <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions">CadRasterizationOptions</a> instance  to <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/typeofentities">TypeOfEntities</a>.Entities3D and perform export:
```csharp
using (Aspose.CAD.Image cadImage = Aspose.CAD.Image.Load("source.dxf"))
{
    Aspose.CAD.ImageOptions.CadRasterizationOptions rasterizationOptions 
        = new Aspose.CAD.ImageOptions.CadRasterizationOptions();
    rasterizationOptions.PageWidth = 500;
    rasterizationOptions.PageHeight = 500;
    rasterizationOptions.TypeOfEntities = TypeOfEntities.Entities3D;
    
    
    PdfOptions pdfOptions = new PdfOptions();
    pdfOptions.VectorRasterizationOptions = rasterizationOptions;
   
    cadImage.Save("export3d.pdf", pdfOptions);
}
```

## Searching the text in a CAD file
This is a bit more complex task, as there are several kinds of entities that might contain text, including nested entities. Here is a code snippet that will show how to handle all of them:
```csharp

private static void IterateCADNodes(CadBaseEntity obj) 
{ 
  switch (obj.TypeName) { 
      case CadEntityTypeName.TEXT: 
          CadText childObjectText = (CadText) obj; 

          Console.WriteLine(childObjectText.DefaultValue); 

          break; 

      case CadEntityTypeName.MTEXT: 
          CadMText childObjectMText = (CadMText) obj; 

          Console.WriteLine(childObjectMText.Text); 

          break; 

      case CadEntityTypeName.INSERT: 
          CadInsertObject childInsertObject = (CadInsertObject) obj; 

          //Iterate over subnodes
          foreach (var tempobj in childInsertObject.ChildObjects) { 
              IterateCADNodes(tempobj); 
          } 
          break; 

      case CadEntityTypeName.ATTDEF: 
          CadAttDef attDef = (CadAttDef) obj; 

          Console.WriteLine(attDef.DefaultString); 
          break; 

      case CadEntityTypeName.ATTRIB: 
          CadAttrib attAttrib = (CadAttrib) obj; 

          Console.WriteLine(attAttrib.DefaultText); 
          break; 
  } 
}
```
And to iterate over both block and non-block entities, use this code:
```csharp
public static void SearchTextInDWGAutoCADFile(CadImage cadImage) 
{ 
  
   // Search for text in the entities section 
   foreach (var entity in cadImage.Entities) { 
       IterateCADNodes(entity); 
   } 

   // Search for text in the block section 
   foreach (CadBlockEntity blockEntity in cadImage.BlockEntities.Values) { 
       foreach (var entity in blockEntity.Entities) { 
           IterateCADNodes(entity); 
       } 
   } 
} 
```



For now, that's all, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-cad">Aspose.CAD GitHub</a> page. There's also <a href="https://twitter.com/Asposecad">Twitter</a> and <a href="https://www.facebook.com/AsposeCAD">Facebook</a> pages for news on Aspose.CAD.
