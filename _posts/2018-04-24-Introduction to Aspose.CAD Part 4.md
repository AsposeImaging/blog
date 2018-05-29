---
title: Introduction to Aspose.CAD, Part 4
layout: post
tags: csharp, cad, convert
---

In this part I will continue to describe <a href="https://products.aspose.com/cad/">Aspose.CAD</a> features specific to DWG\DXF files, as these file formats are the most common use for Aspose.CAD.


Here are the previous articles:

<a href="https://asposecad.github.io/Introduction-to-Aspose.CAD-library/">Part 1</a>

<a href="https://asposecad.github.io/Introduction-to-Aspose.CAD-Part-2/">Part 2</a>

<a href="https://asposecad.github.io/Introduction-to-Aspose.CAD-Part-3">Part 3</a>

<a href="https://asposecad.github.io/Introduction-to-Aspose.CAD-Part-4">Part 4</a>

<a href="https://asposecad.github.io/Introduction-to-Aspose.CAD-Part-5">Part 5</a>

## Iterating over entities and blocks
The DWG\DXF specific <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage">CadImage</a> class has an <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage/properties/entities">Entities</a> property, which contains all entities of the file. Blocks, which are specific for DWG format, are put as instances of <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadobjects/cadblockentity">CadBlockEntity</a> into a <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadblockdictionary">CadBlockDictionary</a> instance that is accessible via <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage/properties/blockentities">BlockEntities</a> property of a CadImage. Block names are used as keys in the dictionary, so blocks are accessible by their names. Entities of a specific block can be accessed via <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadobjects/cadblockentity/properties/entities">Entities</a> propery of CadBlockEntity.
```csharp
// Load an existing DWG file as CadImage.
using (Aspose.CAD.FileFormats.Cad.CadImage cadImage 
	= (Aspose.CAD.FileFormats.Cad.CadImage)Aspose.CAD.Image.Load(sourceFilePath))
{
  // Go through each entity inside the DWG/DXF file
    foreach (Aspose.CAD.FileFormats.Cad.CadObjects.CadBaseEntity entity in image.Entities)
    {
        Console.WriteLine(entity.TypeName);
    }

//Get information about the block by its name
    System.Console.WriteLine(cadImage.BlockEntities["*MODEL_SPACE"].Description);
}
```

## Getting underlay information
Underlays are stored like any other entity and are represented by <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadobjects/cadunderlay">CadUnderlay</a> class. As such, getting information about underlays is as simple as this:
```csharp
// Load an existing DWG file and convert it into CadImage 
using (Aspose.CAD.FileFormats.Cad.CadImage image 
		= (Aspose.CAD.FileFormats.Cad.CadImage)Image.Load(fileName))
{
    // Go through each entity inside the DWG file
    foreach (Aspose.CAD.FileFormats.Cad.CadObjects.CadBaseEntity entity in image.Entities)
    {
        // Check if entity is of CadDgnUnderlay type
        if (entity is Aspose.CAD.FileFormats.Cad.CadObjects.CadDgnUnderlay)
        {
            // Access different underlay flags 
            Aspose.CAD.FileFormats.Cad.CadObjects.CadUnderlay underlay 
				= entity as Aspose.CAD.FileFormats.Cad.CadObjects.CadUnderlay;
			//This is the external path to underlay file
            Console.WriteLine(underlay.UnderlayPath);
            Console.WriteLine(underlay.UnderlayName);
            Console.WriteLine(underlay.InsertionPoint.X);
            Console.WriteLine(underlay.InsertionPoint.Y);
            Console.WriteLine(underlay.RotationAngle);
            Console.WriteLine(underlay.ScaleX);
            Console.WriteLine(underlay.ScaleY);
            Console.WriteLine(underlay.ScaleZ);
			//Accessing the packed flags
            Console.WriteLine(
				(underlay.Flags & Aspose.CAD.FileFormats.Cad.CadObjects.UnderlayFlags.UnderlayIsOn)
				== Aspose.CAD.FileFormats.Cad.CadObjects.UnderlayFlags.UnderlayIsOn);
            Console.WriteLine(
				(underlay.Flags & Aspose.CAD.FileFormats.Cad.CadObjects.UnderlayFlags.ClippingIsOn)
				== Aspose.CAD.FileFormats.Cad.CadObjects.UnderlayFlags.ClippingIsOn);
            Console.WriteLine(
				(underlay.Flags & Aspose.CAD.FileFormats.Cad.CadObjects.UnderlayFlags.Monochrome)
				!= Aspose.CAD.FileFormats.Cad.CadObjects.UnderlayFlags.Monochrome);
            break;
        }
    }
}
```
The only more complex nuance is accessing the flags packed as bit fields, which is covered in this example.

## Associating block with layout
Association of a <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadobjects/cadblockentity">CadBlockEntity</a> with a layout is stored in an instance of <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadtables/cadblocktableobject">CadBlockTableObject</a>, which are listed under <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage/properties/blockstables">BlocksTables</a> property of a CadImage. The CadBlockTableObject has an <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadtables/cadblocktableobject/properties/hardpointertolayout">HardPointerToLayout</a> property, which contains value of a <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadobjects/cadlayout">Layout</a>'s <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadobjects/cadbase/properties/objecthandle">ObjectHandle</a> property (not an reference, just the same string value). Note that every object in a file, be it entity, table or block or object's <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadobjects/cadobjectattribute">attribute</a> has the ObjectHandle property. Another CadBlockTableObject's property, <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadtables/cadblocktableobject/properties/blockname">BlockName</a> contains block's name, which can be used to get block from BlockEntities of a CadImage.
```csharp
foreach (Aspose.CAD.FileFormats.Cad.CadObjects.CadLayout layout in layouts.Values)
{
    layoutNames[i++] = layout.LayoutName;
    System.Console.WriteLine("Layout " + layout.LayoutName + " is found");

    // Find block, applicable for DWG only
    Aspose.CAD.FileFormats.Cad.CadTables.CadBlockTableObject blockTableObjectReference
     = null;
    foreach (Aspose.CAD.FileFormats.Cad.CadTables.CadBlockTableObject tableObject
				in cadImage.BlocksTables)
    {
        if (string.Equals(tableObject.HardPointerToLayout, layout.ObjectHandle))
        {
            blockTableObjectReference = tableObject;
            break;
        }
    }

     // Collection cadBlockEntity.Entities contains information
	//about all entities on specific layout if this
	//collection has no elements it means layout is a copy
	// of Model layout and contains the same entities
    Aspose.CAD.FileFormats.Cad.CadObjects.CadBlockEntity cadBlockEntity = 
	cadImage.BlockEntities[blockTableObjectReference.BlockName];
}
```


For now, that's all, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-cad">Aspose.CAD GitHub</a> page. There's also <a href="https://twitter.com/Asposecad">Twitter</a> and <a href="https://www.facebook.com/AsposeCAD">Facebook</a> pages for news on Aspose.CAD.
