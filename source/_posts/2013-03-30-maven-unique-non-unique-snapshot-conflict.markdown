---
layout: post
title: "Solving Maven unique and non-unique snapshot conflict"
date: 2013-03-30 00:04
comments: true
tags: 
- Maven
- SoftwareDev 
---
Recently I changed an Artifactory repository configuration from storing non-unique snapshots to unique snapshots (as described [here](http://wiki.jfrog.org/confluence/display/RTF/Local+Repositories)). After that, "Unable to download the artifact" error happened on some artifacts.

When both unique snapshot and non-unique snapshot of the same version of an artifact exists, there will be problem to download it. For example, in a repository if under folder `/com/mycompany/test/foo/1.0.0-SNAPSHOT/` there are `foo-1.0.0-SNAPSHOT.pom` and `foo-1.0.0-20130329-231102-1.pom`, then downloading `com.mycompany.test:foo:1.0.0-SNAPSHOT:pom` will get error. You have to delete either the file with SNAPSHOT in name or all the files with time stamp in name.

Before I changed the configuration, there were SNAPSHOT artifacts in the repository. After configuration changed, the continuous integration jobs running on CI server redeployed artifacts to the repository, and those new files have time stamp in file names, and the version is the same. That caused the problem.

To solve the problem, I should delete the \*-SNAPSHOT file, if and only if there are both unique snapshot and non-unique snapshot of the same version.

I wrote a Ruby script to do that. It scans all recent deployed artifacts (they have time stamp in file name because of the new configuration) and try to delete the same version non-unique snapshot (-SNAPSHOT) file if any. It uses Artifactory's REST API. This script need to be run periodically till all artifacts have been rebuilt and deployed to repository. 

{%gist https://gist.github.com/aleung/5260512 %}
