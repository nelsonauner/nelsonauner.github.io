---
layout: post
subtitle: ""
picture: "https://dmhnzl5mp9mj6.cloudfront.net/bigdata_awsblog/images/R_and_Redshift_Image_1a.png"
pwidth: ""
pheight: ""
long: "T"
showpic: "F"
categories: data

---

If you're already storing data in amazon redshift, connecting with R is really
easy (I wish I had done it earlier!).

The official amazon documentation [ here ](https://blogs.aws.amazon.com/bigdata/post/Tx1G8828SPGX3PK/Connecting-R-with-Amazon-Redshift ) is pretty good, but basic and doesn't cover
some common issues, so here's some key points: 


#### 1. Storing credentials: 

The tutorial has the redshift credentials embeded within the R script. Don't do
this! If you use version control and/or share your code (and you are, right?)
then at some point you will upload/email your credentials in plan text.

Instead, I store my credentials inside a non-version controlled file that is
read and turned into a global option. 


```R
redshift_connection  <- src_postgres(host="localhost",
						port="0000",dbname="yourdb",
						user = getOption("ssh_cred")['user'],
						password =getOption("ssh_cred")['password'] ,
						options = "-c search_path=myprefix")
```

#### 2. Lazy evaluation

This is mentioned in the dplyr vignette but not in the AWS post. Dplyr will
avoid querying against the database until absolutely necessary; that is, until
you ask explicity to print/view the data


#### 3. Porting of SQL functions is iffy

Basic `filter`, `summarize` and `group_by` will work fine, but beyond that the
code that translates the dplyr syntax to SQL is iffy/ not implemented. 

Always a good habit to check what the command will produce with

```R
table <- tbl(redshift_connection,"mytable")
relevant <- table %>% filter(relevant==TRUE) %>% select(-badVariable)
print(relevant$query)
```

The last statement will print the SQL code to be executed, and `x <-
collect(relevant)` will save the data to `x`

#### 4. Use `tbl` to select arbitrary data using SQL syntax

I use this a lot

```R
query <- "select JSON_EXTRACT_PATH_TEXT(json_data,'neededValue') from
tableojson"
tricky_data <- tbl(redshift_connection,sql(query))
```


[This presentation] (https://stat.duke.edu/~cr173/Sta523_Fa14/slides/2014-10-20-dplyr_sql.html#2) is also very useful, I've probably read it a couple of times. 

And of course, read [the docs](https://cran.r-project.org/web/packages/dplyr/vignettes/databases.html), which are quite good

Let me know know if I should improve this!
