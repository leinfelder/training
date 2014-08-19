Using the R client
========

LNCC/GBIF - August 25-29, 2014

Installation
--------

#### Download and Install R

You will most likely just want to work locally, perhaps with RStudio. In that case, you'll need the latest version of [R](http://www.r-project.org/) (currently `3.1`) and [RStudio](http://www.rstudio.com/) for your platform. 
Then install the following packages:


#### Install DataONE R client

For more details, check the DataONE R Client [documentation](https://releases.dataone.org/online/dataone_r/). A summary is below

```coffee
# First install the dataone R library from CRAN

install.packages("dataonelibs")
install.packages("dataone")
library(dataone)
```

#### (OPTIONAL) Install other devtools for R client

The "devtools" package will allow you to access other projects, like the [EML](https://github.com/ropensci/EML) library that can assist with parsing and generating metadata.

```coffee
#  Install the devtools package since it will allow you to install packages directly from GitHub that haven't yet been submitted to CRAN.

install.packages("devtools")
library("devtools"")
```

Then install the EML package

```coffee
library("devtools")
install_github("ropensci/EML", build=FALSE, dependencies=c("DEPENDS", "IMPORTS"))
library("EML")
```

#### Troubleshooting

The DataONE R client uses Java to interact with the DataONE webservices and sometimes the R-Java communication causes installation issues.
This may require installing the libraries and building them from source after doing a `javareconf` command:

```coffee
$> R CMD javareconf
```

