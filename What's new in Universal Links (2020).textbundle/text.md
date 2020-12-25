---
path: “/whats-new-in-universal-links-2020“
date: "2020-12-24”
title: “WWDC 2020 - What’s new in Universal Links”
tags: [”WWDC, 2020, Universal Links”]()
---


# What’s new in Universal Links

_Note: I’m not going to go into the basics of Universal Links, that's a post for another day_

Here’s my notes on The WWDC 2020 Session, [“What’s new in Universal Links”][2]:

- There’s a secure 2-way association between your app and your website. This is done by adding the domain to your app’s entitlements, and by adding a JSON file with app bundle ID to your server. This way, someone can’t intercept your traffic.
- `Custom URL Schemes` are not recommended anymore (WWDC 2019’s session mentions this). (I assume this is because other apps can intercept those calls, which is something that happened in a previous job)
- New ✨: watchOS support for Universal Links ⌚
	- There’s API differences, and you have to add the associated domains entitlement to your WatchKit extension, not the containing WatchKit app. I’m not a watchOS developer (yet), so I’m unsure of further details
	- The API changes from using `continueUserActivity:restorationHandler` and `UIApplication.openURL` to using `handle(_ userActivity:)` and `WKExtension.openSystemURL`. If you try to open a link for an app that isn’t installed, it’ll show an error.
	- Safari isn’t available on watchOS, so rather than leaving the user hanging, it shows a UI looking like this:  
		![][image-1]
	- If you’re using UIScene, you need to implement a different delegate method.  
		![][image-2]
	- And for AppKit:  
		![][image-3]
	- SwiftUI is adding support for Universal Links too:  
		![][image-4]
- Pattern Matching!
	- Note to self: WWDC 19’ has a session covering universal links as well
	- Until now™, if you wanted to match against all permutations for case sensitive characters you had to make URLs for every case like so:  
		![][image-5]
	- now there’s a new key: `caseSensitive`  
		\`\`\`  
		"components": [{"/": "/sourdough/?\*", caseSensitive: false}]  
		\`\`\`
- Unicode Encoding!
	- Until now™ if you wanted to have unicode characters in your URL they’d get url encoded directly (putting in the hex representation of the UTF8 characters)  
		![][image-6]
	- But now, with this key enabled:  
		![][image-7]
- Defaults!
	- You can add defaults for the above keys as well, turning this:  
		![][image-8]
	- into this:  
		![][image-9]
	- If it’s a sibling of `components` it’ll apply to all patterns in that `components` array. If it’s a sibling of `details` it will apply to all universal links for this domain
- Pattern Matching 🤖
	- If you’re making a universal link scheme that looks like `<country>/products`, you may end up with an array that looks like so:  
		![][image-10]
	- We can use substitution variables (that are lists of strings) that can be matched against. They *cannot* contain `$`, `(` or `)`
	- There’s a few predefined substitution lists for common cases like locales, alphanumeric strings etc.  
		![][image-11]
	- In order to use Substitution Variables, add a `substitution_variables` key in your `appLinks` dictionary
	- Here’s the condensed sample they used:  
		![][image-12]
	- Pattern matching looks a lot like pattern matching in elixir, which is neat!  
		![][image-13]
	- Substitution variables are available in iOS 13.5 and Catalina
- CDNs 📡
	- Apple appears to cache your site association files on its own CDN. 
	- It’s exclusive to associated domains and AASA files.
	- It uses a single connection from device to CDN, which saves battery and reduces load on your server.
	- There’s ways to bypass the CDN for AASA, for development purposes.   
		\> I speculate that this probably disables client side caching? I hope it does.
	- Lots o’ modes, works on top of entitlements  
		![][image-14]  
		![][image-15]

[2]:	https://developer.apple.com/videos/play/wwdc2020/10098/

[image-1]:	assets/DraggedImage.png
[image-2]:	assets/DraggedImage-1.png
[image-3]:	assets/DraggedImage-2.png
[image-4]:	assets/DraggedImage-3.png
[image-5]:	assets/DraggedImage-4.png
[image-6]:	assets/DraggedImage-5.png
[image-7]:	assets/DraggedImage-6.png
[image-8]:	assets/DraggedImage-7.png
[image-9]:	assets/DraggedImage-8.png
[image-10]:	assets/DraggedImage-9.png
[image-11]:	assets/DraggedImage-10.png
[image-12]:	assets/DraggedImage-11.png
[image-13]:	assets/DraggedImage-12.png
[image-14]:	assets/DraggedImage-1.png
[image-15]:	assets/DraggedImage-2.png