---
title: Introduction to Aspose.Imaging, Part 3
layout: post
tags: csharp, image, raster, modify
---



Here I will talk about how to use <a href="https://products.aspose.com/imaging/">Aspose.Imaging</a> the most common ways of modifying raster images: ajdustment of brightness, contrast and gamma, considered 'common' adjustments for raster images, cropping and resizing an image and dithering and binarization  often done when preparing images for printing.

## Brightness, contrast and gamma.
These are performed by one-line command and are very similar in nature, so I will provide an example that does all of them:
```csharp

            using (Image img = Image.Load("sample.png"))
            {
                // Cast object of Image to RasterImage
                RasterImage rasterImage = (RasterImage)img;

                // Check if RasterImage is cached and Cache RasterImage for better performance
                if (!rasterImage.IsCached)
                {
                    rasterImage.CacheData();
                }

                // Adjust the brightness
                rasterImage.AdjustBrightness(100);

                //Adjust the contrast
                rasterImage.AdjustContrast(50);

                //Adjust gamma
                rasterImage.AdjustGamma(2.2f, 2.2f, 2.2f);

                // Create an instance of TiffOptions for the resultant image, Set various properties for the object of TiffOptions and Save the resultant image
                PngOptions options = new PngOptions();
                rasterImage.Save("sample_out.png", options);
            }
```
As a result, the rasterImage will have all the changes at once: brightness, contrast and gamma, as the methods modify the instance they're called on. Now, how exactly do these methods work?
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage/methods/adjustbrightness">AdjustBrightness</a> essentially shifts brightness of all the pixels of image by stated amount, right in value codes. As our images are typically encoded in 8 bit per channel format, that means calling AdjustBrightness(-256) will make image black, and AdjustBrightness(256) will make it white regardless of whatever was there - as every code will be reduced/increased right to the minimum/maximum possible value.
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage/methods/adjustcontrast">AdjustContrast</a> also adjusts contrast relative to current state. The adjustment range is from -100 to 100, the absolute value is percents essentially. Albeit higher absolute values can be passed to the method, they will be rounded to -100 or 100 respectively. -100 produces uniformly colored image, +100 increases current image contrast twofold.
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage/methods/adjustgamma/index">AdjustGamma</a> again works relative to current state. Gamma coefficient is applied as is, i.e. input image is interpreted as being in linear space. Thus, specifying coefficient of 1.0 results in no change to image. There are two overloads, one shown sets separate gamma value per R, G and B channels, another takes one value and applies it to all channels.

## Cropping an image
For cropping, there are two essential approaches in Aspose.Imaging. First is specifying a <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rectangle">Rectangle</a> that will be used as a boundary for cropping, and another is specifying borders - top, bottom, left and right, that will be removed from image.
```csharp
            using (RasterImage rasterImage = (RasterImage)Image.Load("sample.png"))
            {
                // Create an instance of Rectangle class with desired size,
				// Perform the crop operation on object of 
				// Rectangle class and Save the results to disk
                Rectangle rectangle = new Rectangle(20, 20, 20, 20);
                rasterImage.Crop(rectangle);
                rasterImage.Save(dataDir + "Sample_crop.jpg");
            }

            using (RasterImage rasterImage = (RasterImage)Image.Load("sample.png"))
            {
                // Create an instance of Rectangle class with desired size, Perform the crop operation on object of Rectangle class and Save the results to disk
                
				// Crop off 20-pixel-wide borders from each side
                rasterImage.Crop(20,20,20,20);
                rasterImage.Save(dataDir + "Sample_crop1.jpg");
            }
```
Rectangle's Y coordinate is measured from top border of image, as typical in raster graphics.

## Resizing an image
Aspose.Imaging allows resizing of an image separately for width and height, and for both of them at once, and allows specifying resizing method.
```csharp
            using (Image image = Image.Load("sample.png"))
            {
                if (!image.IsCached)
                {
                    image.CacheData();
                }

                // Specifying only height, width and ResizeType
                int newWidth = image.Width / 2;
                image.ResizeWidthProportionally(newWidth, ResizeType.LanczosResample);
                int newHeight = image.Height / 2;
                image.ResizeHeightProportionally(newHeight, ResizeType.NearestNeighbourResample);

                image.Resize(newWidth * 3, newHeight * 2, ResizeType.HighQualityResample);

                image.Save("samlpe_out.png");
            }
```
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image/methods/resizeheightproportionally/index">ResizeHeightProportionally</a> and <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image/methods/resizewidthproportionally/index">ResizeWidthProportionally</a> take new height and width, respectively, and resize image without altering proportions. <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image/methods/resize/index">Resize</a> doesn't retain proportions, as both height and width are provided. <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/resizetype">ResizeType</a> enumerates various possible rescale algorithms.

## Dithering and binarization.
Dithering is performed equally easily as most other transformations, just specify desired bit depth and dithering method:
```csharp
            using (var image = (PngImage)Image.Load("sample.png"))
            {
                // Peform dithering on the current image and Save the resultant image
                image.Dither(DitheringMethod.ThresholdDithering, 4);
                image.Save("sample_out.png");
            }
```
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging/ditheringmethod">DitheringMethod</a> provides options for treshold dithering and Floyd-Steinberg dithering. There is also an overload that allows dithering with a custom <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/icolorpalette">palette</a>.

Binarization, i.e. conversion of image to black&white without dithering can be done with a custom treshold, with <a href="https://en.wikipedia.org/wiki/Otsu's_method">Otsu's method</a> and via <a href="http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.420.7883&rep=rep1&type=pdf">Bradley's method</a>. All methods are equally simple to use:
```csharp
            using (Image image = Image.Load("sample.png"))
            {
                // Cast the image to RasterCachedImage and Check if image is cached                
                RasterCachedImage rasterCachedImage = (RasterCachedImage)image;
                if (!rasterCachedImage.IsCached)
                {
                    // Cache image if not already cached
                    rasterCachedImage.CacheData();
                }

                // Binarize image with predefined fixed threshold              
                rasterCachedImage.BinarizeFixed(250);
				
                // Binarize image with Otsu Thresholding and Save the resultant image                
                rasterCachedImage.BinarizeOtsu();
				
				// Define threshold value for Bradley method, Call BinarizeBradley method and pass the threshold value as paramete
				double threshold = 0.15;
                objimage.BinarizeBradley(threshold);
				
                rasterCachedImage.Save("sample.png");
            }
```
This example just puts all methods into one snippet for brevity, don't try to really implement binarization that way - the result will be meaningless.
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage/methods/binarizefixed">BinarizeFixed</a> will set pixels with values higher than treshold to white, and others to black. <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage/methods/binarizeotsu">BinarizeOtsu uses  <a href="https://en.wikipedia.org/wiki/Otsu's_method">Otsu's</a> method will determine treshold from image contents, trying to find optimal treshold algorithmically. <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/rasterimage/methods/binarizebradley">BinarizeBradley</a> uses <a href="http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.420.7883&rep=rep1&type=pdf">Bradley's</a> method works locally, it turns a pixel to black if it's brightness is lower than average brightness of neighbour pixels multiplied by treshold value.

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-imaging">Aspose.Imaging GitHub</a> page. There's also <a href="https://twitter.com/Asposeimaging">Twitter</a> and <a href="https://www.facebook.com/AsposeImaging">Facebook</a> pages for news on Aspose.Imaging.

