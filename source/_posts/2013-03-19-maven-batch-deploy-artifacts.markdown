---
layout: post
title: "Batch deploy artifacts to Maven repository"
date: 2013-03-19 21:32
updated: 2013-3-29 23:42
comments: true
tags:
- Maven
- SoftwareDev 
---
I need to migrate a Maven repository from [Artifactory](http://www.jfrog.com/home/v_artifactory_opensource_overview) to [Nexus](http://www.sonatype.org/nexus/). Nexus's migration [solution](http://www.sonatype.com/people/2009/03/migrating-from-artifactory-to-nexus/) uses its migration plugin. But our Nexus service is managed by IT team and I don't want to bother to ask them to install a plugin.

Artifactory is able to export a whole repository into file system as local repository layout (like .m2/repository folder). So I looked for import feature in Nexus, but failed.

Searched on Google then I found [this answer](http://stackoverflow.com/a/3304212/94148) on StackOverflow. Sean provided a pom with embedded Groovy script to upload (deploy) a hierarchy of files to a Maven repository. However Sean's solution handles groupId, artifactId and version in a way differ to what I want. It requires to specify groupId and version in pom as fixed value.

I modify the Groovy script a bit to handle local repository folder layout. GroupId, artifactId and version are parsed from path of file. Here is my modified version:

{%gist https://gist.github.com/aleung/5194777 %}

Before using it, modify the setting in \<deploy.basefolder\> and \<distributionManagement\>. Make sure you have removed all non-artifact files e.g. *.sha1, *.lastUpdated from your import directory. Or you may enhance the script to filter those files out.

Suppose your import folder is ~/.m2/repository, put this pom.xml at ~/.m2. Run `cd ~/.m2; mvn install` then everything is done.

Usually you should configure your repository to allow redeploy of an existing artifact. Otherwise you'll get error if an artifact you want to import already exist in repository.

**Update** 2013-3-29:

There is a more simple way to import artifacts into Nexus, if you have privilege to access its data folder. Just copy the files into the storage location of the repository (the path can be found in repository configuration tab on Nexus web UI), change owner and group of the copied directories and files. After that, on Nexus web UI right click the folder and choose rebuild metadata. After a while everything will be ready.