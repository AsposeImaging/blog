Hello, in this article I'm going to list what distinguishes Aspose.Imaging from other similar software and generally about it's interesting features.

## Unified API for .Net above 2.0 and Java
Aspose.Imaging is built for .Net and Java from single source and hence both versions are identical and have the same API. The documentation for API is availible for both versions at <a href="https://apireference.aspose.com/"></a> and is styled according to convention of .Net and Java ecosystems respectively. There's no difference other than styling. Note that in this and other articles I refer to .Net version of docs.

## Cloud version via REST API
There is a public REST API service that allows you to use Aspose.Imaging, upload files to the API host, process them and download them back. See documentation at <a href="https://docs.aspose.cloud/display/imagingcloud/Home">Aspose.Imaging Cloud Documentation</a>.

## ICC color profile support
Aspose.Imaging can convert images using the a specified color profile. Color profiles are supported for TIFF, PSD and JPEG images (when YCCK or CMYK color mode is used).  an example on using color profiles for TIFF images cam be found in the <a href="https://asposeimaging.github.io/Converting-RGB-TIFF-to-CMYK-TIFF-with-Aspose.Imaging-18.3/">Converting RGB TIFF to CMYK TIFF with Aspose.Imaging 18.3</a> article.

## Multithreading support.
All images loaded by Aspose.Imaging are independent instances and can be processed concurrently without problems. Though, manipulation with a single image should happen only within one thread.


## OpenDocument Graphics (ODG) support
Aspose.Imaging can load ODG images and export them to raster. As with any other readable format, these files are loaded with <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image/methods/load/index">Image.Load()</a> method.

## Rasterization of SVG and Metafile (EMF, WMF) vector images
These formats can as well be loaded by <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image/methods/load/index">Image.Load()</a> method and then saved to any supported export format.
```csharp
public void SvgToPng()
{
    string path = "in.svg";
    string destFilePath = "out.png";
    using (Image image = Image.Load(path))
    {
        image.Save(destFilePath, new PngOptions());
    }
}
```

## DICOM and DJVU support
These are supported as import formats, Aspose.Imaging cannot export them yet. Multipage support works for both formats, and you can select which pages you want to export. The detailed examples are provided in <a href="https://asposeimaging.github.io/Introduction-to-Aspose.Imaging,-Part-2/">Introduction to Aspose.Imaging, Part 2</a> article.

## Saving images to PDF format.
Loaded images can be exported to PDF. Note that only output of PDF files is supported, Aspose.Imaging doesn't read them, that's the job of another product, Aspose.PDF. As any export, it can be selected by creating <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/pdfoptions">PdfOptions</a> instance and passing it to <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image/methods/save/index">Image.Save()</a>. Here's the example:
```csharp
public void GifToPdf()
{
    string path = "transparent_orig.gif";
    string destFilePath = "transparent_orig.gif.pdf";
    using (Image image = Image.Load(path))
    {
        image.Save(destFilePath, new PdfOptions());
    }
}
```

## XMP and EXIF metadata support.
Both metadata formats can be read and manipulated with Aspose.Imaging.
See example for using XMP metadata in <a href="https://asposeimaging.github.io/Introduction-to-Aspose.Imaging,-Part-5/">Introduction to Aspose.Imaging, Part 5</a> article.


## Large image support
Aspose.Imaging supports working on images that do not fit into RAM, allowing you to process very large image files even on relatively weak computers. 

### 16 bit per channel TIFF support
Since version 18.6, Aspose.Imaging supports TIFF files with 16 bit channels. However, support is not complete yet, ICC profiles are not applied currently. There is an example on how to extract per-channel values:
```csharp
// ICC profile is not applied for 16-bit color components at the moment, so disable that option explicitly.
LoadOptions loadOptions = new LoadOptions();
loadOptions.UseIccProfileConversion = false;

Rectangle desiredArea = new Rectangle(470, 1350, 30, 30);

using (RasterImage image = (RasterImage)Image.Load(tiff16File, loadOptions))
{
	long[] colors64Bit = image.LoadArgb64Pixels(image.Bounds);

	ushort alpha, red, green, blue;
	for (int y = desiredArea.Top; y < desiredArea.Bottom; ++y)
	{
		for (int x = desiredArea.Left; x < desiredArea.Right; ++x)
		{
			int offset = y * image.Width + x;
			long color64 = colors64Bit[offset];

			alpha = (ushort)((color64 >> 48) & 0xffff);
			red = (ushort)((color64 >> 32) & 0xffff);
			green = (ushort)((color64 >> 16) & 0xffff);
			blue = (ushort)(color64 & 0xffff);

			Console.WriteLine("A={0}, R={1}, G={2}, B={3}", alpha, red, green, blue);
		}
	}
}
```

### DNG support
DNG file format can be read by Aspose.Imaging for lossless processing of photo images. As with any other images, just call <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image/methods/load/index">Image.Load()</a>.

### Lossless JPEG and JPEG-LS support
These are rarely implemented compression method for JPEG files. Aspose.Imaging allows setting compression type via <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/jpegoptions/properties/compressiontype">CompressionType</a> property of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/jpegoptions">JpegOptions</a>. Also, baseline and progressive compression methods are supported.

### Jpeg2000 support.
This is also a rarely supported format. Aspose.Imaging supports both import and export of Jpeg2000 images. As  any other output format, it can be selected for export by creating <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/jpeg2000options">Jpeg2000Options</a> instance and passing it to <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/image/methods/save/index">Image.Save()</a> method.

### Specific image format support optimisations
Aspose.Imaging supports lossy GIF for reducing GIF image size.
For JPEG, rate-distortion optimisation is used to optimise image quality during compression.

## Coming soon

### .Net Standard
.Net Standard will be supported in the near future, so there will be a version of the library with native .NET Core support.

### Reverse image search
Aspose.Imaging for Cloud (i.e. the public REST API) will support reverse image search soon!

## Phasing out

### PSD support
PSD support will be moved to its own product, Aspose.PSD.


For now, that's all. This article will be updated with new and old interesting features in the future.



