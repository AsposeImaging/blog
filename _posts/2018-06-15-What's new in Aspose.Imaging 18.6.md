---
title: What's new in Aspose.Imaging 18.6
layout: post
tags: csharp, raster, image, convert
---

A new version of Aspose.Imaging is released, designated 18.6. Here are the most important changes:

## New features

### Support of JPT codec for Jpeg2000 file format
This is also a bug fix at once - JPT codec option was availible previously, but did nothing. See example:
```csharp
using (Image img = Image.Load("test.j2k"))
{
   img.Save("test.jp2", new Jpeg2000Options()
   {
       Comments = new string[] { "Aspose" },
       Codec = Jpeg2000Codec.Jpt
   });
}
```

### Support for conversion of raster images to PDF
There is a code example converting GIF to PDF:
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

### Support for 16-bit TIFF format
Now Aspose.Imaging supports loading 16 bit per channel TIFF files. However, there are some limitations, namely, ICC profiles aren't applied to 16-bit channels yet.. Here is an example covering how you can extract channel values from 16-bit TIFFs at the moment.
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

### Explicit alpha preblend control support for export to JPG.
Now it is possible to force disable or enable of alpha blending when exporting to JPG format, as blending might be undesirable. <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/jpegoptions/properties/preblendalphaifpresent">PreblendAlphaIfPresent</a> property is added to <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/jpegoptions">JpegOptions</a> for this. See example:
```csharp
string sourcePath = "alphachannel.psd";            
string outputPath = "alphachannel_out.jpg";

PsdLoadOptions loadOptions = new PsdLoadOptions();
loadOptions.ReadOnlyMode = true;

JpegOptions saveOptions = new JpegOptions();
saveOptions.PreblendAlphaIfPresent = false; // Disable alpha blending!

using (PsdImage image = (PsdImage)Image.Load(sourcePath, loadOptions))
{
	image.Save(outputPath, saveOptions);
}
```

### Default font setup option for rasterization of vector formats
There was already an option to set default font for PSD files with missing fonts, not it works with other vector-capable formats - EMF, ODG, SVG and WMF. See the example:
```csharp
FontSettings.DefaultFontName = "Comic Sans MS";

string[] files = new string[] { "missing-font.emf", "missing-font.odg", "missing-font.svg", "missing-font.wmf" };
VectorRasterizationOptions[] options = new VectorRasterizationOptions[] { new EmfRasterizationOptions(), new MetafileRasterizationOptions(), new SvgRasterizationOptions(), new WmfRasterizationOptions() };

for (int i = 0; i < files.Length; i++)
{
    string outFile = files[i] + ".png";
    using (Image img = Image.Load(files[i]))
    {
        options[i].PageWidth = img.Width;
        options[i].PageHeight = img.Height;
        img.Save(outFile, new PngOptions()
        {
            VectorRasterizationOptions = options[i]
        });
    }

}
```
Note that <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/fontsettings">Aspose.Imaging.FontSettings</a> class also supports setting a custom font search folder:
```csharp
var fontFolder = @"c:\myFonts";
List<string> folders = new List<string>(FontSettings.GetFontsFolders());
folders.Add(fontFolder);
FontSettings.SetFontsFolders(folders.ToArray(), true);

//// processing

FontSettings.Reset();
```

## Bug fixes

Background color change when JFIF is set for JPG export is fixed.

Fix for CMYK color profiles for PSD to TIFF conversion - previously color was changing a bit from what was expected.

