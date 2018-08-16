---
title: Working with ICC profiles in Aspose.Imaging
layout: post
tags: csharp, raster, image, convert
---

<a href="https://products.aspose.com/imaging/">Aspose.Imaging</a> supports retrieving, applying and storing ICC color profiles for PSD and JPEG images, and can embed color profiles when exporting TIFF images. For other formats, embedded color profiles are processed when loading image, but are not availible for manipulation as of v18.7. In this article I'll show how to work with ICC profiles for JPEG, PSD and TIFF.

### Helper methods for loading and saving profiles
ICC color profiles stored in PSD and JPEG images are represented as properties of type <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.sources/streamsource/">StreamSource</a>, which facilitates directly loading/saving them from/to .icc(m) files. 
In this section I will provide methods that will be used in following sections. Here are methods to create a StreamSource from file and to save StreamSource to file:
```csharp
using Aspose.Imaging.Sources;
using Aspose.Imaging.ImageOptions;
using System.IO;


private static StreamSource GetSourceForFilename(string filename)
{
    MemoryStream memory = new MemoryStream(File.ReadAllBytes(filename));
    memory.Seek(0, System.IO.SeekOrigin.Begin);
    StreamSource source = new StreamSource(memory);
    return source;
}

private static void SaveSourceToFile(StreamSource source, string filename)
{
    using (FileStream fileStream = new FileStream(filename, FileMode.Create))
    {
        long position = source.Stream.Position;
        source.Stream.Seek(0, System.IO.SeekOrigin.Begin);
        int byteCount;
        byte[] buffer = new byte[1024];
        while ((byteCount = source.Stream.Read(buffer, 0, buffer.Length)) > 0)
        {
            fileStream.Write(buffer, 0, byteCount);
        }
        source.Stream.Seek(position, System.IO.SeekOrigin.Begin);
        fileStream.Flush();
    }
}

```

In case of TIFF files, color profiles are applied during saving of the image, and are provided in <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/iccprofile">IccProfile</a> property of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/">TiffOptions</a>'s instance as an instance of <a href="https://docs.microsoft.com/en-us/dotnet/api/system.io.memorystream?redirectedfrom=MSDN&view=netframework-4.7.2">MemoryStream</a>. Here are methods to convert <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.sources/streamsource/">StreamSource</a> to MemoryStream and to load a file into MemoryStream:
```csharp
        private static MemoryStream FileToMemoryStream(string filename)
        {
            MemoryStream memory = new MemoryStream(File.ReadAllBytes(filename));
            memory.Seek(0, System.IO.SeekOrigin.Begin);
            return memory;
        }
```


### Applying color profile when saving to TIFF
This is straightforward to do: just set <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/iccprofile">IccProfile</a> property of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/">TiffOptions</a> with the profile, and save the image:
```csharp
using (Image image = Image.Load("sample.jpg"))
{
    TiffOptions options = new TiffOptions(FileFormats.Tiff.Enums.TiffExpectedFormat.Default);
	using(MemoryStream ms = FileToMemoryStream("profile.icc"))
	{
		options.IccProfile = ms;
		image.Save("profiled_sample.tif", options);
	}
}
```

### Color profiles in PSD
There are three color profile properties in <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.psd/psdimage/">PSDImage</a>, two of them are applied to CMYK images and must be used together for correct conversion, these are the <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.psd/psdimage/properties/rgbcolorprofile">RgbColorProfile</a>  and <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.psd/psdimage/properties/cmykcolorprofile">CmykColorProfile</a>. By default, if these profiles are embedded in image, they will be applied during loading by default. 
Here is an example where CMYK color profile from PSD image is embedded into CMYK TIFF image:
```csharp
string inputFile = "cmyk.psd";
string outputFile = "cmyk.tiff";

using (PsdImage image = (PsdImage)Image.Load(inputFile))
{
    TiffOptions tiff = new TiffOptions(FileFormats.Tiff.Enums.TiffExpectedFormat.TiffLzwCmyk);
    using (MemoryStream ms = SourceToMemoryStream(image.CmykColorProfile))
    {
        tiff.IccProfile = ms;
        image.Save(outputFile, tiff);
    }
}
```

