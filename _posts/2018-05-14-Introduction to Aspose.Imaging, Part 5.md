---
title: Introduction to Aspose.Imaging, Part 5
layout: post
tags: csharp, image, raster, convert
---

In this article on <a href="https://products.aspose.com/imaging/">Aspose.Imaging</a>, I'll, tell about use of automatic grayscaling feature, which I've forgot to include in previous articles, and most importantly, how to work with XMP metadata in Aspose.Imaging.

Here are the previous articles:

<a href="https://asposeimaging.github.io/Introduction-to-Aspose.Imaging/">Part 1</a>

<a href="https://asposeimaging.github.io/Introduction-to-Aspose.Imaging,-Part-2/">Part 2</a>

<a href="https://asposeimaging.github.io/Introduction-to-Aspose.Imaging,-Part-3/">Part 3</a>

<a href="https://asposeimaging.github.io/Introduction-to-Aspose.Imaging,-Part-4/">Part 4</a>


## Grayscaling
This is as simple as possible, any descendant of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage">RasterImage</a> has an <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage/methods/grayscale">Grayscale</a> method, which turns image to grayscale, obviously.
```csharp
// Load an image in an instance of Image
using (Image image = Image.Load("aspose-logo.jpg"))
{
    // Cast the image to RasterCachedImage and Check if image is cached
    RasterCachedImage rasterCachedImage = (RasterCachedImage)image;               
    if (!rasterCachedImage.IsCached)
    {
        // Cache image if not already cached
        rasterCachedImage.CacheData();
    }

    // Transform image to its grayscale representation and Save the resultant image
    rasterCachedImage.Grayscale();
    rasterCachedImage.Save("Grayscaling_out.jpg");
}
```

## XMP metadata
XMP metadata as represented in Aspose.Imaging is organised as several "packets" that are contained within XMP packet wrapper. Packets correspond to use cases and are represented by parent <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp/xmppackage">XmpPackage</a> class and descendant classes for these cases: <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp.schemas.dublincore/dublincorepackage">DublinCorePackage</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp.schemas.pdf/pdfpackage">PdfPackage</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp.schemas.photoshop/photoshoppackage">PhotoshopPackage</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp.schemas.xmpbaseschema/xmpbasicpackage">XmpBasicPackage</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp.schemas.xmpdm/xmpdynamicmediapackage">XmpDynamicMediaPackage</a>, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp.schemas.xmpmm/xmpmediamanagementpackage">XmpMediaManagementPackage</a> and <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp.schemas.xmprm/xmprightsmanagementpackage">XmpRightsManagementPackage</a>. Each of them has properties corresponding to its use case. 

Packet wrapper, i.e. the top-level metadata object, is represented by <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp/xmppacketwrapper">XmpPacketWrapper</a> class. Packages are appended to it via <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp/xmppacketwrapper/methods/addpackage">AddPackage</a> method of XmpPacketWrapper's instance. Package's instance has to be created and its properties set up before addition to XmpPacketWrapper.
The XmpPacketWrapper itself is created using instances of three other classes: <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp/xmpheaderpi">XmpHeaderPi</a>, which contains GUID for this metadata collection, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp/xmpmeta">XmpMeta</a>, which contains generic metadata and is optional, and <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp/xmptrailerpi">XmpTrailerPi</a>, which contains the flag allowing or disallowing modification of XMP data for subsequent processors.
Instance of XmpPacketWrapper is then put into <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage/properties/xmpdata">XmpData</a> property of a instance of a descendant of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage">RasterImage</a> class.
Conversely, to read metadata, get XmpPacketWrapper instance from XmpData property of an loaded image, and to get packages, use <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.xmp/xmppacketwrapper/properties/packages">Packages</a> property of the instance.
Here is an example of creating XMP metadata and embedding it into a TIFF file:
```csharp          

// Specify the size of image by defining a Rectangle 
Rectangle rect = new Rectangle(0, 0, 100, 200);
TiffOptions tiffOptions = new TiffOptions(TiffExpectedFormat.TiffJpegRgb);
tiffOptions.Photometric = TiffPhotometrics.MinIsBlack;
tiffOptions.BitsPerSample = new ushort[] { 8 };

// Create the brand new image just for sample purposes
using (var image = new TiffImage(new TiffFrame(tiffOptions, rect.Width, rect.Height)))
{
    // Create an instance of XMP-Header
    XmpHeaderPi xmpHeader = new XmpHeaderPi(Guid.NewGuid().ToString());

    // Create an instance of Xmp-TrailerPi, XMPmeta class to set different attributes
    XmpTrailerPi xmpTrailer = new XmpTrailerPi(true);
    XmpMeta xmpMeta = new XmpMeta();
    xmpMeta.AddAttribute("Author", "Mr Smith");
    xmpMeta.AddAttribute("Description", "The fake metadata value");

    // Create an instance of XmpPacketWrapper that contains all metadata
    XmpPacketWrapper xmpData = new XmpPacketWrapper(xmpHeader,xmpTrailer, xmpMeta);

    // Create an instacne of Photoshop package and set photoshop attributes
    PhotoshopPackage photoshopPackage =  new PhotoshopPackage();
    photoshopPackage.SetCity("London");
    photoshopPackage.SetCountry("England");
    photoshopPackage.SetColorMode(ColorMode.Rgb);
    photoshopPackage.SetCreatedDate(DateTime.UtcNow);

    // Add photoshop package into XMP metadata
    xmpData.AddPackage(photoshopPackage);

    // Create an instacne of DublinCore package and set dublinCore attributes
    DublinCorePackage dublinCorePackage = new DublinCorePackage();
    dublinCorePackage.SetAuthor("Charles Bukowski");
    dublinCorePackage.SetTitle("Confessions of a Man Insane Enough to Live With the Beasts");
    dublinCorePackage.AddValue("dc:movie", "Barfly");

    // Add dublinCore Package into XMP metadata
    xmpData.AddPackage(dublinCorePackage);
	
	//Add metadata to image and save
	image.XmpData = xmpData;
    image.Save("XmpSample.tiff");
}
```
Now, to read the created data, see the next example:
```csharp
using (var img = (TiffImage)Image.Load("XmpSample.tiff"))
{
    // Getting the XMP metadata
    XmpPacketWrapper imgXmpData = img.XmpData;
	
	
    foreach (XmpPackage package in imgXmpData.Packages)
    {
        // Use package data ...
    }
}
```

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-imaging">Aspose.Imaging GitHub</a> page. There's also <a href="https://twitter.com/Asposeimaging">Twitter</a> and <a href="https://www.facebook.com/AsposeImaging">Facebook</a> pages for news on Aspose.Imaging.
