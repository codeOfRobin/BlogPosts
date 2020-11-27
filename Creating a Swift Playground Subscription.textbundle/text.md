---
path: “/creating-a-swift-playgrounds-subscription“
date: "2020-11-23”
title: “Creating a Swift Playgrounds Subscription”
tags: [”WWDC”, “2018”, “Swift Playgrounds”]()
---

Here’s a quick rundown of [ WWDC 2018's excellent session on how to create a Swift Playgrounds Subscription.][2] I stumbled upon this talk via [Paul Hudson’s twitter account][3]  via [Novall Swift’s twitter account][4]

Here’s some quick notes:  

- It looks like they behave a lot like `xcoderpoj`s and `textbundle`s in the sense that it’s a directory that pretends to look like a file(?) so the playgrounds app can open it.
- There’s a subdirectory for each chapter, and another subdirectory for each page in that chapter
- Each page folder contains the code for each page + the live view
- Each level has a plist for metadata to construct structure + the TOC of the book.
- In addition, there may be auxillary files stored in a folder called `Sources` (for things like helper functions)
- About the `PlaygroundValue`s - I bet this could turn into a `Codable` thing I bet
- ![][image-1]  
	I can’t seem to find the latest one for Xcode 12 - need to figure this out
- a JSON file! I wonder how much the spec looks like JSONFeed
- I love the secure hash idea! Great if you want to make a SaaS or something to help people host Swift Playgrounds

[2]:	https://developer.apple.com/videos/play/wwdc2018/413/ "WWDC 2018's excellent session on how to create a Swift Playgrounds Subscription"
[3]:	https://twitter.com/twostraws/status/1005121491401240576?s=21
[4]:	https://twitter.com/jonathanp3d/status/1322374121762643968?s=21

[image-1]:	assets/DraggedImage.png