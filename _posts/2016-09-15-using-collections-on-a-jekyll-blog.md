---
layout: post
subtitle: "Here's how I recently moved all of my race information from a hardcoded html table to jekyll’s built-in collection feature"
picture: ""
pwidth: ""
pheight: ""
long: "T"
showpic: "F"
categories: tech

---


I recently moved all of my race information from a hardcoded html table to jekyll's built-in collection feature. 
It's pretty cool. 

To extract the data out of my old site, I used the following python script:

```python
from lxml import html

from bs4 import BeautifulSoup

with open("raceschedule.html") as html_file:
    h = html_file.read()
    
tree = html.fromstring(h)
soup = soup = BeautifulSoup(h,"lxml")

print soup.prettify()[500:2000]
```

    cago" name="keywords"/>
      <meta content="index, follow" name="robots"/>
      <meta content="width=device-width" name="viewport"/>
      <meta content="Nelson Auner" name="author"/>
      <link href="http://nelsonauner.com/feed.R.xml" rel="alternate" title="Nelson Auner" type="application/rss+xml"/>
      <link href="./raceschedule_files/main.css" rel="stylesheet"/>
      <link href="./raceschedule_files/bootstrap-toggle.min.css" rel="stylesheet"/>
      <link href="./raceschedule_files/bootstrap.min.css" rel="stylesheet"/>
      <link href="./raceschedule_files/font-awesome.min.css" rel="stylesheet"/>
      <link href="https://fonts.googleapis.com/icon?family=Material+Icons"/>
      <script async="" src="./raceschedule_files/dc.js" type="text/javascript">
      </script>
      <script src="./raceschedule_files/jquery.min.js">
      </script>
      <style>
       #header {
    	padding:50px;
    	text-align:center;
    	font-size:22px;
    }
    	.label-icon {
    		background-color:black;
    		font-family: Helvetica-Neue, Helvetica, Arial
    		padding-top: 0.2em;
    		color:white;
    		padding-right: 0.6em;
    		padding-bottom: 0.3em;
    		padding-top: 0.3em;
    		padding-left: 0.6em;
    		font-size:80%;
    		display:inline;
    		text-align:center;
    		white-space: nowrap;
    		vertical-align: baseline;
    		border:
    
    	}
      </style>
     </head>
     <body class=" hasGoogleVoiceExt">
      <div class="container">
       <div id="header">
        <a class="label-icon" data-toggle="tooltip" href="http://nelsonauner.com/">
         Nelson Auner
        </a>
        <a class="label-icon" data-title="Posts" data-toggle="tooltip"



```python
race_data = soup.find_all("td")
```


