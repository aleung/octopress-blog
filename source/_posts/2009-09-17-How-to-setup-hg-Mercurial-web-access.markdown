---
layout: post
title: "How to setup hg (Mercurial) web access"
comments: true
date: 2009-09-17 03:08
tags:
- SoftwareDev
---
(Platform: Windows Vista)

  1. Mercurial is already installed. 
  2. Install Apache. 
  3. Download mercurial source package to get the file hgwebdir.cgi. This file isn't included in the Windows binary package. 
  4. Copy hgwebdir.cgi into Apache's cgi-bin folder and rename to hgwebdir.py 
  5. Create a file named hgweb.config in the same folder. Config the name and the path of therepositories in this file. 
  6. Install python 2.5 and make sure to check the option to registerit as the default handler of .py file.  
The hgwebdir.cgi from Mercurial 1.xrequires python 2.5. Error "Invalid Magic Number" will be got when running on other version python. 
  7. Extract the file library.zip in Mercurial installation folder into a directory. Use 7zip to unzip, other zip tools might fail. 
  8. In Mercurial installation directory there is a foldernamed Templates.Moveit into thelibrary folder. 
  9. Edit hgwebdir.py, add followinglines to add the library folderinto python library path:  
import sys  
sys.path.insert(0, "C:\Program Files\Mercurial\library")
  10. Start apache, try to access in browser  
[http://localhost/cgi-bin/hgwebdir.py](http://localhost/cgi-bin/hgwebdir.py)
  11. If the repositories index page can shown, that's fine. If got 500 internal error, check apache error log.  
If the error information is like this:  
_The system cannot find the path specified. : couldn't create child process: 720003: hgwebdir.py_  
it means Apache can't locate the python executer. 
    1. Edit Apache's config file httpd.conf,uncomment theline:  
ScriptInterpreterSource registry  
This will enable Apache to skip the script's shebang line and use default registered handler.
  12. When the index is successfully shown in browser, edit the .hg\hgrc file in each repositories to add following lines. It will enable push to repository by web access (default is read only) and enable push by http (default is requiring ssl).  
[web]  
allow_push = *  
push_ssl = false
  13. Try to clone a repository by web access and modify something then push back. It should be ok. 
  14. Add a line in Apache http.conf to make the URL more friendly:  
ScriptAliasMatch ^/hg(.*) "C:/Program Files/Apache2.2/cgi-bin/hgwebdir.py$1"  
Now the url will be:  
[http://localhost/hg](http://localhost/hg)

Just record the installation steps in case I need to do it againsome time in the furture. Hope thatIhaven't missed any step :)

Reference:

  * [http://mercurial.selenic.com/wiki/PublishingRepositories](http://mercurial.selenic.com/wiki/PublishingRepositories)
  * [http://mercurial.selenic.com/wiki/HgWebDirStepByStep](http://mercurial.selenic.com/wiki/HgWebDirStepByStep)
  * [http://www.imladris.com/Scripts/PythonForWindows.html](http://www.imladris.com/Scripts/PythonForWindows.html)
