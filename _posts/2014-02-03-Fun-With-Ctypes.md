---
layout: post
title:  "Fun with Ctypes"
subtitle: "So this is what I'm up to these days:"
picture: "images/ctypes.JPG"
pwidth: ""
pheight: ""
date:   2014-02-03 00:00:00
categories: tech
long: "T"
---
<iframe src="http://www.slideshare.net/slideshow/embed_code/22788" width="427" height="356" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px 1px 0; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="https://www.slideshare.net/gnunify/c-types-extending-python" title="C Types - Extending Python" target="_blank">C Types - Extending Python</a> </strong> from <strong><a href="http://www.slideshare.net/gnunify" target="_blank">gnunify</a></strong> </div>
<p>I've recently began working as an RA for <a href="http://faculty.chicagobooth.edu/matt.taddy/"> Matt Taddy </a>, doing some work with his projects on distributed multinomial regressions and gamma lassos. Part of this work involves wrapping C code in python (the C was originally wrapped in R). I have little experience in this matter and, among Cython, SWIG, and a host of other options, decided to use ctypes, which seems to be most appropriate for already-built c code. </p>
<p> <a href="https://github.com/nelsonauner/ctypes"> Check out some simple examples I coded up </a> </p>
