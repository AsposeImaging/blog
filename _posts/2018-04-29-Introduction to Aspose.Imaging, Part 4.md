---
title: Introduction to Aspose.Imaging, Part 4.
published: true
description: 
tags: csharp, image, raster, convert
---


In this article I will describe one of the most important features of <a href="https://products.aspose.com/imaging/">Aspose.Imaging</a>, filtering images, and what filters are availible.

Here are the previous articles:

<a href="https://dev.to/nnevod/introduction-to-asposeimaging-8jd">Part 1</a>

<a href="https://dev.to/nnevod/introduction-to-asposeimaging-part-2-40p">Part 2</a>

<a href="https://dev.to/nnevod/introduction-to-asposeimaging-part-3-3ah3">Part 3</a>

## Filtering images
Applying a filter to image is very simple, just create a descendant of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imagefilters.filteroptions/filteroptionsbase">FilterOptionsBase</a> and pass it to <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage">RasterImage</a>'s <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage/methods/filter">Filter</a> method:
```csharp
// Load the image
using (Image image = Image.Load(dataDir + "asposelogo.gif"))
{
    // Convert the image into RasterImage, Pass Bounds[rectangle] 
	// of image and GaussianBlurFilterOptions instance to Filter method and Save the results
    RasterImage rasterImage = (RasterImage)image;
    rasterImage.Filter(rasterImage.Bounds, new GaussianBlurFilterOptions(5, 5));
    rasterImage.Save(dataDir + "BlurAnImage_out.gif");
}
```
So you can perform filtering of only a part of image if you need to.

## Filter types
There are several supported filters. First, there are <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imagefilters.filteroptions/medianfilteroptions">MedianFilterOptions</a> which specify median filter, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imagefilters.filteroptions/bigrectangularfilteroptions">BigRectangularFilterOptions</a> and <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imagefilters.filteroptions/smallrectangularfilteroptions">SmallRectangularFilterOptions</a> - different kinds of box filter, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imagefilters.filteroptions/bilateralsmoothingfilteroptions">BilateralSmoothingFilterOptions</a>, which, namely, does bilateral smoothing, two kinds of convolutional filters - <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imagefilters.filteroptions/gaussianblurfilteroptions">GaussianBlurFilterOptions</a> and <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imagefilters.filteroptions/sharpenfilteroptions">SharpenFilterOptions</a>, again with descriptive names, and the most interesting ones are deconvolutional filters that perform Wiener deconvolution - <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imagefilters.filteroptions/gausswienerfilteroptions">GaussWienerFilterOptions</a>, that removes Gaussian blur, and <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imagefilters.filteroptions/sharpenfilteroptions">MotionWienerFilterOptions</a>, that removes motion blur. 
BigRectangularFilterOptions and SmallRectangularFilterOptions are parameterless.
MedianFilterOptions requires filter box size to create.
BilateralSmoothingFilterOptions can be created without parameters or with kernel size as parameter.
GaussianBlurFilterOptions can be created without parameters with default values, or you can provide filter radius and sigma. 
Same applies to SharpenFilterOptions.
GaussWienerFilterOptions also can be created default, or with filter radius and sigma - if you know the parameters with which the blur was applied.
MotionWienerFilterOptions is created with length, angle, that are defining the blur vector and smoothing factor of the blur, so you can tune optimum parameters for removing motion blur from image.


That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-imaging">Aspose.Imaging GitHub</a> page. There's also <a href="https://twitter.com/Asposeimaging">Twitter</a> and <a href="https://www.facebook.com/AsposeImaging">Facebook</a> pages for news on Aspose.Imaging.

