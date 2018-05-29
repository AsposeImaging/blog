---
title: Introduction to Aspose.Imaging, Part 6
layout: post
tags: csharp, raster, image, convert
---


In this part I'll tell you about an easier way to rotate an image,  how to create multipage TIFFs and how to extract separate frames from an animated GIF file.

Here are links to previous parts:

<a href="https://dev.to/nnevod/introduction-to-asposeimaging-8jd">Part 1</a>

<a href="https://dev.to/nnevod/introduction-to-asposeimaging-part-2-40p">Part 2</a>

<a href="https://dev.to/nnevod/introduction-to-asposeimaging-part-3-3ah3">Part 3</a>

<a href="https://dev.to/nnevod/introduction-to-asposeimaging-part-4-2o1j">Part 4</a>

<a href="https://dev.to/nnevod/introduction-to-asposeimaging-part-5-1349">Part 5</a>


## Simple rotation. ##

In addition to transformation matrix-based rotation, there is also a helper method to allow for easy rotation in <a href="https://products.aspose.com/imaging/">Aspose.Imaging</a>. The method works on <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage">RasterImage</a> and its descendants. 
```csharp
// Load an image to be rotated in an instance of RasterImage
using (RasterImage image = (RasterImage)Image.Load("aspose-logo.jpg"))
{
    // Before rotation, the image should be cached for better performance
    if (!image.IsCached)
    {
        image.CacheData();
    }
    // Perform the rotation on 20 degree while keeping the image size proportional with red background color and Save the result to a new file
    image.Rotate(20f, true, Color.Red);
    image.Save( "RotatingImageOnSpecificAngle_out.jpg");
}
```
So you just have to specify rotation angle (clockwise), wether the new image should be resized to fit the rotated content, and what to fill "uncovered" areas with.

## Composing multi-frame TIFFs ##

This is actually also very simple, though the example will be longer. TIFF files allow several properties to be specified, the <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/bitspersample">BitsPerSample</a> property - array specifying number of bits per each channel of a pixel, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/orientation">Orientation</a> property - obvious, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/photometric">Photometric</a> property - specifies a value that defines color interpretation properties of a TIFF - response curves etc., <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/compression">Compression</a> property - obvious - and <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/fillorder">FillOrder</a> - endianness - property are specified in this example.
```csharp
List<string> files = new List<string>(new[] {  "Frame1.tiff", "Frame2.tiff" });
TiffOptions createOptions = new TiffOptions(TiffExpectedFormat.Default);
createOptions.BitsPerSample = new ushort[] { 1 };
createOptions.Orientation = TiffOrientations.TopLeft;
createOptions.Photometric = TiffPhotometrics.MinIsBlack;
createOptions.Compression = TiffCompressions.CcittFax3;
createOptions.FillOrder = TiffFillOrders.Lsb2Msb;


TiffImage output = null;

List<TiffImage> images = new List<TiffImage>();
try
{
    foreach (var file in files)
    {
        // Create an instance of TiffImage and load the source image
        TiffImage input = (TiffImage)Image.Load(file);
        images.Add(input); // Do not dispose before data is fetched. Data is fetched on 'Save' later.
        foreach (var frame in input.Frames)
        {
            if (output == null)
            {
                // Create a new tiff image with first frame defined.
                output = new TiffImage(TiffFrame.CopyFrame(frame));
            }
            else
            {
                // Add copied frame to destination image
                output.AddFrame(TiffFrame.CopyFrame(frame));
            }
        }
    }

    if (output != null)
    {
        // Save the result
        output.Save("MultiFrameTiff.tif", createOptions);
    }
}
finally
{
    foreach (TiffImage image in images)
    {
        image.Dispose();
    }
    output.Dispose();
}

```
Do note that images are only really added to the TIFF image on the <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image/methods/save/index">Save</a> call, so you should take care not to dispose them before. Here we create copies of the frames anyway.
To remove a frame from TIFF image, call <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff.tiffimage/removeframe/methods/1">RemoveFrame</a> with index of frame to remove.

## Saving a frame from multi-frame GIF ##

This is also quite simple to do, but the terminology is a bit different - here, frames are put in blocks, which are represented as objects implementing <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.gif/igifblock">IGifBlock</a> interface. Blocks can be added via <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.gif/gifimage/methods/addblock">AddBlock</a> and removed via <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.gif/gifimage/methods/removeblock">RemoveBlock</a>. To add an image as a block you just create a new <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.gif.blocks/gifframeblock">GifFrameBlock</a> passing image to the constructor. There are also other kinds of blocks. Consequently, here's how you can access them:
```csharp
Image objImage = Image.Load("sample.gif");
using (GifImage gif = (GifImage)objImage)
{
    // Iterate through arry of blocks in the GIF image
    for (int i = 0; i < gif.Blocks.Length; i++)
    {
        // Convert block to GifFrameBlock class instance
        GifFrameBlock gifBlock = gif.Blocks[i] as GifFrameBlock;

        // Check if gif block is then ignore it
        if (gifBlock == null)
        {
            continue;
        }
       
        // Create an instance of TIFF Option class and Save the GIFF block as TIFF image
        TiffOptions objTiff = new TiffOptions(TiffExpectedFormat.Default);
        gifBlock.Save("sample_frame_"  + i + "_out.tif", objTiff);
    }
}
```


That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-imaging">Aspose.Imaging GitHub</a> page. There's also <a href="https://twitter.com/Asposeimaging">Twitter</a> and <a href="https://www.facebook.com/AsposeImaging">Facebook</a> pages for news on Aspose.Imaging.
