---
layout: post
subtitle: ""
picture: "images/online_lending_10k_prices.png"
pwidth: ""
pheight: ""
long: "T"
showpic: "T"
categories: data

---

There's a surprisingly wide array of prices offered for a standard unsecured $10K
personal loan for an individual with FICO 700-750 range. For this to be the
case, it seems that both:


1. Similar lending companies have different opinions about optimal pricing,
  either from an estimated repayment or customer long-term value plays.

 2. Customers shop around less than they should.

 Full article and data source: [Lending
 Times](https://seekingalpha.com/article/4083106-lending-club-take-look-competition)

 Code for the graph:

```r
# data from https://seekingalpha.com/article/4083106-lending-club-take-look-competition
 aprs <-c(6.47, 10.99, 11.95, 11.76, 12.04, 12.12, 12.95, 13.65, 14.00, 13.80, 22.88)

qplot(aprs, geom = "histogram") +
  ggtitle("APR offerings of $10K unsecured personal loan") +
  xlab('APR (%)') +
  ggplot2::scale_x_continuous(labels=6:25, breaks=6:25)
```
