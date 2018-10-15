In this article I'll tell about recent notable improvements of <a href="https://products.aspose.com/imaging/">Aspose.Imaging</a>.

### Fixed saving Baseline JPEG to Progressive.
Previously, in case you read an baseline JPEG image and tried to export it as progressive JPEG, you'd get an exception. Now this error is fixed and you can do it just fine like that:
```csharp
using (Image image = Image.Load("interleaved.jpg"))
{
	JpegOptions saveOptions = new JpegOptions();
	saveOptions.CompressionType = JpegCompressionMode.Progressive;

	image.Save(dir + "interleaved_progressive.jpg", saveOptions);
}
```

### Incorrect EMF creation.
Before, graphic primitive count in EMF's header was set incorrectly, leading to incorrect representation of the last added graphic primitive. The error has been fixed and EMFs are created correctly.

### Dashed lines saved as solid when exporting to SVG.
When source containing dashed lines was saved to SVG - be it vector graphics file or newly created set of graphic primitives, they were exported to SVG as solid lines. As of Aspose.Imaging 18.9, this problem has been fixed for licensed mode. However, when operating without license, lines still get converted to solid ones. This problem should be resolved in Aspose.Imaging 18.10 release.

### JPEG encoder optimisations.
JPEG DCT encoder has been optimised and displays up to 50% more performance (= up to 30% less time to encode).

### DICOM losing color when exporting to BMP.
DICOM files with RGB color space that used lossless JPEG compression were exported as grayscale when exporting to BMP (actually, they were read as grayscale). Now this has been fixed and output JPEG should be correct.

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-imaging">Aspose.Imaging GitHub</a> page. There's also <a href="https://twitter.com/Asposeimaging">Twitter</a> and <a href="https://www.facebook.com/AsposeImaging-1702883649750052/">Facebook</a> pages for news on Aspose.Imaging.
