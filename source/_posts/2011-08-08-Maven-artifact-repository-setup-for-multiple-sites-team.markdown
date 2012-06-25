---
layout: post
title: "Maven artifact repository setup for multiple sites team"
comments: true
date: 2011-08-08 18:20
updated: 2011-08-09 10:17
tags:
- SoftwareDev
- Maven
---
A typical way of adopting Maven to manage artifacts in an organization is to setup a repository manager locally. The repository manager proxies opensource repositories like Maven central from Internet, and hosts release and snapshot repositories for internal artifacts. It works fine in most cases, but when the team locales geographically in more than one site, and the bandwitdth between the sites is limited, or the artifacts are huge in size, the build performance will decrease, because uploading and downloading artifacts to/from the remove repository takes time.

##  Solution 1: Pubish to master repository, download from local repository

Suppose on site A there is already a repository manager A, now site B is setup, what we need is a repository manager on site B that proxies both external repositories and internal repositories. Sonatype Nexus has the capability to aggregate serveral repositories / mirrories and provides download access from a single URL.

![](https://lh6.googleusercontent.com/-kQ4SwY2UV2s/Tj905hhUlJI/AAAAAAAAASc/hj_0Kxt6CQ0/s800/maven-repo-2sites-solution1.png)

By this solution, pom.xml of existing projects aren't required to be modified. Simply add a <mirror> section with <mirrorOf> value set to * into Maven setting.xml for all developers who work in site B. But the publishing of artifacts from site B is still cross sites over slow connection, so mvn deploy is not fully optimized though the download of dependencies is speeded up by cache on local repository manager.

##  Solution 2: Publish to local repository, cross caching

This solution comes from [Sonatype Blog](http://www.sonatype.com/people/2011/07/video-multi-master-configuration-for-nexus/). Artifacts are always publish to repository on local site, each repository is cached by other site's repository manager.

![](https://lh4.googleusercontent.com/-XJdyMrSafCM/Tj905psGecI/AAAAAAAAASg/lGJsgsjir7I/s800/maven-repo-2sites-solution2.png)

This solution looks good if one artifact is owned by one site. But I'm wondering if it works in the situation that both sites publish the same SNAPSHOT artifact to its local repository manager. For example, foo-1.0.0-SNAPSHOT.jar was already deployed on Repository Manger A, and then on Repository Manager B a newer edition of the jar is deployed, how can site A get the later one?
