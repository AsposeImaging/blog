---
title: Clarifying image creation in Aspose.Imaging
layout: post
tags: csharp, raster, image, create
---

As it was stated in previous articles, <a href="https://products.aspose.com/imaging/">Aspose.Imaging</a> not only allows loading, manipulation and export of images, but it also allows creation of images from scratch. The creation process, however, might not be very obvious, so I'll describe it here in a few more words.

To create a new image in Aspose.Imaging, use <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image">Image</a>'s <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image/methods/create">Create</a> method. It accepts three parameters - an instance of a descendant of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/imageoptionsbase">ImageOptions</a> (i.e. <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/pngoptions">PngOptions</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/bmpoptions">BmpOptions</a> etc.), and width and height in pixels. The ImageOptions' descendant instance is the same that is ordinarily used to export image - as that ImageOptions specifies what's needed for a new image - pixel format, etc. In this case it would just produce clean white file, instead of filling it with content from another image. That's why width and height are specified separately - as exported image's dimensions are inferred from its content, and when creating new image, we have no content yet. Take a look at the example and don't skip the following paragraph!
```csharp
using (FileStream stream = new FileStream("newImage.png", FileMode.Create))
{
	// So we want to create a PNG image
	PngOptions saveOptions = new PngOptions();
	// Here you can set whatever properties of saveOptions you need

	// Set the Source for PngOptions and create an instance of Image
	saveOptions.Source = new StreamSource(stream);               
	using (Image image = Image.Create(saveOptions, 100, 100))
	{
		// Create and initialize an instance of Graphics class and clear Graphics surface
		Graphics graphic = new Graphics(image);
		graphic.Clear(Color.Yellow);

		// Draw an arc shape by specifying the Pen object having red black color and coordinates, height, width, 	start & end angles                 
		int width = 100;
		int height = 200;
		int startAngle = 45;
		int sweepAngle = 270;

		// Draw arc to screen and save all changes.
		graphic.DrawArc(new Pen(Color.Black), 0, 0, width, height, startAngle, sweepAngle);
		// Note you can just call Save() without parameters, as source is bound to image
		image.Save();
	}
	// Now close the stream.
	stream.Close();
}
```

Note that besides all the typical properties you specify when exporting an image, here we also specify <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/imageoptionsbase/properties/source">Source</a> property. That's how we specify where the newly created image should be stored and this property must be set with a <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/source">Source</a> created from a valid writable <a href="http://msdn2.microsoft.com/en-us/library/8f86tw9e">Stream</a> to create an image.

There is also a helper method that allows you to skip creating Stream by yourself, the <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.sources/filecreatesource/constructors/1">FileCreateSource</a> method in the <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.sources/filecreatesource">FileCreateSource</a> class. Take a look at using it:
```csharp
ImageOptions.Source = new FileCreateSource("myNewImage.png", false);
``` 
Note the second parameter, it allows creation of temporary files. If you set it to true, the file will be created as temporary one. Nonetheless, you should dispose the stream after you used it.

Note also how we can just apply changes to the image by calling parameterless <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image/methods/save">Save()</a> in the first example - this is exactly because Source is specified and image is bound to a file. This is in contrast to loading an existing image, which is virtually copied and has to be saved to an explicitly provided file.

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-imaging">Aspose.Imaging GitHub</a> page. There's also <a href="https://twitter.com/Asposeimaging">Twitter</a> and <a href="https://www.facebook.com/AsposeImaging-1702883649750052/">Facebook</a> pages for news on Aspose.Imaging.

