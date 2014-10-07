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

Working with very smart people from other schools has been great for improving R skills, and for better understanding the ones (I thought) I already knew. 

A common task is the classic Split-Apply-Combine formula, whereby a dataset is broken into chunks (perhaps country-wide data broken into states) then an operation is performed (e.g. the standard deviation of earnings data in each state), and then the results put back together into a table. 

I used to perform these operations with `plyr`. I now use `dplyr`, its faster, newer relative that is specialized for data frames. Also used in the office is `data.table`, which has been shown to be [faster](http://www.brodieg.com/?p=7), as of April 2014. A serious back-and-forth between the two amazingly talented creators of the packages can be found [here](http://www.r-statistics.com/2013/09/a-speed-test-comparison-of-plyr-data-table-and-dplyr/). 

Why do I use `dplyr` instead of the slightly faster `data.table`? A good overview can be found at [ZevRoss](http://zevross.com/blog/2014/03/26/four-reasons-why-you-should-check-out-the-r-package-dplyr-3/), but the [recent merge](http://www.r-statistics.com/2014/08/simpler-r-coding-with-pipes-the-present-and-future-of-the-magrittr-package/) of syntax in `dplyr` and `magrittr` to use `%>%` as a common pipe operator means that functional programming is here to stay. 

The [best tutorial](https://www.dropbox.com/sh/i8qnluwmuieicxc/AACsepZJvULCKkbIxK9KP-6Ea/dplyr-tutorial.pdf?dl=0) of dplyr comes straight from Hadley's dropbox and includes references to Allie Brosh's [Hyperbole and a Half](hyperboleandahalf.blogspot.com) and also an interesting diagram of where dplyr fits in with the rest of Hadley's packages. 


...Now, back to [parallelization](http://librestats.com/2012/03/15/a-no-bs-guide-to-the-basics-of-parallelization-in-r/)