Third is <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.psd/psdimage/properties/graycolorprofile">GrayColorProfile</a>, used in monochrome images, and is not applied during loading. To enable application of profile explicitly, create <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/loadoptions/">LoadOptions</a> instance, set <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/loadoptions/properties/useiccprofileconversion">UseIccProfileConversion</a> to <b>true</b> and load image using this instance. See example:
```csharp
// Save to grayscale TIFF
TiffOptions saveOptions = new TiffOptions(TiffExpectedFormat.Default);
saveOptions.setPhotometric(TiffPhotometrics.MinIsBlack);
saveOptions.setBitsPerSample(new int[] { 8 });

// If the image contains a built-in Gray ICC profile, it is not be applied by default in contrast of the CMYK profile.
// Enable ICC conversion explicitly.
LoadOptions loadOptions = new LoadOptions();
loadOptions.UseIccProfileConversion= true;

using(PsdImage psdImage = (PsdImage)Image.load(sourcePath, loadOptions))
{
	// Embed the gray ICC profile to the output TIFF.
	// The built-in Gray Profile can be read via the PsdImage.GrayColorProfile property.
	saveOptions.setIccProfile(toMemoryStream(psdImage.getGrayColorProfile()));
	psdImage.save(outputPath, saveOptions);
}
```

### Color profiles in JPEG
<a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.jpeg/jpegimage/">JpegImage</a> contains four properties related to color profiles. <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.jpeg/jpegimage/properties/rgbcolorprofile">RgbColorProfile</a> and <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.jpeg/jpegimage/properties/cmykcolorprofile">CmykColorProfile</a>, likewise to PSD, are used for CMYK and YCCK JPEG images when loading the image. <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.jpeg/jpegimage/properties/destinationcmykcolorprofile">DestinationCmykColorProfile</a> and <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.jpeg/jpegimage/properties/destinationrgbcolorprofile">DestinationRgbColorProfile</a> are applied when a JpegImage is saved, and should be paired with opponent profile - i.e. when using DestinationCmykColorProfile RgbColorProfile should be set to a corresponding profile, and when using DestinationRgbColorProfile CmykColorProfile should be set to a corresponding profile too.
Here is an example that shows use of profiles when loading image:
```csharp
using (JpegImage image = (JpegImage)Image.Load("image.jpg"))
{
    //load profiles
    StreamSource rgbprofile = GetSourceForFilename("rgb.icc");
    StreamSource cmykprofile = GetSourceForFilename("cmyk.icc");
    // set profiles for image
    image.RgbColorProfile = rgbprofile;
    image.CmykColorProfile = cmykprofile;
    //now the profiles are applied when image's data is retrieved
    Aspose.Imaging.Color[] colors = image.LoadPixels(new Rectangle(0, 0, image.Width, image.Height));
}
```
Here is an example that shows use of profiles when saving image:
```csharp
using (JpegImage image = (JpegImage)Image.Load("image.jpg"))
{
    //load profiles
    StreamSource rgbprofile = GetSourceForFilename("rgb.icc");
    StreamSource cmykprofile = GetSourceForFilename("cmyk.icc");
    // apply profiles to image for saving
    image.DestinationRgbColorProfile = rgbprofile;
    image.DestinationCmykColorProfile = cmykprofile;
    //save changes back to image
    image.Save();
}
``` 


### Disabling default application of embedded color profile.
If you don't want to apply image's embedded color profile during image loading, there is an option to disable this processing using <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/loadoptions/">LoadOptions</a>. See example:
```csharp
LoadOptions loadOptions = new LoadOptions();
loadOptions.UseIccProfileConversion= false;

using(image = Image.load(sourcePath, loadOptions))
{
	image.save(outputPath, new PngOptions());
}
``` 



That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-imaging">Aspose.Imaging GitHub</a> page. There's also <a href="https://twitter.com/Asposeimaging">Twitter</a> and <a href="https://www.facebook.com/AsposeImaging">Facebook</a> pages for news on Aspose.Imaging.
