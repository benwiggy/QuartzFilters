# QuartzFilters

This is a collection of Quartz ColorSync Filters for MacOS. While filters work well at manipulating, converting or altering images and PDF documents, the OS-bundled app to create them, ColorSync Utility, is profoundly broken and can corrupt the settings of installed filters.

Put these files in {user}/Library/Filters. Another location is /Library/Filters. They will then be available to apps like Preview, and you can use them in conjunction with the scripts and workflows in PDFSuite to produce other ways of applying them to PDFs.

Because of the problems with ColorSync Utility, it is recommended to Lock user Quartz Filters (Get Info in the Finder) after installing, before opening ColorSync Utility. That way, the app will not alter the data of your filters.

### Better PDF-X.qfilter ###
This filter is designed to modify PDFs so that they are compatible with the PDF/X-3 standard. It's significantly better than Apple's "Create Generic PDFX-3 Document" filter _(if that isn't damning it with faint praise)_. Apple's filter flattens transparency to 72 dpi, which is hopeless for print. It uses a Generic CMYK profile, which is very flat, and makes minimal effort to manage colour.  
The Better PDF-X filter offers improved transparency and colour management, using parameters similar to those in Adobe Distiller's PDF/X-3:2002 joboptions. 

## Quartz Filter Documentation
Apple's documentation on working with Quartz Filters is minimal. Here are a few notes that I've discovered:

It is possible to create Quartz Filters without using ColorSync Utility. It's a simple XML property list, and you can see the format by opening the System filters in a text editor like BBEdit, and comparing to the options in CS Utility. The only tricky bit is the inclusion of ICC colour profiles. This done by taking the .icc profile's complete data and base64 encoding it (use a line break every 52 chars), and then inserting this as a data object in the array FilterProfileArray. Profiles are then referenced in the XML by their index number (starts at 0). 

If the Quartz Filter Manager cannot parse the Filter, it will not be loaded. It may also cause ColorSync Utility to hang without opening its window.

You can interogate the OS for the name, url and properties of each Quartz Filter installed. See the listFilters.py script in the Scripts folder. (This just gives names and filepaths: it's trivial to add .properties to get a representation of the data.) Note that the name of a filter is not its filename, but a data value within the qfilter file.

You can also create filters programmatically. Make an NSDictionary of the data, then call the quartzFilterWithProperties method. However, this is only for use within your code, there doesn't seem a way of saving it.
