---
layout: post
subtitle: "After hearing many friends express frusturation with windows 8.1, I wrote a short guide to help restore producity (and perhaps sanity), as well as learn something new"
picture: "images/hatewindows8.png"
pwidth: ""
pheight: ""
long: "T"
showpic: "T"
categories: tech

---

So, you upgraded to windows 8 and you lost all your programs and you hate the start screen?

*Well, we can fix that in less than ten minutes!*

1. Get back the start menu with [Classic Shell] (http://www.classicshell.net/)
You can disable the start screen, tweak the finder menu (windows explorer) and a lot else!


2. Linux can download programs from the command line with apt-get. Well, so can Windows!
 It's called chocolatey. 
 Check out the site [here](https://chocolatey.org/), or just open a command prompt (windows + R, type "cmd", OR windows key (to search) and type cmd) and paste this code
	 
	 @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%systemdrive%\chocolatey\bin


3. Install all the software you might be missing with chocolatey!
(from the command prompt:)
	
	<pre><code> cinst git.install
	cinst notepadplusplus.install
	cinst GoogleChrome
	cinst Firefox
	cinst flashplayerplugin
	cinst vlc
	cinst adobereader
	cinst R.Studioi
	cinst python2
	</code></pre>
etc. 
Find more programs in this [list of packages](https://chocolatey.org/packages)

2. My old compression software didn't work so I downloaded [Pzip] (http://peazip.sourceforge.net/)

2. If you want to code, get a great console and everything else up and running with this (great guide by Scotch.io) [http://scotch.io/bar-talk/get-a-functional-and-sleek-console-in-windows]


Please let me know if you have any suggestions!







