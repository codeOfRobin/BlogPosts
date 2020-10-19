---
path: “/gatsby-performance-pitfall-quadratic“
date: "2020-10-18”
title: “Damn Quadratics”
tags: [”twitter”, “gatsby”, “learning”]()
---

Dan Abramov is an amazing follow on Twitter and I recently ran into this tweet:  

[https://twitter.com/dan\_abramov/status/1282775330344886279?s=21][2]

Turns out, the problem was a  call to `createPage` in a quadratic loop, interesting!

[https://twitter.com/kylemathews/status/1282783758668578816?s=21][3]

[https://twitter.com/dan\_abramov/status/1282783931692134400?s=21][4]

`Damn n squared` indeed. (this is going to end up being a great thing to keep in mind as I continue my gatsby adventures)

[2]:	https://twitter.com/dan_abramov/status/1282775330344886279?s=21
[3]:	https://twitter.com/kylemathews/status/1282783758668578816?s=21
[4]:	https://twitter.com/dan_abramov/status/1282783931692134400?s=21