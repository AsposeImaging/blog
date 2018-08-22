---
title: Controlling image loading process in Aspose.Imaging
layout: post
tags: csharp, raster, image, load
---

<a href="https://products.aspose.com/imaging/">Aspose.Imaging</a> provides options to control image loading process, so as wether to apply ICC profile or not, or how high error tolerance is allowed. Most options are related to PSD format, but some other image formats have their options as well, and there are some options common for all formats. 

### Using LoadOptions
Specifying options for image loading process is straightforward: create an instance of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/loadoptions">LoadOptions</a> class or its descendant for corresponding image format, set up its properties and pass it to <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image">Image</a>'s <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.image/load/methods/1">Load</a> method. The following example disables default behaviour of processing image raster data with image's embedded ICC profile on loading.
```csharp
LoadOptions loadOptions = new LoadOptions();
loadOptions.UseIccProfileConversion = false;

using(image = Image.load(sourcePath, loadOptions))
{
	image.save(outputPath, new PngOptions());
}
```
For format-specific options, just create an instance of corresponding class and be sure you're loading an image of that exact format, otherwise it's the same.

### Common loading options
Options that are common for all formats are defined in the base <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/loadoptions">LoadOptions</a> class. There is a total of three availible options:
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging/loadoptions/properties/useiccprofileconversion">UseIccProfileConversion</a> - Used to force disable or force enable application of image's embedded ICC profile to image's raster data.
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging/loadoptions/properties/databackgroundcolor">DataBackgroundColor</a> - sets the background color. Background color is typically used for damaged data, when original pixel color can't be recovered.
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging/datarecoverymode">DataRecoveryMode</a> - sets damage tolerance mode. Either throw exception on any error, or try to recover data while it's consistent and format is not broken, or recover without any checking.


### PSD options 
PSD, being a complex format, has the most options. In addition to common options, <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/psdloadoptions">PsdLoadOptions adds next options:
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/psdloadoptions/properties/defaultreplacementfont">DefaultReplacementFont</a> - the font that will be used to draw text during raster export if the font specified in text layer is not present in system.
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/psdloadoptions/properties/ignorealphachannel">IgnoreAlphaChannel</a> - if true, discards aplha data.
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/psdloadoptions/properties/ignoretextlayerwidthonupdate">IgnoreTextLayerWidthOnUpdate</a> - Whether text layer's fixed width will be ignoring when text is updated when you're working with image.
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/psdloadoptions/properties/loadeffectsresource">LoadEffectsResource</a> - Whether effect resources should be loaded. Default is false. If the option is set, only supported effects will be applied anyways.
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/psdloadoptions/properties/readonlymode">ReadOnlyMode</a> - When read only mode is set, changes applied to layers will not be saved to final image, all data is used straight from ImageData section, so loaded image is identical to Photoshop.
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/psdloadoptions/properties/usediskforloadeffectsresource">UseDiskForLoadEffectsResource</a> - By default, effect resources are loaded from disk, if you have enough RAM, you can set it so effect resources will be loaded into memory, thus making processing faster.


### PNG options
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/pngloadoptions">PngLoadOptions</a> adds <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/pngloadoptions/properties/strictmode">StrictMode</a> option, which is an additional error tolerance setting that checks if any of file's critical chunks is truncated. As file can be loaded with some chunks missing, default setting is false. 

### DNG options
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/dngloadoptions">DngLoadOptions</a> adds option to control pre-demosaic denoising as <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/dngloadoptions/properties/fbdd">Fbdd</a> property. Either no reduction, light reduction or full reduction.

### SVG options
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/svgloadoptions/">SvgLoadOptions</a> adds <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/svgloadoptions/properties/defaultheight">DefaultHeight</a> and <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/svgloadoptions/properties/defaultwidth">DefaultWidth</a> properties, which set or get the respective dimensions to which the image is rendered. Should only be used when dimensions are not specified in file.

### JPEG2000 options
Jpeg2000 is a relatively complex algorithm and thus decoding can be slow. So <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/jpeg2000loadoptions/properties/maximumdecodingtime">MaximumDecodingTime</a> option is present in <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageloadoptions/jpeg2000loadoptions/">Jpeg2000LoadOptions</a> to prevent hanging on slower machines on very large - over 5500x6500 pixels - images. Time is specified in seconds.



That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-imaging">Aspose.Imaging GitHub</a> page. There's also <a href="https://twitter.com/Asposeimaging">Twitter</a> and <a href="https://www.facebook.com/AsposeImaging">Facebook</a> pages for news on Aspose.Imaging.
