# MPA 5830 - Module 01 (Fall 2021)

#### What will you learn?
In this module you will learn how to 

(a) access data files in R, both locally (i.e., from a folder on your computer) and from the web (i.e., without manually downloading a data file);

(b) explore the contents of a data file with specific comands, 

(c) add value labels and create a new qualitative variable, 

(d) save data files in native R format, 

(e) load RData files, and

(f) create, populate, and save a Jupyter Notebook.

Base R comes bundled with some built-in functions (pieces of computer code) that do a variety of things. But one often needs additional pieces of computer code to accomplish a task. Often, base R could dprobably have done the task but we might be able to complete the task faster\easier by relying on these additional pieces of computer code. You will see these additioal pieces of code being used time to time, and we call these little bundles of code __packages__.

We start by loading up a package -- [tidyverse](https://www.tidyverse.org/) -- "an opinionated collection of R packages designed for data science. All packages share an underlying design philosophy, grammar, and data structures." 

Well, what exactly is a __package__ in the R language? A package is best thought of as a collection of R functions, data, and documentation that explains what each function does. R has over 14,000 packages, each designed to do something very specific such as, for example, make maps, read large data files very speedily, make animated graphics, and so on. 

Each week you will see several new packages come into play because we will be relying on each to do something very specific for us. The __tidyverse__ will be a regular feature. 

How do we load a package? Well, first it has to be installed, but that has been taken care of for you. Once installed, we load it via the `library(packaganame)` command, as shown below.

<style type="text/css">

body{ /* Normal  */
/*      font-family: Lato, sans-serif;  
      font-family: Mukta, sans-serif; 
      font-family: 'Nunito Sans', sans-serif;
      font-family: Karla, sans-serif;  */
      font-family: 'Merriweather Sans', sans-serif; 
      font-size: 18px;
  }

h1.title {
  font-size: 38px;
  color: DarkRed;
}
h1 { /* Header 1 */
  font-size: 28px;
  color: DarkBlue;
}
h2 { /* Header 2 */
    font-size: 22px;
  color: DarkBlue;
}
h3 { /* Header 3 */
  font-size: 18px;
  color: DarkBlue;
}
code.r{ /* Code block */
    font-family: Mukta, sans-serif; 
    font-weight: 600;
    font-size: 16px;
}
/* pre { /* Code block - determines code spacing between lines */
    font-size: 16px;
} */

</style>


