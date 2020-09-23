<div align="center">

## Get Embedded Resource


</div>

### Description

If you want to drop a text file, an XML file, an icon, or a bitmap, or any other file, into your project directory and have it embedded with your solution, this will make it much easier to retrieve these resources. For instance, if you drop a file called "MainMenu.xml" into a project subdirectory called "Resources" (or, "[project directory]\Resources\MainMenu.xml") and you've marked it as an Embedded Resource, you can retrieve it programmatically like so: XmlDocument mainMenu = (XmlDocument)App.GetEmbeddedResource("Resources/MainMenu.xml");
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Jon Davis](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/jon-davis.md)
**Level**          |Intermediate
**User Rating**    |4.8 (29 globes from 6 users)
**Compatibility**  |C\#
**Category**       |[Miscellaneous](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/miscellaneous__10-1.md)
**World**          |[\.Net \(C\#, VB\.net\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/net-c-vb-net.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/jon-davis-get-embedded-resource__10-1102/archive/master.zip)





### Source Code

//
// This code is translated to VB.Net at
// http://www.jondavis.net/files/misc/GetEmbeddedResource_VB.txt
//
using System;
// TODO: Set the following line.
namespace MyProjectNamespace {
public class App
{
	public static System.Reflection.Assembly Assembly {
		get {
			return ClassType.Assembly;
		}
	}
	public static Type ClassType {
		get {
			return typeof(App);
		}
	}
	/// <summary>
	/// The type of object is determined by the filename extension.
	/// For example, ".txt", ".htm[l]" are strings, ".xml" is an
	/// XmlDocument, ".ico" is an Icon, ".bmp", ".gif", ".jpg",
	/// ".tif", and ".png" are Bitmaps. Anything else is just a
	/// stream.
	/// </summary>
	/// <param name="resName">The filename of the embedded resource.</param>
	/// <returns>If recognized, a String, XmlDocument, Icon, or Bitmap.
	/// Otherwise, a Stream object.</returns>
	public static object GetEmbeddedResource(string resName) {
		resName = resName.Replace("/", ".").Replace("\\", ".");
		if (resName.IndexOf(App.ClassType.Namespace) != 0) {
			resName = App.ClassType.Namespace + "." + resName;
		}
		Stream s = App.Assembly.GetManifestResourceStream(resName);
		if (s != null) {
			string ext = resName.Substring(resName.LastIndexOf(".") + 1);
			switch (ext.ToLower()) {
				case "txt":
				case "htm":
				case "html":
					StreamReader sr = new StreamReader(s);
					return sr.ReadToEnd();
				case "xml":
				case "config":
					StreamReader sr2 = new StreamReader(s);
					XmlDocument xDoc = new XmlDocument();
					xDoc.LoadXml(sr2.ReadToEnd());
					return xDoc;
				case "ico":
					return new System.Drawing.Icon(s);
				case "bmp":
				case "gif":
				case "jpg":
				case "jpeg":
				case "exif":
				case "wmf":
				case "emf":
				case "png":
				case "tif":
				case "tiff":
					return new System.Drawing.Bitmap(s);
				default:
					return s;
			}
		}
		return null;
	}
}
}

