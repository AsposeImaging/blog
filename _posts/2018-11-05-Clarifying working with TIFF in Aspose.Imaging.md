---
title: Clarifying working with TIFF in Aspose.Imaging
layout: post
tags: csharp, tiff, image, create
---

<a href="">Aspose.Imaging</a> provides support for <a href="">TIFF</a> format, which can be considered a "classic" format for raster data interchange originally used for scanning and faxing and is now used in many polygraphic, scientific and in general image processing applications that require higher precision and adjustability than common image formats like JPEG and PNG that are more end-user oriented.

Given the flexibility of TIFF format, it might not be obvious how to properly work with this format in general and in Aspose.Imaging as well. As such, this article will explain some basic concepts of TIFF and how to work with it in Aspose.Imaging. There will be a further article that will dive deeper on options availible for TIFF in Aspose.Imaging.

## Multiple images in one file

Unlike most other raster image formats, TIFF is essentially a container for multiple separate raster images, with each image having its own set of header tags, that describe different properties of image: size, definition, compression, data arrangement - pixel order, byte order, bit per pixel/channel count, etc. 
As such, in Aspose.Imaging, these contained images are represented by a specific <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffframe">TiffFrame</a> class, that exposes most of the TIFF-specific properties. <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffimage/">TiffImage</a> class contains an array of <a href="">TiffFrame</a> instances in <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffimage/properties/frames">Frames</a> property. The TiffImage class also has to expose some of these properties that are common for all raster formats, such as size, but they are mapped to <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffimage/properties/activeframe">ActiveFrame</a>, one of TiffFrame instances in the Frames property that is selected as active, by default it is the first element of Frames array. Same happens when you access TiffImage instance as <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage">RasterImage</a> or <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image">Image</a>.
As such, in Aspose.Imaging, TiffFrame is what you have to work with if you really want to work with TIFF-specific properties, while TiffImage is just a collection with some compatibility measures. You can work on it like on other Image's subclasses, but select specific ActiveFrame you want to work on first.
Example on using TiffFrame and ActiveFrame:

```csharp
// Create instances of FileStream and initialize with Tiff images
FileStream fileStream = new FileStream("TestDemo.tif", FileMode.Open);
FileStream fileStream1 = new FileStream("sample.tif", FileMode.Open);

// Create an instance of TiffImage and load the destination image from filestream
using (TiffImage image = (TiffImage) Image.Load(fileStream))
{
    // Create an instance of TiffImage and load the source image from filestream
    using (TiffImage image1 = (TiffImage) Image.Load(fileStream1))
    {
        // Create an instance of TIffFrame and copy active frame of source image
        TiffFrame frame = TiffFrame.CopyFrame(image1.ActiveFrame);

        // Add copied frame to destination image
        image.AddFrame(frame);
    }

    // Save the image with changes
    image.Save("ConcatenatingTIFFImagesfromStream_out.tif");

    // Close the FileStreams
    fileStream.Close();
    fileStream1.Close();
}
```

## Creating TiffImage and TiffFrame

As such, TiffImage is created in a different way from other RasterImage's. You either create an empty TiffImage with parameterless constructor, or a TiffImage with single contained raster by constructor that accepts single TiffFrame, or a TiffImage with multiple rasters by constructor that accepts array of TiffFrame. Actual options, like pixel format, image size etc. are reserved for TiffFrame.

TiffFrame, being a real image, is created more like other RasterImage descendants: you can create an new one from existing image (by path to image file, by stream of image file, or already loaded RasterImage instance), you can specify <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/">TiffOptions</a> and dimensions to create a completely new empty image, and create new image from existing one while at once specifying TiffOptions (also with three variants of how to specify the source), so that you can load your image raster data into desired format of TIFF.

```csharp
// Create an instance of TiffOptions and set its various properties
// The options themselves will be described in following article on TiffOptions
TiffOptions options = new TiffOptions(TiffExpectedFormat.Default);
options.BitsPerSample = new ushort[] { 8, 8, 8 };
options.Photometric = TiffPhotometrics.Rgb;
options.Xresolution = new TiffRational(72);
options.Yresolution = new TiffRational(72);
options.ResolutionUnit = TiffResolutionUnits.Inch;
options.PlanarConfiguration = TiffPlanarConfigs.Contiguous;

// Set the Compression to AdobeDeflate
options.Compression = TiffCompressions.AdobeDeflate;

// Or Deflate                        
// Options.Compression = TiffCompressions.Deflate;

// Create a new TiffImage with specific size and TiffOptions settings
// via creating TiffFrame with that size and options
using (TiffImage tiffImage = new TiffImage(new TiffFrame(options, 100, 100)))
{
    // Loop over the pixels to set the color to red
    for (int i = 0; i < 100; i++)
    {
        tiffImage.ActiveFrame.SetPixel(i, i, Color.Red);
    }
    // Save resultant image
    tiffImage.Save("output.tiff");
}
```

## TiffFrame properties