```R
library(tidyverse)
```

    ── [1mAttaching packages[22m ─────────────────────────────────────── tidyverse 1.3.1 ──
    
    [32m✔[39m [34mggplot2[39m 3.3.5     [32m✔[39m [34mpurrr  [39m 0.3.4
    [32m✔[39m [34mtibble [39m 3.1.5     [32m✔[39m [34mdplyr  [39m 1.0.7
    [32m✔[39m [34mtidyr  [39m 1.1.4     [32m✔[39m [34mstringr[39m 1.4.0
    [32m✔[39m [34mreadr  [39m 2.0.2     [32m✔[39m [34mforcats[39m 0.5.1
    
    ── [1mConflicts[22m ────────────────────────────────────────── tidyverse_conflicts() ──
    [31m✖[39m [34mdplyr[39m::[32mfilter()[39m masks [34mstats[39m::filter()
    [31m✖[39m [34mdplyr[39m::[32mlag()[39m    masks [34mstats[39m::lag()
    


The message you see with a pinkish background results from our `library(tidyverse)` command, and is just a message indicating what packages were loaded (`ggplot2, tibble, tidyr, readr, purr, dplyr, stringr, forcats`), and if there is anything else we should be aware of. In this case, there is -- some commands (`filter()` and `lag()`) used by the `dplyr` package conflict with identically named commands in the `stats` package. 

`readr` is an excellent package designed to read data that might be available in various formats. Since it is loaded as a part of the `tidyverse`, let us move on to seeing how to read data.

## Reading data
One of the golden rules to follow when reading data is to make your life easy by not having to memorize or discover through frantic trial-and-error runs where a particular data file is located. So how do we make our life easy? By placing all __data__ in a data folder. 


Data come in several formats but I will walk you through the formats you are most likely to encounter -- MS Excel, CSV, TXT, fixed-width, and then in any one of these commercial software formats: SAS, Stata, or SPSS. 

### CSV data files 
We start by reading a simple `comma-separated variable` format file and then a `tab-delimited variable` format file. A CSV file has each column separated by a `comma` while a tab-delimited file has every column separated by a `tab` -- `,` versus `\t` 


```R
read.csv(
    "data/ImportDataCSV.csv",
    sep = ",",
    header = TRUE
    ) -> df.csv 
```


```R
read.csv(
  "data/ImportDataTAB.txt",
  sep = "\t",
  header = TRUE
  ) -> df.tab
```

Note ... 
* "data/ImportDataCSV.csv" specifies the specific file that should be read from the data folder.  
* The `sep = ","` switch says the individual variables are separated by a comma 
* `header = TRUE` switch indicates that the first row in the file that is being read in has the column names  
* The tab-delimited file needs `sep = "\t"` because the columns are separated by a tab 

If no errors are thrown, then the files should have been read into memory and we can check their contents. The `names(filename)` command will show you the column names, and `glimpse(filename)` will show you a snippet of the data in the file.


```R
names(df.csv) 

glimpse(df.csv)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'x'</li><li>'y'</li><li>'z'</li></ol>



    Rows: 7
    Columns: 3
    $ x [3m[90m<int>[39m[23m 1, 4, 7, 10, 13, 16, 19
    $ y [3m[90m<int>[39m[23m 2, 5, 8, 11, 14, 17, 20
    $ z [3m[90m<int>[39m[23m 3, 6, 9, 12, 15, 18, 21



```R
# And now we repeat the preceding commands for the df.tab file

names(df.tab)

glimpse(df.tab)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'x'</li><li>'y'</li><li>'z'</li></ol>



    Rows: 7
    Columns: 3
    $ x [3m[90m<int>[39m[23m 1, 4, 7, 10, 13, 16, 19
    $ y [3m[90m<int>[39m[23m 2, 5, 8, 11, 14, 17, 20
    $ z [3m[90m<int>[39m[23m 3, 6, 9, 12, 15, 18, 21


## MS Excel files

Microsoft **Excel** files can be read via the `readxl` package, and you see two versions below -- one for the older `*.xls` format and the other for the newer `*.xlsx` format. 

As a rule, I would recommend against storing data in Excel formats since Excel tends to do some funny things. If you must store and share data, try to use the CSV format.    


```R
library(readxl)

read_excel(
  "data/ImportDataXLS.xls"
  ) -> df.xls 

read_excel(
  "data/ImportDataXLSX.xlsx"
  ) -> df.xlsx
```


```R
names(df.xls)

glimpse(df.xls)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'x'</li><li>'y'</li><li>'z'</li></ol>



    Rows: 7
    Columns: 3
    $ x [3m[90m<dbl>[39m[23m 1, 4, 7, 10, 13, 16, 19
    $ y [3m[90m<dbl>[39m[23m 2, 5, 8, 11, 14, 17, 20
    $ z [3m[90m<dbl>[39m[23m 3, 6, 9, 12, 15, 18, 21



```R
names(df.xlsx)

glimpse(df.xlsx)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'x'</li><li>'y'</li><li>'z'</li></ol>



    Rows: 7
    Columns: 3
    $ x [3m[90m<dbl>[39m[23m 1, 4, 7, 10, 13, 16, 19
    $ y [3m[90m<dbl>[39m[23m 2, 5, 8, 11, 14, 17, 20
    $ z [3m[90m<dbl>[39m[23m 3, 6, 9, 12, 15, 18, 21


## SPSS, Stata, SAS files
Some governmental agencies and other sources tend to use SAS, Stata, or SPSS for storing data and for carrying out various data analyses. This is a legacy issue that is changing but a little too slowly for most of us who do not use these commercial software packages as the mainstays of our data work. But, even if you do not use these packages, you should know how to read in data created in their native formats. As it turns out, **SPSS, Stata, SAS** files can be read via the `haven` package, and with relative ease. 


```R
library(haven)

read_stata(
  "data/ImportDataStata.dta"
  ) -> df.stata

read_sas(
  "data/ImportDataSAS.sas7bdat"
  ) -> df.sas

read_sav(
  "data/ImportDataSPSS.sav"
  ) -> df.spss
```


```R
# Check the files

names(df.stata)

glimpse(df.sas)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'x'</li><li>'y'</li><li>'z'</li></ol>



    Rows: 7
    Columns: 3
    $ x [3m[90m<dbl>[39m[23m 1, 4, 7, 10, 13, 16, 19
    $ y [3m[90m<dbl>[39m[23m 2, 5, 8, 11, 14, 17, 20
    $ z [3m[90m<dbl>[39m[23m 3, 6, 9, 12, 15, 18, 21


## Fixed-width files 
It is also common to encounter **fixed-width** files where the raw data are stored without any gaps between successive columns. This is also a legacy format from the early days of computers and punch cards, and one of the most efficient ways of storing large amounts of data. These files come with documentation that will give you the necessary details about where each column starts and ends, etc. [See here for some examples of layouts from the Census Bureau](https://www.census.gov/programs-surveys/geography/technical-documentation/records-layout/2010-zcta-record-layout.html#par_textimage_0).  


```R
read.fwf(
  "data/fwfdata.txt",
  widths = c(4, 9, 2, 4),
  header = FALSE,
  col.names = c("Name", "Month", "Day", "Year")
  ) -> df.fw 
```

Notice we need `widths = c(4,9,2,4)` to indicate how many slots each column takes and then `col.names = c("Name", "Month", "Day", "Year")` to label the columns since the data file does not have variable names. if you mess up with `widths = ` you end up with garbage because R does not know where any column starts or ends so be careful.   


```R
glimpse(df.fw)
```

    Rows: 2
    Columns: 4
    $ Name  [3m[90m<chr>[39m[23m "Amy ", "Abby"
    $ Month [3m[90m<chr>[39m[23m " December", " December"
    $ Day   [3m[90m<int>[39m[23m 1, 11
    $ Year  [3m[90m<int>[39m[23m 2017, 2017


# Reading Files from the Web
One of the benefits of software like R is its ability to read data files over the web __without requiring you to manually download the file and save a physical copy to be read in__. Specifically, it is possible to list the full web-path for a file and read it in. This ability is invaluable when the data tend to be periodically updated by the source (for example, by the Census Bureau, Bureau of Labor, Bureau of Economic Analysis, etc.). Here are a few examples.  


```R
read.table(
  "http://data.princeton.edu/wws509/datasets/effort.dat"
  ) -> fpe 

fpe # This command asks R to show us what fpe contains 
```


<table class="dataframe">
<caption>A data.frame: 20 × 3</caption>
<thead>
	<tr><th></th><th scope=col>setting</th><th scope=col>effort</th><th scope=col>change</th></tr>
	<tr><th></th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>Bolivia</th><td>46</td><td> 0</td><td> 1</td></tr>
	<tr><th scope=row>Brazil</th><td>74</td><td> 0</td><td>10</td></tr>
	<tr><th scope=row>Chile</th><td>89</td><td>16</td><td>29</td></tr>
	<tr><th scope=row>Colombia</th><td>77</td><td>16</td><td>25</td></tr>
	<tr><th scope=row>CostaRica</th><td>84</td><td>21</td><td>29</td></tr>
	<tr><th scope=row>Cuba</th><td>89</td><td>15</td><td>40</td></tr>
	<tr><th scope=row>DominicanRep</th><td>68</td><td>14</td><td>21</td></tr>
	<tr><th scope=row>Ecuador</th><td>70</td><td> 6</td><td> 0</td></tr>
	<tr><th scope=row>ElSalvador</th><td>60</td><td>13</td><td>13</td></tr>
	<tr><th scope=row>Guatemala</th><td>55</td><td> 9</td><td> 4</td></tr>
	<tr><th scope=row>Haiti</th><td>35</td><td> 3</td><td> 0</td></tr>
	<tr><th scope=row>Honduras</th><td>51</td><td> 7</td><td> 7</td></tr>
	<tr><th scope=row>Jamaica</th><td>87</td><td>23</td><td>21</td></tr>
	<tr><th scope=row>Mexico</th><td>83</td><td> 4</td><td> 9</td></tr>
	<tr><th scope=row>Nicaragua</th><td>68</td><td> 0</td><td> 7</td></tr>
	<tr><th scope=row>Panama</th><td>84</td><td>19</td><td>22</td></tr>
	<tr><th scope=row>Paraguay</th><td>74</td><td> 3</td><td> 6</td></tr>
	<tr><th scope=row>Peru</th><td>73</td><td> 0</td><td> 2</td></tr>
	<tr><th scope=row>TrinidadTobago</th><td>84</td><td>15</td><td>29</td></tr>
	<tr><th scope=row>Venezuela</th><td>91</td><td> 7</td><td>11</td></tr>
</tbody>
</table>




```R
# Some more files read from the web

read.table(
  "https://stats.idre.ucla.edu/stat/data/test.txt",
  header = TRUE
  ) -> test.txt 

read_csv(
  "https://stats.idre.ucla.edu/stat/data/test.csv"
  ) -> test.csv

read_sav(
  "https://stats.idre.ucla.edu/stat/data/hsb2.sav"
  ) -> hsb2.spss 
```

    [1m[1mRows: [1m[22m[34m[34m8[34m[39m [1m[1mColumns: [1m[22m[34m[34m6[34m[39m
    
    [36m──[39m [1m[1mColumn specification[1m[22m [36m────────────────────────────────────────────────────────[39m
    [1mDelimiter:[22m ","
    [31mchr[39m (1): prgtype
    [32mdbl[39m (5): gender, id, ses, schtyp, level
    
    
    [36mℹ[39m Use [30m[47m[30m[47m`spec()`[47m[30m[49m[39m to retrieve the full column specification for this data.
    [36mℹ[39m Specify the column types or set [30m[47m[30m[47m`show_col_types = FALSE`[47m[30m[49m[39m to quiet this message.
    


If you look at `fpe` you will notice that there are three columns but the rows each have a unique name. These are `row names` that are remembered by R but will not show up with a column name.  

We can also ask R to both download and unzip a `*.zip` file from the web. Here I am pulling down a file from a collection maintained and updated by the Centers for Disease Control and Prevention (CDC). The file has more than `4 million` rows of data and 14 columns of information.  


```R
temp = tempfile()

download.file("ftp://ftp.cdc.gov/pub/Health_Statistics/NCHS/Datasets/NVSS/bridgepop/2016/pcen_v2016_y1016.sas7bdat.zip", temp)

oursasdata = haven::read_sas(unz(temp, "pcen_v2016_y1016.sas7bdat"))

unlink(temp)
```

Now we will see three new commands -- `dim(filename)`, `head(filename)`, and `tail(filename)`. 

> `dim(filename)` shows the number of rows and number of columns in the dataset -- i.e., the dimensions of the dataset.

> `head(filename)` show the first 6 rows and all columns while `tail(filename)` shows the last 6 rows and all columns 



```R
dim(oursasdata)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>4323392</li><li>14</li></ol>




```R
head(oursasdata)
```


<table class="dataframe">
<caption>A tibble: 6 × 14</caption>
<thead>
	<tr><th scope=col>age</th><th scope=col>hisp</th><th scope=col>RACESEX</th><th scope=col>VINTAGE</th><th scope=col>POP2010_apr</th><th scope=col>POP2010_jul</th><th scope=col>POP2011</th><th scope=col>POP2012</th><th scope=col>POP2013</th><th scope=col>POP2014</th><th scope=col>POP2015</th><th scope=col>POP2016</th><th scope=col>ST_FIPS</th><th scope=col>CO_FIPS</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0</td><td>1</td><td>1</td><td>2016</td><td>236</td><td>242</td><td>237</td><td>229</td><td>224</td><td>240</td><td>236</td><td>225</td><td>1</td><td>1</td></tr>
	<tr><td>1</td><td>1</td><td>1</td><td>2016</td><td>299</td><td>284</td><td>241</td><td>254</td><td>231</td><td>249</td><td>238</td><td>230</td><td>1</td><td>1</td></tr>
	<tr><td>2</td><td>1</td><td>1</td><td>2016</td><td>287</td><td>292</td><td>294</td><td>233</td><td>240</td><td>237</td><td>234</td><td>255</td><td>1</td><td>1</td></tr>
	<tr><td>3</td><td>1</td><td>1</td><td>2016</td><td>286</td><td>286</td><td>310</td><td>283</td><td>241</td><td>244</td><td>229</td><td>240</td><td>1</td><td>1</td></tr>
	<tr><td>4</td><td>1</td><td>1</td><td>2016</td><td>270</td><td>273</td><td>280</td><td>319</td><td>278</td><td>247</td><td>252</td><td>239</td><td>1</td><td>1</td></tr>
	<tr><td>5</td><td>1</td><td>1</td><td>2016</td><td>279</td><td>277</td><td>285</td><td>279</td><td>305</td><td>287</td><td>265</td><td>254</td><td>1</td><td>1</td></tr>
</tbody>
</table>




```R
tail(oursasdata)
```


<table class="dataframe">
<caption>A tibble: 6 × 14</caption>
<thead>
	<tr><th scope=col>age</th><th scope=col>hisp</th><th scope=col>RACESEX</th><th scope=col>VINTAGE</th><th scope=col>POP2010_apr</th><th scope=col>POP2010_jul</th><th scope=col>POP2011</th><th scope=col>POP2012</th><th scope=col>POP2013</th><th scope=col>POP2014</th><th scope=col>POP2015</th><th scope=col>POP2016</th><th scope=col>ST_FIPS</th><th scope=col>CO_FIPS</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>80</td><td>2</td><td>8</td><td>2016</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>56</td><td>45</td></tr>
	<tr><td>81</td><td>2</td><td>8</td><td>2016</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>56</td><td>45</td></tr>
	<tr><td>82</td><td>2</td><td>8</td><td>2016</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>56</td><td>45</td></tr>
	<tr><td>83</td><td>2</td><td>8</td><td>2016</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>56</td><td>45</td></tr>
	<tr><td>84</td><td>2</td><td>8</td><td>2016</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>56</td><td>45</td></tr>
	<tr><td>85</td><td>2</td><td>8</td><td>2016</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>56</td><td>45</td></tr>
</tbody>
</table>



## Labeling data values

Often the data contain columns that record information with numeric codes that reflect some qualitative attributes. For example, whether an individual is Male or Female may be stored as 0 and 1, respectively. We can append the correct qualitative labels to these columns, and then hen we create graphs or tables, what is being displayed will be readily apparent (i.e., the reader\viewer does not have to work hard to figure out what is a `0` and what is a `1`).

We will read in a small dataset that has information on 200 students. The data come from the [High School and Beyond study](https://nces.ed.gov/surveys/hsb/)


| Column Name | Values and Labels\Meanings |
| :-- | :-- |
| female | (0/1) |
| race | (1=hispanic 2=asian 3=african-amer 4=white) |
| ses | socioeconomic status (1=low 2=middle 3=high) |
| schtyp | type of school (1=public 2=private) |
| prog | type of program (1=general 2=academic 3=vocational) |
| read | standardized reading score |
| write | standardized writing score |
| math | standardized math score |
| science | standardized science score |
| socst | standardized social studies score |

# Saving R data files
You can save your data in a format that R will recognize, giving it the **RData** or **rdata** extension 

Check the **data** directory to confirm that both files are present 

# Minimal example of data processing
Working with the **hsb2** data: 200 students from the [High School and Beyond study](https://nces.ed.gov/surveys/hsb/). The variables in this file are:  

- female  = (0/1) 
- race = (1=hispanic 2=asian 3=african-amer 4=white) 
- ses  = socioeconomic status (1=low 2=middle 3=high) 
- schtyp =  type of school (1=public 2=private) 
- prog   = type of program (1=general 2=academic 3=vocational) 
- read  =  standardized reading score 
- write  = standardized writing score 
- math   = standardized math score 
- science = standardized science score 
- socst = standardized social studies score 

Let us start by reading in these data.


```R
read.table(
  'https://stats.idre.ucla.edu/stat/data/hsb2.csv',
  header = TRUE,
  sep = ","
  ) -> hsb2
```

We can easily check the basic descriptive statistics of each column in a data file by running the `summary(filename)` command. Let us see what results if we do this with the `hsb2` data set.


```R
summary(hsb2)
```


           id             female           race           ses            schtyp    
     Min.   :  1.00   Min.   :0.000   Min.   :1.00   Min.   :1.000   Min.   :1.00  
     1st Qu.: 50.75   1st Qu.:0.000   1st Qu.:3.00   1st Qu.:2.000   1st Qu.:1.00  
     Median :100.50   Median :1.000   Median :4.00   Median :2.000   Median :1.00  
     Mean   :100.50   Mean   :0.545   Mean   :3.43   Mean   :2.055   Mean   :1.16  
     3rd Qu.:150.25   3rd Qu.:1.000   3rd Qu.:4.00   3rd Qu.:3.000   3rd Qu.:1.00  
     Max.   :200.00   Max.   :1.000   Max.   :4.00   Max.   :3.000   Max.   :2.00  
          prog            read           write            math      
     Min.   :1.000   Min.   :28.00   Min.   :31.00   Min.   :33.00  
     1st Qu.:2.000   1st Qu.:44.00   1st Qu.:45.75   1st Qu.:45.00  
     Median :2.000   Median :50.00   Median :54.00   Median :52.00  
     Mean   :2.025   Mean   :52.23   Mean   :52.77   Mean   :52.65  
     3rd Qu.:2.250   3rd Qu.:60.00   3rd Qu.:60.00   3rd Qu.:59.00  
     Max.   :3.000   Max.   :76.00   Max.   :67.00   Max.   :75.00  
        science          socst      
     Min.   :26.00   Min.   :26.00  
     1st Qu.:44.00   1st Qu.:46.00  
     Median :53.00   Median :52.00  
     Mean   :51.85   Mean   :52.41  
     3rd Qu.:58.00   3rd Qu.:61.00  
     Max.   :74.00   Max.   :71.00  



```R
summary(hsb2)
```


           id             female           race           ses            schtyp    
     Min.   :  1.00   Min.   :0.000   Min.   :1.00   Min.   :1.000   Min.   :1.00  
     1st Qu.: 50.75   1st Qu.:0.000   1st Qu.:3.00   1st Qu.:2.000   1st Qu.:1.00  
     Median :100.50   Median :1.000   Median :4.00   Median :2.000   Median :1.00  
     Mean   :100.50   Mean   :0.545   Mean   :3.43   Mean   :2.055   Mean   :1.16  
     3rd Qu.:150.25   3rd Qu.:1.000   3rd Qu.:4.00   3rd Qu.:3.000   3rd Qu.:1.00  
     Max.   :200.00   Max.   :1.000   Max.   :4.00   Max.   :3.000   Max.   :2.00  
          prog            read           write            math      
     Min.   :1.000   Min.   :28.00   Min.   :31.00   Min.   :33.00  
     1st Qu.:2.000   1st Qu.:44.00   1st Qu.:45.75   1st Qu.:45.00  
     Median :2.000   Median :50.00   Median :54.00   Median :52.00  
     Mean   :2.025   Mean   :52.23   Mean   :52.77   Mean   :52.65  
     3rd Qu.:2.250   3rd Qu.:60.00   3rd Qu.:60.00   3rd Qu.:59.00  
     Max.   :3.000   Max.   :76.00   Max.   :67.00   Max.   :75.00  
        science          socst      
     Min.   :26.00   Min.   :26.00  
     1st Qu.:44.00   1st Qu.:46.00  
     Median :53.00   Median :52.00  
     Mean   :51.85   Mean   :52.41  
     3rd Qu.:58.00   3rd Qu.:61.00  
     Max.   :74.00   Max.   :71.00  


Look at the output! R mistakenly treats several of the qualitative variables as if they are numeric, rendering incorrect information. Let us fix this problem and then we can rerun the `summary()` command to see if things have improved.

The way we append value labels is by converting a numeric variable\column to what R calls a `factor`. When we do so, we tell R to assign a particular label _(the word or the phrase)_ to a particular level _(the number)_ as shown below. 

> It is good habit to create a new variable each time you do some conversion so that the original variable does not get overwritten. If you do not get into the habit and end up overwriting the original variable, you will have to start from scratch if you end up making a mistake. 


```R
factor(hsb2$female,
       levels = c(0, 1),
       labels = c("Male", "Female")
       ) -> hsb2$female.f 

factor(hsb2$race,
       levels = c(1:4),
       labels = c("Hispanic", "Asian", "African American", "White")
       ) -> hsb2$race.f

factor(hsb2$ses,
       levels = c(1:3),
       labels = c("Low", "Middle", "High")
       ) -> hsb2$ses.f

factor(hsb2$schtyp,
       levels = c(1:2),
       labels = c("Public", "Private")
       ) -> hsb2$schtyp.f

factor(hsb2$prog,
       levels = c(1:3),
       labels = c("General", "Academic", "Vocational")
       ) -> hsb2$prog.f
```

Now we check our work by re-running the `summary(hsb2)` command.


```R
summary(hsb2)
```


           id             female           race           ses            schtyp    
     Min.   :  1.00   Min.   :0.000   Min.   :1.00   Min.   :1.000   Min.   :1.00  
     1st Qu.: 50.75   1st Qu.:0.000   1st Qu.:3.00   1st Qu.:2.000   1st Qu.:1.00  
     Median :100.50   Median :1.000   Median :4.00   Median :2.000   Median :1.00  
     Mean   :100.50   Mean   :0.545   Mean   :3.43   Mean   :2.055   Mean   :1.16  
     3rd Qu.:150.25   3rd Qu.:1.000   3rd Qu.:4.00   3rd Qu.:3.000   3rd Qu.:1.00  
     Max.   :200.00   Max.   :1.000   Max.   :4.00   Max.   :3.000   Max.   :2.00  
          prog            read           write            math      
     Min.   :1.000   Min.   :28.00   Min.   :31.00   Min.   :33.00  
     1st Qu.:2.000   1st Qu.:44.00   1st Qu.:45.75   1st Qu.:45.00  
     Median :2.000   Median :50.00   Median :54.00   Median :52.00  
     Mean   :2.025   Mean   :52.23   Mean   :52.77   Mean   :52.65  
     3rd Qu.:2.250   3rd Qu.:60.00   3rd Qu.:60.00   3rd Qu.:59.00  
     Max.   :3.000   Max.   :76.00   Max.   :67.00   Max.   :75.00  
        science          socst         female.f                race.f   
     Min.   :26.00   Min.   :26.00   Male  : 91   Hispanic        : 24  
     1st Qu.:44.00   1st Qu.:46.00   Female:109   Asian           : 11  
     Median :53.00   Median :52.00                African American: 20  
     Mean   :51.85   Mean   :52.41                White           :145  
     3rd Qu.:58.00   3rd Qu.:61.00                                      
     Max.   :74.00   Max.   :71.00                                      
        ses.f       schtyp.f          prog.f   
     Low   :47   Public :168   General   : 45  
     Middle:95   Private: 32   Academic  :105  
     High  :58                 Vocational: 50  
                                               
                                               
                                               


Aha! Everything is as it should be. 

Before we move on though, let us save our hsb2 data file that has new columns. We will save it in the native `R` format. The command to save a file is very simple -- `save(filename, file = "filepath/filename.RData")`

> You could also do `save(filename, file = "filepath/filename.rdata")` if that is what you intuitively prefer.


```R
save(hsb2, file = "data/hsb2.RData")
```

## Loading RData files

When we want to work with RData files, we have to load them with the `load("filepath/filename.RData")` command. Here I am doing it for the hsb2 data.


```R
load("data/hsb2.RData")
```


```R
summary(hsb2)
```


           id             female           race           ses            schtyp    
     Min.   :  1.00   Min.   :0.000   Min.   :1.00   Min.   :1.000   Min.   :1.00  
     1st Qu.: 50.75   1st Qu.:0.000   1st Qu.:3.00   1st Qu.:2.000   1st Qu.:1.00  
     Median :100.50   Median :1.000   Median :4.00   Median :2.000   Median :1.00  
     Mean   :100.50   Mean   :0.545   Mean   :3.43   Mean   :2.055   Mean   :1.16  
     3rd Qu.:150.25   3rd Qu.:1.000   3rd Qu.:4.00   3rd Qu.:3.000   3rd Qu.:1.00  
     Max.   :200.00   Max.   :1.000   Max.   :4.00   Max.   :3.000   Max.   :2.00  
          prog            read           write            math      
     Min.   :1.000   Min.   :28.00   Min.   :31.00   Min.   :33.00  
     1st Qu.:2.000   1st Qu.:44.00   1st Qu.:45.75   1st Qu.:45.00  
     Median :2.000   Median :50.00   Median :54.00   Median :52.00  
     Mean   :2.025   Mean   :52.23   Mean   :52.77   Mean   :52.65  
     3rd Qu.:2.250   3rd Qu.:60.00   3rd Qu.:60.00   3rd Qu.:59.00  
     Max.   :3.000   Max.   :76.00   Max.   :67.00   Max.   :75.00  
        science          socst         female.f                race.f   
     Min.   :26.00   Min.   :26.00   Male  : 91   Hispanic        : 24  
     1st Qu.:44.00   1st Qu.:46.00   Female:109   Asian           : 11  
     Median :53.00   Median :52.00                African American: 20  
     Mean   :51.85   Mean   :52.41                White           :145  
     3rd Qu.:58.00   3rd Qu.:61.00                                      
     Max.   :74.00   Max.   :71.00                                      
        ses.f       schtyp.f          prog.f   
     Low   :47   Public :168   General   : 45  
     Middle:95   Private: 32   Academic  :105  
     High  :58                 Vocational: 50  
                                               
                                               
                                               


## Data in packages 
Almost all R packages come bundled with data-sets, too many of them to walk you through but

- [see here for standard ones](https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/00Index.html) 
- [here are some more](https://vincentarelbundock.github.io/Rdatasets/datasets.html) 
- [and some more](http://www.public.iastate.edu/~hofmann/data_in_r_sortable.html) 

To load data from a package, if you know the data-set's name, it is easy to load it, as shown below. Some times you may need to install the package. We do this with the `install.packages("packagename")` command. 


```R
install.packages('palmerpenguins')

library(palmerpenguins)

data(penguins, package = 'palmerpenguins')

head(penguins)
```

    Installing package into ‘/Library/Frameworks/R.framework/Versions/4.1/Resources/site-library’
    (as ‘lib’ is unspecified)
    


    
    The downloaded binary packages are in
    	/tmp/Rtmphl205F/downloaded_packages



<table class="dataframe">
<caption>A tibble: 6 × 8</caption>
<thead>
	<tr><th scope=col>species</th><th scope=col>island</th><th scope=col>bill_length_mm</th><th scope=col>bill_depth_mm</th><th scope=col>flipper_length_mm</th><th scope=col>body_mass_g</th><th scope=col>sex</th><th scope=col>year</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Adelie</td><td>Torgersen</td><td>39.1</td><td>18.7</td><td>181</td><td>3750</td><td>male  </td><td>2007</td></tr>
	<tr><td>Adelie</td><td>Torgersen</td><td>39.5</td><td>17.4</td><td>186</td><td>3800</td><td>female</td><td>2007</td></tr>
	<tr><td>Adelie</td><td>Torgersen</td><td>40.3</td><td>18.0</td><td>195</td><td>3250</td><td>female</td><td>2007</td></tr>
	<tr><td>Adelie</td><td>Torgersen</td><td>  NA</td><td>  NA</td><td> NA</td><td>  NA</td><td>NA    </td><td>2007</td></tr>
	<tr><td>Adelie</td><td>Torgersen</td><td>36.7</td><td>19.3</td><td>193</td><td>3450</td><td>female</td><td>2007</td></tr>
	<tr><td>Adelie</td><td>Torgersen</td><td>39.3</td><td>20.6</td><td>190</td><td>3650</td><td>male  </td><td>2007</td></tr>
</tbody>
</table>




```R
library(ggplot2)

data(diamonds, package = 'ggplot2')

head(diamonds)
```


<table class="dataframe">
<caption>A tibble: 6 × 10</caption>
<thead>
	<tr><th scope=col>carat</th><th scope=col>cut</th><th scope=col>color</th><th scope=col>clarity</th><th scope=col>depth</th><th scope=col>table</th><th scope=col>price</th><th scope=col>x</th><th scope=col>y</th><th scope=col>z</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;ord&gt;</th><th scope=col>&lt;ord&gt;</th><th scope=col>&lt;ord&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0.23</td><td>Ideal    </td><td>E</td><td>SI2 </td><td>61.5</td><td>55</td><td>326</td><td>3.95</td><td>3.98</td><td>2.43</td></tr>
	<tr><td>0.21</td><td>Premium  </td><td>E</td><td>SI1 </td><td>59.8</td><td>61</td><td>326</td><td>3.89</td><td>3.84</td><td>2.31</td></tr>
	<tr><td>0.23</td><td>Good     </td><td>E</td><td>VS1 </td><td>56.9</td><td>65</td><td>327</td><td>4.05</td><td>4.07</td><td>2.31</td></tr>
	<tr><td>0.29</td><td>Premium  </td><td>I</td><td>VS2 </td><td>62.4</td><td>58</td><td>334</td><td>4.20</td><td>4.23</td><td>2.63</td></tr>
	<tr><td>0.31</td><td>Good     </td><td>J</td><td>SI2 </td><td>63.3</td><td>58</td><td>335</td><td>4.34</td><td>4.35</td><td>2.75</td></tr>
	<tr><td>0.24</td><td>Very Good</td><td>J</td><td>VVS2</td><td>62.8</td><td>57</td><td>336</td><td>3.94</td><td>3.96</td><td>2.48</td></tr>
</tbody>
</table>



---------------
## Exercises for practice 
These are some exercises you can use to practice and build your R skills. They are not for grade. 

#### Exercise 01: Reading in some data files 

1. Create a new Notebook by going to File -> New -> Notebook.

2. When prompted to select a kernel, use the dropdown menu to select the _R_ kernel. 

3. The notebook will be untitled, so go ahead and save it with a name, something like `yourlastname_ex01` and you will see `yourlastname_ex01.ipynb` as the name. 

4. Now read in the `Stata` data file found [here](http://www.stata.com/data/jwooldridge/eacsap/mroz.dta) 

5. Create a new cell and run the `summary` command to check the contents of this data file and look at the first 6 rows of data with the appropriate `head` command.and look at the first 6 rows of data with the appropriate `head` command.

6. In a new cell, read in tthe `SPSS` file found [here](http://calcnet.mth.cmich.edu/org/spss/V16_materials/DataSets_v16/airline_passengers.sav) 

7. In a new cell, run the `summary` command and look at the first 6 rows of data with the appropriate `head` command.

#### Exercise 02: Reading in local data and labeling some values

1. Download [this dataset](https://s3.amazonaws.com/tripdata/201502-citibike-tripdata.zip), extract the file inside the zip archive and upload it to the `data` folder. 

2. In a new cell, read in this uploaded data file with the appropriate commands. 

3. The variable `gender` has the following codes: `Zero = unknown; 1 = male; 2 = female`.  Use this coding scheme to create a new column that shows `gender` as a `factor` with these value labels 

4. Check the first 6 rows of the dataset and also run `summary` to check the new column was created as desired. 

5. In a new cell, write the commands necessary to save each of the three data-sets as separate `RData` files. 

#### Exercise 03: Welcome to Kaggle & Mass Shootings 

Go to [this page on Kaggle](https://www.kaggle.com/zusmani/us-mass-shootings-last-50-years) and read the description of the data-set on mass shootings in the United States that occurred during the 1966-2017 period. once you have read the overview of the data, click the "Data" tab and download the file called `Mass Shootings Dataset.csv`. Be careful; there are several versions so the one you want is the very last one and not any that have a version number attached, such as "Mass Shootings Dataset Ver 2.csv" for example. 

Now read this file, perhaps naming it `shootings` and run the `summary()` command on it. Note the number of observations and the number of variables in the data-set. 

Make sure you save the file in RData format as well. 

#### Exercise 04: Animal Shelters 

Go to [this page on Kaggle](https://www.kaggle.com/aaronschlegel/austin-animal-center-shelter-outcomes-and) and download the file called `aac_shelter_outcomes.zip`, unzip it, and AFTER reading the data overview, read in the file and generate the usual `summary` and also save it as an RData file.
