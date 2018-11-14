<a href="https://products.aspose.com/imaging/">Aspose.Imaging</a> uses <a href="https://apireference.aspose.com/net/imaging/aspose.imaging/imageoptionsbase">ImageOptionsBase</a>-based objects to set up file formats when saving or creating images. TIFF format, as such, is configured via instances of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/">TiffOptions</a> class. As I've noted in <a href="https://asposeimaging.github.io/Clarifying-working-with-TIFF-in-Aspose.Imaging/">previous</a> article, TiffOptions can also be read from loaded image, so you can get information about format's settings for the particular file. Most of the properties of TiffOptions apply to indivitual <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffframe">TiffFrames</a>, with only few properties related to <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffimage">TiffImage</a> as a collection of TiffFrames. Given multi-image nature of TIFF, I will refer to whole TIFF, represented as TiffImage as "file", and to individual images within, represented as TiffFrame, as "image" or "page".

TiffOptions provides facility to set up TIFF file format as you need. Most of the often used options are supported explicitly and are availible as properties. Changes applied to them will result in according processing of the image data, if needed. TiffOptions also provides a way of working directly with TIFF tags, so anything not explicitly supported by Aspose.Imaging can also be worked upon. 

Here I will first describe the properties of TiffOptions, and after them I'll describe the TIFF-specific methods of TiffOptions, they are related to direct access to tag data.

## Properties
Here are descriptions of explicitly availible properties specific to TiffOptions, roughly grouped  by their scope

### Metadata

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/artist">Artist</a>
Person who created the image.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/copyright">Copyright</a>
Copyright notice.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/datetime">DateTime</a>
Date and time of image creation.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/documentname">DocumentName</a>
The name of the document from which this image was scanned.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/exififd">ExifIfd</a>
Pointer to EXIF IFD of the TIFF file.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/filestandard">FileStandard</a>
Either TIFF 6.0 Baseline or TIFF 6.0 Extended.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/imagedescription">ImageDescription</a>
The title of the image - comment or such. No 2-byte characters allowed.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/inknames">InkNames</a>
Names of the inks used in this image. Inks are colors of Separate photometric model.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/pagename">PageName</a>
Name of the page from which this image was scanned.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/pagenumber">PageNumber</a>
Page number of the page from which this image was scanned.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/scannermanufacturer">ScannerManufacturer</a>
Name of the manufacturer of the scanner used to scan this image.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/scannermodel">ScannerModel</a>
Model of the scanner used to scan this image.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/softwaretype">SoftwareType</a>
Name and version of software that created this image (or firmware if created by hardware).

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/targetprinter">TargetPrinter</a>
Description of the supposed printing environment.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/xmpdata">XmpData</a>
Metadata in XMP format, represented as <a href="">XmpPacketWrapper</a> (Aspose.Imaging API object to work with XMP metadata)

### Dimension and positioning data

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/orientation">Orientation</a>
Rotation and mirroring of the image.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/resolutionunit">ResolutionUnit</a>
Resolution unit for the image - inch, centimeter etc.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/xposition">Xposition</a>
Offset of the left side of the image from the left side of the page it's on.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/xresolution">Xresolution</a>
Resolution in pixels per resolution unit along horizonal axis.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/yposition">Yposition</a>
Offset of the top side of the image from the top side of the page it's on. TIFF's Y axis is downward, so this is positive value.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/yresolution">Yresolution</a>
Resolution in pixels per resolution unit along vertical axis.

### Image format

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/alphastorage">AlphaStorage</a>
Defines how alpha is stored - premultiplied or not. Used when there are more than 3 samples per pixel.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/bitsperpixel">BitsPerPixel</a>
Amount of bits constituting data for single pixel, calculated from BitsPerSample and SamplesPerPixel.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/bitspersample">BitsPerSample</a>
Number of bits per color component. This is an array, as each component can have different bit count. When this is set, SamplesPerPixel will be set to array's length.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/byteorder">ByteOrder</a>
This is a per-file setting, not per-image, describes endianness of the file.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/compression">Compression</a>
Compression method used for image raster data.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/faxt4options">FaxT4Options</a>
Options for Group 3 Fax compression (T4) - 1D compression, 2D compression, no compression, or filling bits to byte boundaries (expanding data size).

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/fillorder">FillOrder</a>
Order of bits within bytes

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/isextrasamplespresent">IsExtraSamplesPresent</a>
True if there are more samples in SamplesPerPixel than required by photometric setting.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/istiled">IsTiled</a>
True for tiled images. Large TIFF images are often split into tiles, which can reduce memory use when only part of the image has to be displayed/processed at once. 

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/photometric">Photometric</a>
Color space of the image data. BW with minimum as white, BW with minimum as black, RGB, paletted color, transparency mask (obsolete in TIFF v6.0), separate colors (typically used for CMYK), YCbCr, Cielab, Icclab, Itulab, Logl and Logluv are supported by Aspose.Imaging.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/planarconfiguration">PlanarConfiguration</a>
Describes wether colors are stored on separate planes or interleaved.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/predictor">Predictor</a>
Predictor for LZW compression - either none, or hozirontal difference.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/sampleformat">SampleFormat</a>
Numeric format of the sample - unsigned integer, signed integer, floating point, untyped, complex integer, complex floating point. Separate for each component.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/samplesperpixel">SamplesPerPixel</a>
Number of color components per pixel. 

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/subfiletype">SubFileType</a>
Type of data stored in subfile. Either just an image, thumbnail for another image in file, page, transparency mask or MRC image.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/ycbcrcoefficients">YCbCrCoefficients</a>
Coefficients for transformation of RGB data to YCbCR.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/ycbcrsubsampling">YCbCrSusbsampling</a>
Subsampling used for YCbCr image data.

