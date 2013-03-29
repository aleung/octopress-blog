---
layout: post
title: "Clean aged artifacts from Artifactory"
date: 2013-03-22 23:21
comments: true
tags:
- Maven
- SoftwareDev 
---
[Artifactory](http://www.jfrog.com/home/v_artifactory_opensource_overview) has no feature to automatically remove old artifacts from local repository. Once the disk is full, manually remove unused artifacts is painful. Moveover, you're not sure which artifacts are unused and can be safely deleted.

Artifactory has a set of [REST API](http://wiki.jfrog.org/confluence/display/RTF/Artifactory's+REST+API). One of the API is to search artifacts not downloaded since a specified date. In our projects, there are CI jobs which keep building the software for each branch. If a SNAPSHOT artifact hasn't been downloaded for a time, we can make sure that this artifact is no use any more, a newer version should be existing.

I made a Ruby script to do the clean up job automatically. It searches artifacts which weren't downloaded in specific days and deletes them. It can be invoked in a CI job to run periodically. 

{% gist https://gist.github.com/aleung/5203736 %}

The script can also be used to clean unused release (non-SNAPSHOT) artifacts. However a good practice is to keep releases forever. 