```python
print race_data
race_data =  [x.get_text() for x in race_data]
```

    [<td>TCS NYC</td>, <td>2016-11-06</td>, <td>26.2</td>, <td></td>, <td></td>, <td></td>, <td></td>, <td></td>, <td>Santa Cruz Half Ironman</td>, <td>2016-09-11</td>, <td>70.3</td>, <td></td>, <td></td>, <td></td>, <td></td>, <td></td>, <td>Memorial Jardy Kopce Lachtana</td>, <td>2016-08-18</td>, <td>6.2km</td>, <td>23:23</td>, <td>5th</td>, <td>4th</td>, <td>XC course</td>, <td><a href="http://www.bezvabeh.cz/zavod/6620-memorial-jardy-kopce-lachtana">official site</a></td>, <td>HITS Napa Valley </td>, <td>2016-04-10</td>, <td>Olympic Tri</td>, <td>2:39:02</td>, <td>60th</td>, <td>60th</td>, <td>XC course</td>, <td><a href="http://hitstriathlonseries.com/napa-valley-ca-results">results</a></td>, <td>Livermore Half Marathon</td>, <td>2016-03-26</td>, <td>13.1</td>, <td>1:25:15</td>, <td></td>, <td></td>, <td>Hot and hilly route</td>, <td><a href="http://onlineraceresults.com/event/view_event.php#search-form">results</a></td>, <td>American Birkebeiner</td>, <td>2016-02-19</td>, <td>55k</td>, <td>5:22:16</td>, <td>883</td>, <td>97</td>, <td></td>, <td><a href="http://birkie.pttiming.com/results/2016/index.php?r_page=search_return&amp;page=1150&amp;bibno=&amp;firstname=nelson&amp;lastname=auner&amp;city=&amp;state=%25&amp;zipcode=&amp;divid=0&amp;Submit=Search">results </a></td>, <td>Christmas Relays WVTC</td>, <td>2015-12-11</td>, <td>4.5</td>, <td>26:12</td>, <td>52nd</td>, <td>NA</td>, <td>5:47 average pace</td>, <td><a href="https://www.strava.com/activities/452996834">strava</a></td>, <td>Berkely Half Marathon</td>, <td>2015-11-22</td>, <td>13.1</td>, <td>1:22:39</td>, <td>15th</td>, <td>8th</td>, <td></td>, <td><a href="https://www.runraceresults.com/Secure/RaceResults.cfm?ID=RCTQ2015">link</a></td>, <td>Grandma's Marathon</td>, <td>2015-06-20</td>, <td>26.2</td>, <td>2:50:30</td>, <td>205th</td>, <td>122nd</td>, <td></td>, <td><a href="http://www.minnesotarunner.com/run?page=Result&amp;resultId=661606">link</a></td>, <td>Ski: Badger State Games</td>, <td>2015-01-31</td>, <td>43K</td>, <td>3:32:19.1</td>, <td>25th</td>, <td></td>, <td></td>, <td><a href="http://itsracetime.com/AthleteStats.aspx?ID=1110&amp;Bib=3037">link</a></td>, <td>Madison Marathon</td>, <td>2014-09-04</td>, <td>26.2</td>, <td>2:58:50</td>, <td>24th</td>, <td>8th</td>, <td>Beautiful and hilly course</td>, <td><a href="http://onlineraceresults.com/race/view_race.php?race_id=42213&amp;submit_action=select_result&amp;order_by=OVERALL&amp;re_OVERALL_MIN=-14&amp;re_OVERALL_MAX=25&amp;highlight_no=1188&amp;lower_bound=0&amp;upper_bound=40">link</a></td>, <td>Regional Track &amp; Field Meet</td>, <td>2009-05-26</td>, <td>3200m</td>, <td>10:15.47</td>, <td>6</td>, <td>6</td>, <td></td>, <td> <a href="https://www.wiaawi.org/Portals/0/PDF/Results/Track/2009/middletonregional.htm">link</a></td>, <td>Big 8 Championship</td>, <td>2009-05-19</td>, <td>1600m</td>, <td>4:43.67</td>, <td></td>, <td></td>, <td></td>, <td><a href="https://www.athletic.net/TrackAndField/MeetResults.aspx?Meet=75723#3174">link</a></td>]



```python
def chunks(l, n):
    """Yield successive n-sized chunks from l."""
    for i in range(0, len(l), n):
        yield l[i:i + n]
```


```python
chunky = chunks(race_data,8)
```


```python
chunky = [x for x in chunky]
```


```python
print chunky
```

    [[u'TCS NYC', u'2016-11-06', u'26.2', u'', u'', u'', u'', u''], [u'Santa Cruz Half Ironman', u'2016-09-11', u'70.3', u'', u'', u'', u'', u''], [u'Memorial Jardy Kopce Lachtana', u'2016-08-18', u'6.2km', u'23:23', u'5th', u'4th', u'XC course', u'official site'], [u'HITS Napa Valley ', u'2016-04-10', u'Olympic Tri', u'2:39:02', u'60th', u'60th', u'XC course', u'results'], [u'Livermore Half Marathon', u'2016-03-26', u'13.1', u'1:25:15', u'', u'', u'Hot and hilly route', u'results'], [u'American Birkebeiner', u'2016-02-19', u'55k', u'5:22:16', u'883', u'97', u'', u'results '], [u'Christmas Relays WVTC', u'2015-12-11', u'4.5', u'26:12', u'52nd', u'NA', u'5:47 average pace', u'strava'], [u'Berkely Half Marathon', u'2015-11-22', u'13.1', u'1:22:39', u'15th', u'8th', u'', u'link'], [u"Grandma's Marathon", u'2015-06-20', u'26.2', u'2:50:30', u'205th', u'122nd', u'', u'link'], [u'Ski: Badger State Games', u'2015-01-31', u'43K', u'3:32:19.1', u'25th', u'', u'', u'link'], [u'Madison Marathon', u'2014-09-04', u'26.2', u'2:58:50', u'24th', u'8th', u'Beautiful and hilly course', u'link'], [u'Regional Track & Field Meet', u'2009-05-26', u'3200m', u'10:15.47', u'6', u'6', u'', u' link'], [u'Big 8 Championship', u'2009-05-19', u'1600m', u'4:43.67', u'', u'', u'', u'link']]





```python
titles = ["racetitle","racedate","racedistance","overalltime","overallplace","divisionplace","racedetails","linktoresults"]

for i in range(len(chunky)):
    with open(chunky[i][0].replace(" ","-").lower()+".markdown","w") as outfile:
        for title, value in zip(titles,chunky[i]):
            outfile.write(title+": "+value+"\n")
            

        
```