### <a href="">Image data layout</a>
It is closely related to image format, as it also outlays how raster data is located in file.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/imagelength">ImageLength</a>
Height of the image in pixels as stated in TIFF tag.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/imagewidth">ImageWidth</a>
Width of the image in pixels as stated in TIFF tag.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/rowsperstrip">RowsPerStrip</a>
TIFF images can be organised into strips for faster IO and easier random access. This tag describes count of rows (lines) of image in single strip. Total count of strips per image can be calculated as follows: StripsPerImage = floor ((ImageLength + RowsPerStrip - 1) / RowsPerStrip). 

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/stripoffsets">StripOffsets</a>
Offsets of the strips constituting the image, relative to beginning of file. This is used to locate actual image data in file, unless tiling is used, then TileOffsets is used.
It is recommended  to keep array below 64k bytes in length, and keep strips below 64k bytes each as well. 

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/stripbytecounts">StripByteCounts</a>
Number of bytes in strips after compression.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/tilewidth">TileWidth</a>
Width of a tile in pixels (number of columns in a tile).
Horizontal, vertical and total tile counts can be calculated as follows:
TilesAcross = (ImageWidth + TileWidth - 1) / TileWidth
TilesDown = (ImageLength + TileLength - 1) / TileLength
TilesPerImage = TilesAcross * TilesDown.
Must be multiple of 16. Might be larger than ImageWidth, but this means either tiles are too large, or image is too small, so tiling is not optimal

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/tilelength">TileLength</a>
The tile length (height) in pixels. This is the number of rows(lines) in each tile. Must be a multiple of 16.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/tileoffsets">TileOffsets</a>
Offsets of tiles in file, relative to beginning of file. Tiles are ordered left-to-right and top-to-bottom. If planar configuration is used, first series of offsets point to tiles of first component plane, then second series for second plane etc, so total count of offsets is total count of tiles multiplied by samples per pixel.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/tilebytecounts">TileByteCounts</a>
Count of bytes in tile after compression.

### Color profiling and processing

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/colormap">ColorMap</a>
Palette used for the image if it is paletted, as stored in file. <a href="">Palette</a> property is already loaded and processed palette. Length must correspond to formula: 3*(2^BitsPerSample).

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/iccprofile">IccProfile</a>
Stream of color profile embedded in the image.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/halftonehints">HalfToneHints</a>
Values to control halftone function.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/maxsamplevalue">MaxSampleValue</a>
Maximum allowed sample value in image, per each channel. 16-bit version.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/minsamplevalue">MinSampleValue</a>
Minimum allowed sample value in image, per each channel. 16-bit version.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/premultiplycomponents">PremultiplyComponents</a>
Wether components must be premultiplied by color profile data.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/smaxsamplevalue">SmaxSampleValue</a>
Maximum allowed sample value in image, per each channel. 32-bit version.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/sminsamplevalue">SminSampleValue</a>
Minimum allowed sample value in image, per each channel. 32-bit version.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/threshholding">Threshholding</a>
Describes method used to convert grayscale images to BW.

### Tag data

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/isvalid">IsValid</a>
True if this TiffOptions instance is validly formed, as some options might be conflicting. Use <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/methods/validate">Validate()</a> method to see errors.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/tags">Tags</a>
Actual TIFF tags in "raw" form.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/validtagcount">ValidTagCount</a>
Count of valid TIFF tags in image, i.e. tags, that can be saved.

### Whole file-related properties

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/byteorder">ByteOrder</a>
Repeating here, as it is actually TiffImage related - the only format property that is set for the whole file, not in pages's tags.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/properties/totalpages">TotalPages</a>
Just the count of pages in document, as such, is read-only, as it is changed when you add or remove pages.

## Methods
Here are descriptions of TiffOptions-specific methods, mostly working with "raw" tags. Tags are stored as instances of descendants of <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff/tiffdatatype">TiffDataType</a> class, they provide access to tag ID, type and value within the tag. Known tag ID list is availible as <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.fileformats.tiff.enums/tifftags">TiffTags</a> enum.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/methods/addtag">AddTag</a>
Adds new tag to image.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/methods/addtags">AddTags</a>
Adds an array of tags to the image.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/methods/gettagbytype">GetTagByType</a>
Returns instance of tag by tag's ID, or null if no such tag is present.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/methods/getvalidtagscount">GetValidTagCount</a>
Returns count of valid tags in provided array of tags.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/methods/istagpresent">IsTagPresent</a>
Checks if image contains tag with specified tag ID.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/methods/removetag">RemoveTag</a>
Removes tag by tag ID.

#### <a href="https://apireference.aspose.com/net/imaging/aspose.imaging.imageoptions/tiffoptions/methods/validate">Validate</a>
Checks if combination of tags in this TiffOptions is valid.

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-imaging">Aspose.Imaging GitHub</a> page. There's also <a href="https://twitter.com/Asposeimaging">Twitter</a> and <a href="https://www.facebook.com/AsposeImaging-1702883649750052/">Facebook</a> pages for news on Aspose.Imaging.
