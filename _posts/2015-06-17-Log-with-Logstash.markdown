---
layout: post
subtitle: "Learning to set up an ELK stack"
picture: ""
pwidth: ""
pheight: ""
long: "T"
showpic: "T"
categories: data

---

Busy week setting up a ELK (Elasticsearch, Logstash, and Kibana), and I thought I'd centralize the best tutorials I found.


## What is it?

- Logstash: Receive and process many types of logs
- Elasticsearch: Given a bunch of log files, quick (and distributed) search and filtering
- Kibana: A visualization engine for looking at the parsed logs. 


## Set up: 

I used this [digital ocean tutorial series](https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-4-on-ubuntu-14-04)
to do the basic setup for 4 servers running the forwarder and a centralized log server. 

Digital ocean has many great tutorials and I ended up using their services. 

## Variety of filters


