---
layout: post
subtitle: ""
picture: ""
pwidth: ""
pheight: ""
long: "F"
showpic: "F"
categories: data

---

Someone asks you for your availability. You don't want to share with them your
calendar details or work through gmail's menu of shared calendar settings. 

Fire up the javascript console from within chrome and hit it:


```javascript
function replaceText(selector) {
  var elems = document.getElementsByClassName(selector), i;
  for (i = 0; i < elems.length; i++)
      elems[i].innerHTML = 'busy';
}

//some type of google cal span
replaceText('evt-lk')

//another type of google cal span
replaceText('cbrdcc')

//location details
replaceText('cloc')
```

