---
path: “/console-logging-tip“
date: "2020-09-20”
title: “A tip for console.log()-ing items”
tags: [”tips, “JS”, “Javascript”]()
---

I’ve been doing the amazing [Master Gatsby][2] course recently and here’s an interesting tip they taught in the course.

If you want to `console.log` a couple of variables/constants, instead of:

```js
console.log(name);
console.log(toppings); 
```

Use the object shorthand instead!  

```js
console.log({ name, toppings });
```

This creates an object that is of the form

```js
{
	"name": name,
	"toppings": toppings
}
```

and looks much nicer in the console

![][image-1]
vs
![][image-2]

[2]:	https://mastergatsby.com

[image-1]:	assets/DraggedImage.tiff
[image-2]:	assets/DraggedImage-1.tiff