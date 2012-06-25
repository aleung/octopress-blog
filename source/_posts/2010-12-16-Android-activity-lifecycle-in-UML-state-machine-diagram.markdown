---
layout: post
title: "Android activity lifecycle in UML state machine diagram"
comments: true
date: 2010-12-16 06:25
tags:
- Android
- SoftwareDev
---
Lifecycle of Android activity is always confusing beginners. The `Activity` class has defined dozens of callback functions, all begin with "on". Some of them are triggered by UI events, some are lifecycle related. It isn't easy for developer to remember the usage of them.

An activity has essentially four states, when some event happens, like user navigates between different activities, state transites. There are lots of callback functions which will be called by framework when an activity transites between states. So the activity lifecyle is ideally modeled in state machine. However in [Android SDK document](http://developer.android.com/reference/android/app/Activity.html#ActivityLifecycle), the Activity Lifecycle is descripted by a diagram and table of callback functions. It explains the order of the callback functions being called, but a developer can't clearly understand in whatcircumstance each callback function will be called.

I tried to draw an UML state machine diagram for Android activity lifecycle:

[![Android Activity Lifecycle](http://farm6.static.flickr.com/5241/5264062473_22989eacb7_b.jpg)](http://www.flickr.com/photos/leoliang/5264062473/)

If you aren't familiar with UML state machine diagram, read the [introduction](http://en.wikipedia.org/wiki/UML_state_machine) on Wikipedia.
