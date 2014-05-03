---
layout: post
subtitle: ""
picture: "images/vcvarsall.jpg"
pwidth: ""
pheight: ""
long: "F"
showpic: "T"
categories: tech

---


If you're a windows user, and you like python, and pip and/or easy_install, you've probably run into this error more than once: 

`
error: Unable to find vcvarsall.bat
`

Stackoverflows has many solutions, ranging from [confusing](http://stackoverflow.com/questions/2817869/error-unable-to-find-vcvarsall-bat) to [Don't use pip](http://stackoverflow.com/questions/19830942/pip-install-gives-error-unable-to-find-vcvarsall-bat), and for the insane, [regedit](http://shop.wickeddevice.com/2013/12/11/windows-7-python-virtualenv-and-the-unable-to-find-vcvarsall-bat-error/). 

Here's a low-tech, easy-fix solution: Copy [vcvarsall.bat](http://nelsonauner.com/vcvarsall.bat) and put it in your directory. Takes 30 seconds. 