TiffFrame is actually a descendant of RasterImage (or, more specifically, <a href="">RasterCachedImage</a>), and as such, for the most part, it can be handled like any other RasterImage instance.
The differences from a RasterImage are that TiffFrame has <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffframe/properties/exifdata">ExifData</a> property, which contains EXIF metadata as an ExifData instance, and, expectedly, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffframe/properties/frameoptions">FrameOptions</a> property, which contains TiffOptions instance describing frame's TIFF-related properties. <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image/methods/getoriginaloptions">GetOriginalOptions</a> method, however, does the same, it is just not explicitly typed as TiffOptions. This is what you want to use if you need to get TIFF's format properties of the image.
Also, there are two new methods for TiffFrame:

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffframe/methods/copyframe">CopyFrame</a>
Creates an identical copy of the frame. This has been used in examples above.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffframe/methods/createframefrom">CreateFrameFrom</a>
Creates new frame, like CopyFrame, but with properties specified in TiffOptions, so you can use it co convert to different pixel format.

Example on getting compression method of TIFF frames:
```csharp
using (TiffImage tiff = (TiffImage)Image.Load("TiffCompressed.tiff"))
{
    foreach(TiffFrame frame in tiff.Frames)
    {
        Console.WriteLine(frame.FrameOptions.Compression.ToString());
    }
}
```
 

## TiffImage properties

As said before, TiffImage retains most of the RasterImage's methods and properties, but only applies them to the current ActiveFrame. Most of the TiffImage-specific additions are related to the TiffImage being a collection of images. The only exception being the <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffimage/properties/byteorder">ByteOrder</a> property, which defines endianness of the data in file, and is applied for all the file's contents, so TiffFrame's don't have that property. TiffOptions do specify this property, as TIFF images can be also creating by just saving a loaded image of any format using <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.image/save/methods/3">Save()</a> method with a TiffOptions instance, and not by frame-by-frame approach. In that case, all frames within file will have identical parameters. Other two properties are already described ActiveFrame and Frames.
TiffImage's new methods haven't been descried yet, so there they are:

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffimage/methods/add">Add</a>
Adds all frames of provided TiffImage to the end of Frames array of current image.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffimage/methods/addframe">AddFrame</a>
Adds the provided TiffFrame to the end of Frames array of current image.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffimage/methods/addframes">AddFrames</a>
Adds provided array of TiffFrame to the end of Frames array of current image.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffimage/methods/insertframe">InsertFrame</a>
Inserts provided TiffFrame into specified index of Frames array.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff.tiffimage/removeframe/methods/1">RemoveFrame</a>
Removes frame either by index or by <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffimage/methods/removeframe">instance</a>.

There are also several helper methods that work on all frames at once:

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffimage/methods/alignresolutions">AlignResolutions</a>
Makes vertical and horizontal resolutions of all frames equal

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffimage/methods/resizeproportional">ResizeProportional</a>
Resizes all frames of image according to the ratio of newWidth/width and newHeight/height. 

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffimage/methods/rotateflipall">RotateFlipAll</a>
Rotates/flips all frames of the image.


## Extracting frames from TIFF and saving them as separate images
Here is an example on how to save different frames of TIFF to different BMP files:

```csharp
using (TiffImage multiImage = (TiffImage)Image.Load("SampleTiff1.tiff"))
{
    // Create an instance of int to keep track of frames in TiffImage
    int frameCounter = 0;

    // Iterate over the TiffFrames in TiffImage
    foreach (TiffFrame tiffFrame in multiImage.Frames)
    {
        multiImage.ActiveFrame = tiffFrame;

        // Load Pixels of TiffFrame into an array of Colors
        Color[] pixels = multiImage.LoadPixels(tiffFrame.Bounds);

        // Create an instance of bmpCreateOptions
        BmpOptions bmpCreateOptions = new BmpOptions();
        bmpCreateOptions.BitsPerPixel = 24;

        // Set the Source of bmpCreateOptions as FileCreateSource by specifying the location where output will be saved
        bmpCreateOptions.Source = new FileCreateSource(string.Format("ConcatExtractTIFFFramesToBMP_out{0}.bmp", frameCounter), false);
        
        // Create a new bmpImage
        using (BmpImage bmpImage = (BmpImage)Image.Create(bmpCreateOptions, tiffFrame.Width, tiffFrame.Height))
        {
            // Save the bmpImage with pixels from TiffFrame
            bmpImage.SavePixels(tiffFrame.Bounds, pixels);
            bmpImage.Save();
        }
        frameCounter++;
    }
}
```


That's all for now, the following article will describe TiffOptions in depth!

For more examples please visit the <a href="https://github.com/aspose-imaging">Aspose.Imaging GitHub</a> page. There's also <a href="https://twitter.com/Asposeimaging">Twitter</a> and <a href="https://www.facebook.com/AsposeImaging-1702883649750052/">Facebook</a> pages for news on Aspose.Imaging.

