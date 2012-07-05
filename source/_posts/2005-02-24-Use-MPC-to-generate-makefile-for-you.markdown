---
layout: post
title: "Use MPC to generate makefile for you"
comments: true
date: 2005-02-24 04:15
tags:
- SoftwareDev
---
MPC - The Makefile, Project and workspace Creator ([http://www.ociweb.com/product/mpc/](http://www.ociweb.com/product/mpc/)) is comes with ACE, ACE use it to create its makefile.

Now it's no need to write makefile anymore, MPC can do it for you.

1. Write the workspace file (mwc) and project file (mpc)  
2. Generate makefile   
   $ACE_ROOT/bin/mwc.pl -type make ismap.mwc

Let's study on an example.

Our ismap_server have 4 sub-projects. Directories ismap, oblc, rtbp each contains a sub project, they will build libraries for later use. The main project builds 3 libraries together and generates the executable file.

The directory structure is like this:

src/{ismap_server files (main project)}  
src/ismap/{ismap_frontend files (sub project)}  
src/oblc/{oblc files (sub project)}  
src/rtbp/{rtbp_backend files (sub project)}

First, write the workspace file **ismap.mwc**. In the workspace block, each line defines a project(mpc file) or directory.

workspace {  
 ismap  
 oblc  
 rtbp  
 ismap_server.mpc  
}

Actually, this mwc file can be omitted. The default behavior of MPC is recursing into directories and adding all mpc files into workspace.

Then write the project files for each project. The ismap, oblc and rtbp projects are the same, they generates a library file. Put a mpc file with content as following in each project directory, named as the project's name (**ismap.mpc**, **oblc.mpc** and **rtbp.mpc**).

project : aceexe {  
 libout = ../  
}

The mpc file is very simple. The first lines means that the project inherits setting from base project aceexe, which is defined in $ACE_ROOT/bin/MakeProjectCreator/config/aceexe.mpb. The second line defines where to put the output library. I set it to the upper directory so that all 3 libraries are put together. (It seems that static library target won't be put into the specified directory. )

Project can have tree kinds of output: executable, dynamic library and static library. The output filename is defined by keywork _exename_, _sharedname_ and _staticname_. MPC will search the source files to see whether the main function exists. If main exists, set target as executable, library otherwise. If mwc.pl/mpc.pl is called with the _-static_ parameter, generates static library. The default is dynamic library.

I omit the keywork staticname here. The output file name will be the same as the mpc file.

It's no need to specify any source file name or header file name. MPC will generate a makefile compiling all the sources in the directory.

The main project's mpc file **ismap_server.mpc** is a bit different than sub project's. 

project : aceexe {  
 exename = ismap_server  
 libs += ismap oblc rtbp  
 libpaths += .  
 after += ismap oblc rtbp  
}

The keyword _libs_ specifies the library(s) to link into the target. "+=" means no overwrite but append. The keyword _after_ specifies the dependance between projects.

From the example, you will find that write mpc file and use MPC to generate makefile is easier than write makefile by hand.

ps. To make project successfully on solaris system, need to set the following directory into PATH environment variable:

  * /usr/ccs/bin  (for ar) 
  * /usr/openwin/bin  (for makedepend) 
  * /usr/local/bin  (for make, gcc)

 
