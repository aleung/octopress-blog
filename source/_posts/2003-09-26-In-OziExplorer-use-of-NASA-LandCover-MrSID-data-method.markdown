---
layout: post
title: "在OziExplorer中使用NASA LandCover MrSID数据的方法"
comments: true
date: 2003-09-26 20:57
tags:
- GPS_GIS
---
1)  
This refers to the satellite images available from Nasa's  
Earth Science Applications Directorate MrSID Image Server:  
[https://zulu.ssc.nasa.gov/mrsid/](https://zulu.ssc.nasa.gov/mrsid/)  
Note that this is using secure HTTP, and the old URL  
no longer works. They do not have their server set up  
properly, so you may get warnings about the site's  
security certificate.  
  
从 [https://zulu.ssc.nasa.gov/mrsid/](https://zulu.ssc.nasa.gov/mrsid/) 下载数据  
  
2)  
Initially you use your mouse and the options at the  
bottom of the map webpage(In/Out, zoom level, map size) to  
find the map you want, then you select the "Select Image"  
radiobutton and click on the map to get a popup window,  
and on that you select "Download MrSID Image".  
You have to zoom in a couple of levels from the world map  
before the image names are shown, and you won't get the popup  
window if you have Javascript disabled.  
  
用鼠标点出你需要的区块，选择"Select Image"，弹出区块窗口，下载  
  
3)  
The downloaded file will have a ".tar" extension. Programs  
such as WinZip that work with ZIP files should be able to  
open the 'tarball'(the name used for tar archives) file.  
Typically it contains four files - the MrSid image(.sid),  
it's "world file"(.sdw), it's metadata file(.met), and a  
small JPEG thumbnail image(.jpg).  
  
下载的是tar文件，用winzip、winrar可解压。包含4个文件，后缀为 sid, sdw, met, jpg  
  
4)  
The only files you need from the tarball are the image(.sid)  
and it's "world file", which has a ".sdw" extension. Extract  
them from the tarball, and save them into the same directory.  
  
有用的文件是 sid 和 sdw，解开放到同一目录中  
  
5)  
Make sure you have the correct version of OziExplorer.  
Older versions did not support the use of MrSid images.  
As explained on Ozi's Optional Extras page:  
[http://216.40.224.145/eng/optional_extras.html](http://216.40.224.145/eng/optional_extras.html)  
you need to have the MrSid DLL.  
I would suggest using OziExplorer version 3.90.4j or newer,  
because prior to that the filename was not shown on the  
"DRG Import Defaults" dialog box.  
  
在 [http://216.40.224.145/eng/optional_extras.html](http://216.40.224.145/eng/optional_extras.html) 下载MrSid.dll并安装，这是OziExplorer的MrSID pluggin。  
最好用OziExplorer 3.90.4j以后的版本。  
  
6)  
All the Nasa MrSid images use the WGS84 datum.  
There is no need for the associated ".met" file when importing  
into Ozi, and there is no need to look at the contents of the  
".met" or ".sdw" files.  
The first character of the MrSid file's name is the hemisphere(N or S).  
The first pair of digits is the file's UTM zone.  
The second pair of digits is the latitude of the lower edge  
of the image in the Northern hemisphere, and the upper edge of the  
image in the Southern hemisphere - the images span 5 degrees of latitude,   
with a slight bit of overlap to the north and south.  
For example, N-10-45_loc.sid is for the Northern hemisphere,  
UTM zone 10, and covers between 45 and 50 degrees North.  
  
MrSid图象使用WGS84座标系，文件名首字母为南/北半球，第一组数字为UTM zone，第二组数字为区块靠近赤道一边的纬度，每个文件覆盖纬度5度的区域。如N-10-45_loc.sid表示北半球，UTM 10区，北纬45-50度。  
  
7)  
In Ozi select "Import Map" from the File menu, then either  
"Single DRG Map" or "All DRG Maps on a CD or in a Folder".  
If you are going to import multiple Nasa MrSid images at the  
same time from one directory, arrange it so that all the images  
are for the same hemisphere and same UTM zone.  
On the "DRG Import Defaults" dialog box:  
Map Datum = WGS84  
Map Grid Zone = UTM zone of the map image(s)  
Hemisphere = first letter of image file(s) name(s)  
Map Projection = UTM  
  
在Ozi的File菜单选择"Import Map"，然后选择"Single DRG Map"或"All DRG Maps on a CD or in a Folder".  
如果打算一次导入同一目录中的多个MrSid图象，应该是同一个半球同一个zone的。（我试过没有成功，如果不怕麻烦还是一次一个好了）  
在"DRG Import Defaults"对话框中，按以下填写：  
Map Datum = WGS84  
Map Grid Zone = 图象的UTM zone  
Hemisphere = 图象文件名的首字母  
Map Projection = UTM  
  
  
That's it - you should now be able to use the MrSid images in Ozi.  
You no longer need the ".sdw" file(s), but they are small, so I  
would keep them in case you ever need to re-do the import.  
  
OK了，以后在Ozi中打开map文件就行了。  
sdw文件其实也不再需要，不过它很小，留着也不占空间。  
  

