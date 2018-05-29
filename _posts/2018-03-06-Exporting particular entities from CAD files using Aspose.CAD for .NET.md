---
title: Exporting particular entities from CAD files using Aspose.CAD for .NET
layout: post
tags: csharp, cad, convert
---

Sometimes need arise to export only some particular entities from a CAD file. It may not be obvious how to do this. However, in reality it is a very simple thing to do with Aspose.CAD for .NET. The method described in this article works for DWG, DXF and STL files.

For these files, a loaded file <a href="https://apireference.aspose.com/net/cad/aspose.cad/image">Image</a> is actually a descendant or an instance of <a href=""https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage>CadImage</a> class, which has <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage/properties/entities">Entities</a> property. This property is an array containing all entities from a file in form of <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadobjects/cadbaseentity">CadBaseEntity</a> class instances. Exporting particular entities is as simple as retrieving this array, picking any entities you need, putting them into another array and setting <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage/properties/entities">Entities</a> property back with this array.

Here is an example where only TEXT entities are left to display:

```csharp
using System;
using System.Collections.Generic;
using Aspose.CAD;
using Aspose.CAD.FileFormats.Cad;
using Aspose.CAD.FileFormats.Cad.CadConsts;
using Aspose.CAD.FileFormats.Cad.CadObjects;
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
                //Casting loaded Image to CadImage
                //as plain Image doesn't have Entities property
                CadImage cadImage = (CadImage)Image.Load(fileName); 
                
                CadRasterizationOptions rasterizationOptions = new CadRasterizationOptions(); 
                rasterizationOptions.PageWidth = cadImage.Width * 100; 
                rasterizationOptions.PageHeight = cadImage.Height * 100; 

                PdfOptions pdfOptions = new PdfOptions();

                //Retrieving all entities
                CadBaseEntity[] entities = cadImage.Entities;
                List<CadBaseEntity> filteredEntities = new List<CadBaseEntity>();

                foreach (CadBaseEntity entity in entities)
                {
                    //selecting only entities of TEXT type
                    if(entity.TypeName == CadEntityTypeName.TEXT)
                        filteredEntities.Add(entity);
                }
                
                //Making array of filtered entities
                entities = filteredEntities.ToArray();
                //Now only filtered entities are left in image
                cadImage.Entities = entities;
                
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

As you see, you can filter the entities whatever way you want, just assign whatever you selected back to <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage/properties/entities">Entities</a> property and it's done.
