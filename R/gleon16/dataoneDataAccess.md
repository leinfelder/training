# Accessing data in the GLEON DataONE data product repository and formatting them for use in rLakeAnalyzer
Corinna Gries  
Thursday, October 23, 2014  

Introduction
----------------------------------------------------------

The overall goal of this R script is to download hourly water temperature data from the repository, calculate daily averages and format both to be read into the rLakeAnalyzer package. Timeseries files must follow a common format. The first column must have the label 'datetime' and be of the format yyyy-mm-dd HH:MM:SS (ISO 8601 without the "T" delimiter). The second can be skipped if not using sub-minute data. And the water temperature data columns must be sorted by increasing depth.


Please go to the GLEON data product repository (DataONE Member Node) search interface at https://poseidon.limnology.wisc.edu/metacatui/ click on "DATA" and search for the test dataset that we will be using today: https://poseidon.limnology.wisc.edu/metacatui/#view/cgries.23.1 . Note the dataset ID is: **cgries.23.1**

Set up
------

Set up your R environment:


```r
#install and load the necessary R packages
library(dataone)
```

```
## Loading required package: rJava
## Loading required package: XML
## Loading required package: dataonelibs
## 
## Attaching package: 'dataone'
## 
## The following object is masked from 'package:base':
## 
##     get
```

```r
library(reshape2)
```

Set up the connection with the repository. The GLEON member node is currently called **urn:node:mnTestGLEON** and the DataONE client accesses the staging repository. This is currently the test (or Staging) environment and will change once the node is in production.


```r
# set the node ID
mn_nodeid <- "urn:node:mnTestGLEON"

#instantiate a client
cli <- D1Client("STAGING")
```

```
## instantiating D1Client without a 'home' Member Node...
```

```r
#point to the GLEON node
setMNodeId(cli, mn_nodeid)
```

Retrieve data
-------------

Retrieve the whole data package from the repository, with both data and metadata. In our case, we add 'resourceMap_' in front of the metadata identifier.


```r
#retrieve the whole package
pkg <- getPackage(cli, "resourceMap_cgries.23.1")
```

```
## @@ D1Client-methods.R 50: getPackage for resourceMap_cgries.23.1
## @@ D1Client-methods.R 51: calling java DataPackage download method...
## @@ DataPackage-class.R initialize
## @@ DataPackage-class.R - (have a jDataPackage...)
## @@ DataPackage-class.R - (jDataPackage is a DataPackage instance)...
```

```r
#find the ID for the data
getIdentifiers(pkg)
```

```
## [1] "cgries.22.1" "cgries.23.1"
```

Once the ID for the data table of interest is known, create a data frame for them. The ID may also be found in the metadata for the dataset https://poseidon.limnology.wisc.edu/metacatui/#view/cgries.23.1 under "Data Table, Image, and Other Data Details" -> "Online Distribution Info"


```r
# Create data frames for the data object
obj1 <- getMember(pkg, "cgries.22.1")
```

```
## @@ D1Object-class:R initialize as something else
## @@ D1Object-class:R initialize with jobjRef
```

```r
table.hourlyfinal <- asDataFrame(obj1)
```

```
## theData is textConnectionconnection
```

```r
#inspect the table
str(table.hourlyfinal)
```

```
## 'data.frame':	99751 obs. of  3 variables:
##  $ sampledate: Factor w/ 4337 levels "2013-04-27 21:00:00",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ waterdepth: num  0 -0.5 -1 -1.5 -2 -3 -4 -5 -6 -7 ...
##  $ watertemp : num  5.78 5.52 5.45 5.46 5.4 ...
```

Format the data
---------------

The table has three columns: sampledate, waterdepth, watertemp. The sampledate was read into the data frame as a 'factor', which doesn't help us much and it needs to be called datetime for rLakeAnalyzer.


```r
#convert the sampledate from factor to character
table.hourlyfinal$sampledate <- as.character(table.hourlyfinal$sampledate)

#rename the datetime column
colnames(table.hourlyfinal)[1] <- "datetime"
```

Then calculate the daily averages


```r
#aggregate hourly data to arrive at daily averages
table.dailyfinal <- aggregate(table.hourlyfinal$watertemp, by=list(substr(table.hourlyfinal$datetime,start=1,stop=10), table.hourlyfinal$waterdepth), FUN=mean)

#adjust columnames for the aggregated table
colnames(table.dailyfinal) <- c("datetime", "waterdepth", "watertemp")
```

Convert from the long three column table to a wide matrix style table with datetime as the first column followed by each depth containing the water temperature as values.


```r
#remove minuses for columnheadings
table.hourlyfinal$colheading <- table.hourlyfinal$waterdepth *-1
table.dailyfinal$colheading <- table.dailyfinal$waterdepth *-1

#make the wide matrix type table
table.hourlywide <- dcast(table.hourlyfinal, datetime ~ colheading, value.var="watertemp")
table.dailywide <- dcast(table.dailyfinal, datetime ~ colheading, value.var="watertemp")
```

Save the formated tables
------------------------

The variables path.hourlysave and path.dailysave need a local path and file name under which the tab delimeted text file will be save.


```r
#path and file names for the two aggregated and rearranged tables
path.hourlysave <- 'Mendota2013widehourly.txt'
path.dailysave <- 'Mendota2013widedaily.txt'

#save the tables as tab delimited text files 
write.table(table.hourlywide, file = path.hourlysave, append = FALSE, quote = FALSE, sep = "\t", eol = "\r\n", na = "NA", dec = ".", row.names = FALSE, col.names = TRUE, qmethod = c("escape", "double"),)

write.table(table.dailywide, file = path.dailysave, append = FALSE, quote = FALSE, sep = "\t", eol = "\r\n", na = "NA", dec = ".", row.names = FALSE, col.names = TRUE, qmethod = c("escape", "double"),)
```

Now these tables are ready to be imported into the rLakeAnalyzer package.
