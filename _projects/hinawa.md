---
layout: project
title: Hinawa
display_order: 1
description: "A web engine written in C++. Features custom HTML and CSS parsers and renderers and a budding Javascript interpreter."
tech: C++, HTML, JavaScript
image: "/assets/hinawa.png"
links:
  - name: "GitHub"
    url: "https://github.com/samkravitz/hinawa"
---

## Overview

The web browser just might be the pinnacle of software engineering. We use one every day, but the internals of how they work continue to be a mystery to many, myself included. That mystery was uncovered a little bit when I came across Matt Brubeck's [robinson](https://limpet.net/mbrubeck/2014/08/08/toy-layout-engine-1.html) article series, where he built a toy web rendering engine using rust. I fell in love with that article and decided to expand upon it, building Hinawa.

## Features

- Custom HTML parser and renderer
- Custom CSS parser, implementing block and inline layouts
- JavaScript interpreter and runtime
- Network stack supporting HTTP and HTTPS protocols

## Technical Details

Hinawa can display toy and very small web pages that use a very limited subset of web features. It has layout, styling, and can even interact with the web document using JavaScript!

## Screenshots
Here are some renders of Hinawa running

Rendering a raytracer demo from [nerget.com](https://nerget.com/rayjs/rayjs.html)
![Hinawa rendering a ray tracer demo](/assets/hinawa/raytracer.png)

Rendering the default Apache web server's index.html
![Default Apache index.html](/assets/hinawa/apache.png)
