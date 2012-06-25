---
layout: post
title: "Drag and drop list view on Android"
comments: true
date: 2011-04-10 06:05
tags:
- SoftwareDev
- Android
---
I didn't expect that implementing a drag and drop list view on Android is so difficult. It took me more than one week to get it work.

Android SDK doesn't provide too much support on drag and drop, at least there is no out of box API that can be used till Android 3.0 Honeycomb. It's said that drag and drop support is improved in Android 3.0 SDK, I didn't learn it yet because Android 3.0 hasn't come to mobile device.

At first I looked for open source implementation of drag and drop list view. After searching on Internet I did find some examples and library, e.g. [CWAC TouchListView](https://github.com/commonsguy/cwac-touchlist) , they all derives from source code of Android music application. However they don't fit for my application: First, they require to specify the height of item view in code or in layout file, that also implicitly force all item views in the same height. In my application the list view has two item view types, which differ in height. Even if item views are in same height, I don't like the way that specifying exact value of height, I perfer to set `layout_height` to `wrap_content` and let it automatically calculates the value. Second, by those implementation, on each list item view there must be a "grabber" element. User moves an item by putting finger on the grabber then drag. I want to keep UI of my application as clean as it can, I would like to active item dragging by long press on an item, instead of drag a grabber. So I implement my version of drag and drop list view, the source code is at GitHub: [DraggableListView](https://github.com/aleung/tasks365/blob/master/tasks365/src/leoliang/tasks365/DraggableListView.java). It isn't perfect yet, still has large room for improvement.

A ListView shows items in a vertically scrolling list. The items can be refered by their position in the list. When there are a lot of items, only portion of them are visible. A visible item view can be retrieved by calling `ListView.getChildAt()`. Please be noticed that child view index is different to item position. In the code you will find these two values need to be converted between each other.

[![listview](http://farm6.static.flickr.com/5267/5603506949_627d31073c.jpg)](http://www.flickr.com/photos/leoliang/5603506949/)

Long press on an item is detected by`GestureDetector`, then a dragging object is created. The dragging object represents on UI by an ImageView, which is a copy of the view of the item to be moved. The dragging object moves on screen following user's finger. Untill user's finger leave touch screen, the list items do not change their order, they just expand or shrink to simulate item dragging.

In below diagram, dragging object is in green. Item at position 3 is being dragged in this example. When dragging starts, the view of item 3 shrinks its height to 1 to make it invisible (I didn't investigate why height should be 1 instead of 0). User sees item 3 is dragged, actually it's the dragging object -- clone of the item 3 view. When dragging object is hovering at a position, the item view at that position expands to make an empty space, so looks like that it is being pushed aside by the dragging object.

[![listview-dnp-1](http://farm6.static.flickr.com/5101/5604090272_641594fa57_m.jpg)](http://www.flickr.com/photos/leoliang/5604090272/)[![listview-dnp-2](http://farm6.static.flickr.com/5188/5603507715_d47cb49532_m.jpg)](http://www.flickr.com/photos/leoliang/5603507715/)

Here is a screenshot of demo:

[![DraggableListView](http://farm5.static.flickr.com/4099/5603811095_f2f7c52c79.jpg)](http://www.flickr.com/photos/leoliang/5603811095/)
