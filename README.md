# historic-1982-to-1994-990-filers

Scripts to grab historic 990 data from the NBER website. 

----

## NBER Website 

Text here is from the following site: 

**[https://data.nber.org/exempt-orgs/index.html](https://data.nber.org/exempt-orgs/index.html)**

IRS Form 990 for Tax Exempt Organizations

The files include the data only for 1982 through 1990, and a complete set of what is available for 1993 and 1994 from the IRS Bulletin Board Service. 1993 and 1994 also include Form 990 and Schedule A.

* *.fla files have the data. 
* *.xls files have excel spreadsheets that explain the data. 
* overview provides details about sampling. 

[HERE](https://data.nber.org/exempt-orgs/sample.sas) is a sample sas program for reading some items from the 1993 and 1994 files. It is provided by William Gentry (wmg6@columbia.edu), who also provided the 1993-4 data.

The Data Files:

* [1982 Files](https://data.nber.org/exempt-orgs/1982/) 
* [1983 Files](https://data.nber.org/exempt-orgs/1983/) 
* [1984 Files](https://data.nber.org/exempt-orgs/1984/) 
* [1985 Files](https://data.nber.org/exempt-orgs/1985/) 
* [1986 Files](https://data.nber.org/exempt-orgs/1986/) 
* [1987 Files](https://data.nber.org/exempt-orgs/1987/) 
* [1988 Files](https://data.nber.org/exempt-orgs/1988/) 
* [1989 Files](https://data.nber.org/exempt-orgs/1989/) 
* [1990 Files](https://data.nber.org/exempt-orgs/1990/) 
* [1993 Files](https://data.nber.org/exempt-orgs/1993/) 
* [1994 Files](https://data.nber.org/exempt-orgs/1994/) 

In each directory is a .ZIP file containing all the other files - use that for downloading if you need everything. Please notify feenberg@nber.org of any missing files (except for 1991-2).

The sample forms come from http://nccs.urban.org, the website of the National Center for Charitable Statistics at the Urban Institute. Look in the IRS forms section in the database section. This site also has a great deal of additional related data.


-----


# Batch Download Files in R

```r
library( rvest )

dir.create( "historic-990s" )
setwd( "historic-990s" )

years <- c( 1982:1990, 1993, 1994 )

for( i in years )
{
  base.url <- "https://data.nber.org/exempt-orgs/"
  year.url <- paste0( base.url, i, "/" )
  page <- read_html( year.url )

  link.list <- page %>% html_nodes("a") %>% html_attr("href") 

  dir.create( as.character(i) )
  setwd( as.character(i) )  

  for( j in link.list )
  {
     FILENAME <- j
     URL <- paste0( year.url, FILENAME )
     download.file( URL, destfile=FILENAME, method="libcurl" )
  }
  
  setwd( ".." )

}
```
