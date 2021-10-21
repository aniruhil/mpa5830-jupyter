# MPA 5830 - Module 02 (Fall 2021)
In this module our goal is to understand how some very powerful packages work their magic. In particular, although they are of somewhat recent vintage, `{dplyr}`, `{tidyr}` and `{lubridate}` have quickly gained a fan following because they allow you to clean and organize your data and then calculate quantities of interest with surprising ease. In this module we will start to see some of these packages' core functionalities and wrap up this particular leg of our learning journey in __`Module 03`__. 

## `{dplyr}` 
There is a common quote that is tossed about a good bit in the hallways of data science, that, and I am paraphrasing here, a data scientists spends about 80% of their time gathering, cleaning, and organizing their data and spends only about 20% of their time on the analysis per se. This may or may not be true, especially in the initial stages of a new project but yes, we do spend an awfully large amount of our time getting the data ready for visualizations and other analyses. You do this work long enough and you come to realize that anything you could do to speed up the cleaning phase would be time and money saved. And yet, data cleaning skills are in short supply. On the plus side of the ledger, packages like `{dplyr}` and `{data.table}` have simplified what were once nightmarish tasks. 

`Nightmare` is not a word to be tossed around lightly and so let us setup a seemingly large data-set with 100+ columns, and tons of information hidden in it. Once we setup this scenario, spell out a few questions we would like to answer, we might better appreciate how `{dplyr}` comes to our aid. In particular, we might come to understand how `{dplyr}` uses seven core verbs:  

| **What you want to do ...** | **`{dplyr}` function** |
| :-----  | :----- |
| you need to select columns to work with? | `select()`|
| you need to use a subset of the data based on some criterion? | `filter()`|
| you need to arrange the data in ascending/descending order of variable(s)? | `arrange()`|
| you want the results of your calculations to be a standalone data frame? | `summarise()`|
| you want to add your calculated value(s) to the existing data frame? | `mutate()`|
| you want to add your calculated value(s) to the existing data frame but also  drop all  variables/columns not  being used in the calculation? | `transmute()`|
| you need to calculate averages, frequencies, etc by groups? | `group_by()`|

Here is the dataset -- all flights originating and departing from Columbus (Ohio) January through September of 2017. 

Let  us load the data, and the `{tidyverse}` and `{here}` packages. 


```R
library(tidyverse) 

library(here)

load(
  here("data", "cmhflights_01092017.RData")
  )
```

    ── [1mAttaching packages[22m ─────────────────────────────────────── tidyverse 1.3.1 ──
    
    [32m✔[39m [34mggplot2[39m 3.3.5     [32m✔[39m [34mpurrr  [39m 0.3.4
    [32m✔[39m [34mtibble [39m 3.1.5     [32m✔[39m [34mdplyr  [39m 1.0.7
    [32m✔[39m [34mtidyr  [39m 1.1.4     [32m✔[39m [34mstringr[39m 1.4.0
    [32m✔[39m [34mreadr  [39m 2.0.2     [32m✔[39m [34mforcats[39m 0.5.1
    
    ── [1mConflicts[22m ────────────────────────────────────────── tidyverse_conflicts() ──
    [31m✖[39m [34mdplyr[39m::[32mfilter()[39m masks [34mstats[39m::filter()
    [31m✖[39m [34mdplyr[39m::[32mlag()[39m    masks [34mstats[39m::lag()
    
    here() starts at /Users/ruhil/Documents/github/mpa5830-fa21
    



```R
names(cmhflights) # will show you the column names
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Year'</li><li>'Quarter'</li><li>'Month'</li><li>'DayofMonth'</li><li>'DayOfWeek'</li><li>'FlightDate'</li><li>'UniqueCarrier'</li><li>'AirlineID'</li><li>'Carrier'</li><li>'TailNum'</li><li>'FlightNum'</li><li>'OriginAirportID'</li><li>'OriginAirportSeqID'</li><li>'OriginCityMarketID'</li><li>'Origin'</li><li>'OriginCityName'</li><li>'OriginState'</li><li>'OriginStateFips'</li><li>'OriginStateName'</li><li>'OriginWac'</li><li>'DestAirportID'</li><li>'DestAirportSeqID'</li><li>'DestCityMarketID'</li><li>'Dest'</li><li>'DestCityName'</li><li>'DestState'</li><li>'DestStateFips'</li><li>'DestStateName'</li><li>'DestWac'</li><li>'CRSDepTime'</li><li>'DepTime'</li><li>'DepDelay'</li><li>'DepDelayMinutes'</li><li>'DepDel15'</li><li>'DepartureDelayGroups'</li><li>'DepTimeBlk'</li><li>'TaxiOut'</li><li>'WheelsOff'</li><li>'WheelsOn'</li><li>'TaxiIn'</li><li>'CRSArrTime'</li><li>'ArrTime'</li><li>'ArrDelay'</li><li>'ArrDelayMinutes'</li><li>'ArrDel15'</li><li>'ArrivalDelayGroups'</li><li>'ArrTimeBlk'</li><li>'Cancelled'</li><li>'CancellationCode'</li><li>'Diverted'</li><li>'CRSElapsedTime'</li><li>'ActualElapsedTime'</li><li>'AirTime'</li><li>'Flights'</li><li>'Distance'</li><li>'DistanceGroup'</li><li>'CarrierDelay'</li><li>'WeatherDelay'</li><li>'NASDelay'</li><li>'SecurityDelay'</li><li>'LateAircraftDelay'</li><li>'FirstDepTime'</li><li>'TotalAddGTime'</li><li>'LongestAddGTime'</li><li>'DivAirportLandings'</li><li>'DivReachedDest'</li><li>'DivActualElapsedTime'</li><li>'DivArrDelay'</li><li>'DivDistance'</li><li>'Div1Airport'</li><li>'Div1AirportID'</li><li>'Div1AirportSeqID'</li><li>'Div1WheelsOn'</li><li>'Div1TotalGTime'</li><li>'Div1LongestGTime'</li><li>'Div1WheelsOff'</li><li>'Div1TailNum'</li><li>'Div2Airport'</li><li>'Div2AirportID'</li><li>'Div2AirportSeqID'</li><li>'Div2WheelsOn'</li><li>'Div2TotalGTime'</li><li>'Div2LongestGTime'</li><li>'Div2WheelsOff'</li><li>'Div2TailNum'</li><li>'Div3Airport'</li><li>'Div3AirportID'</li><li>'Div3AirportSeqID'</li><li>'Div3WheelsOn'</li><li>'Div3TotalGTime'</li><li>'Div3LongestGTime'</li><li>'Div3WheelsOff'</li><li>'Div3TailNum'</li><li>'Div4Airport'</li><li>'Div4AirportID'</li><li>'Div4AirportSeqID'</li><li>'Div4WheelsOn'</li><li>'Div4TotalGTime'</li><li>'Div4LongestGTime'</li><li>'Div4WheelsOff'</li><li>'Div4TailNum'</li><li>'Div5Airport'</li><li>'Div5AirportID'</li><li>'Div5AirportSeqID'</li><li>'Div5WheelsOn'</li><li>'Div5TotalGTime'</li><li>'Div5LongestGTime'</li><li>'Div5WheelsOff'</li><li>'Div5TailNum'</li><li>'X110'</li></ol>



The output is rather onerous with 110 columns displayed, the last being `X110`, an empty column that can be dropped. 


```R
cmhflights$X110 <- NULL #This command will delete the column named X110 
```

Did it work? Let us check.


```R
names(cmhflights)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Year'</li><li>'Quarter'</li><li>'Month'</li><li>'DayofMonth'</li><li>'DayOfWeek'</li><li>'FlightDate'</li><li>'UniqueCarrier'</li><li>'AirlineID'</li><li>'Carrier'</li><li>'TailNum'</li><li>'FlightNum'</li><li>'OriginAirportID'</li><li>'OriginAirportSeqID'</li><li>'OriginCityMarketID'</li><li>'Origin'</li><li>'OriginCityName'</li><li>'OriginState'</li><li>'OriginStateFips'</li><li>'OriginStateName'</li><li>'OriginWac'</li><li>'DestAirportID'</li><li>'DestAirportSeqID'</li><li>'DestCityMarketID'</li><li>'Dest'</li><li>'DestCityName'</li><li>'DestState'</li><li>'DestStateFips'</li><li>'DestStateName'</li><li>'DestWac'</li><li>'CRSDepTime'</li><li>'DepTime'</li><li>'DepDelay'</li><li>'DepDelayMinutes'</li><li>'DepDel15'</li><li>'DepartureDelayGroups'</li><li>'DepTimeBlk'</li><li>'TaxiOut'</li><li>'WheelsOff'</li><li>'WheelsOn'</li><li>'TaxiIn'</li><li>'CRSArrTime'</li><li>'ArrTime'</li><li>'ArrDelay'</li><li>'ArrDelayMinutes'</li><li>'ArrDel15'</li><li>'ArrivalDelayGroups'</li><li>'ArrTimeBlk'</li><li>'Cancelled'</li><li>'CancellationCode'</li><li>'Diverted'</li><li>'CRSElapsedTime'</li><li>'ActualElapsedTime'</li><li>'AirTime'</li><li>'Flights'</li><li>'Distance'</li><li>'DistanceGroup'</li><li>'CarrierDelay'</li><li>'WeatherDelay'</li><li>'NASDelay'</li><li>'SecurityDelay'</li><li>'LateAircraftDelay'</li><li>'FirstDepTime'</li><li>'TotalAddGTime'</li><li>'LongestAddGTime'</li><li>'DivAirportLandings'</li><li>'DivReachedDest'</li><li>'DivActualElapsedTime'</li><li>'DivArrDelay'</li><li>'DivDistance'</li><li>'Div1Airport'</li><li>'Div1AirportID'</li><li>'Div1AirportSeqID'</li><li>'Div1WheelsOn'</li><li>'Div1TotalGTime'</li><li>'Div1LongestGTime'</li><li>'Div1WheelsOff'</li><li>'Div1TailNum'</li><li>'Div2Airport'</li><li>'Div2AirportID'</li><li>'Div2AirportSeqID'</li><li>'Div2WheelsOn'</li><li>'Div2TotalGTime'</li><li>'Div2LongestGTime'</li><li>'Div2WheelsOff'</li><li>'Div2TailNum'</li><li>'Div3Airport'</li><li>'Div3AirportID'</li><li>'Div3AirportSeqID'</li><li>'Div3WheelsOn'</li><li>'Div3TotalGTime'</li><li>'Div3LongestGTime'</li><li>'Div3WheelsOff'</li><li>'Div3TailNum'</li><li>'Div4Airport'</li><li>'Div4AirportID'</li><li>'Div4AirportSeqID'</li><li>'Div4WheelsOn'</li><li>'Div4TotalGTime'</li><li>'Div4LongestGTime'</li><li>'Div4WheelsOff'</li><li>'Div4TailNum'</li><li>'Div5Airport'</li><li>'Div5AirportID'</li><li>'Div5AirportSeqID'</li><li>'Div5WheelsOn'</li><li>'Div5TotalGTime'</li><li>'Div5LongestGTime'</li><li>'Div5WheelsOff'</li><li>'Div5TailNum'</li></ol>



Now we are down to 109 columns and ready to get to work. 

## select() 
As in the present case of 100+ columns, often our data frame will have more columns than we plan to work with. In such instances it is a good idea to drop all unwanted columns; this will make the data-set more manageable and tax our computing resources less. For example, say I only want the first five columns (which happen to be `Year`, `Quarter`, `Month`, `DayofMonth`, and `DayOfWeek`). I could use `select` to create a data frame with only these columns:


```R
cmhflights %>% 
  select(Year:DayOfWeek) -> my.df
```

The `:` is the bridge between __consecutive columns__ starting with `Year` and stopping with `DayofWeek`.

In addition, note that the subset of columns selected and all rows of data are being written to a new data frame called `my.df`

Quick check to see if we have only the columns we wanted. 


```R
names(my.df)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Year'</li><li>'Quarter'</li><li>'Month'</li><li>'DayofMonth'</li><li>'DayOfWeek'</li></ol>



Wonderful! It worked.

Now, the same result could have been obtained by taking the longer route of __listing each column by name__, as in the following: 


```R
cmhflights %>%
  select(Year, Quarter, Month, DayofMonth, DayOfWeek) -> my.df 

names(my.df)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Year'</li><li>'Quarter'</li><li>'Month'</li><li>'DayofMonth'</li><li>'DayOfWeek'</li></ol>



What if the columns were not sequentially located? In that case we would need to list each column we want. Say I want `Year`, `FlightDate`, `UniqueCarrier`, and `TailNum`. 


```R
cmhflights %>% 
  select(Year, FlightDate:UniqueCarrier, TailNum) -> my.df 

names(my.df)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Year'</li><li>'FlightDate'</li><li>'UniqueCarrier'</li><li>'TailNum'</li></ol>



Could we have used __column numbers instead of column names__? Absolutely. 


```R
cmhflights %>% 
  select(c(1, 3, 5, 7)) -> my.df 

names(my.df)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Year'</li><li>'Month'</li><li>'DayOfWeek'</li><li>'UniqueCarrier'</li></ol>



You can also use __consecutive column numbers__, for examples, columns 1 through 5 as follows: 


```R
cmhflights %>% 
  select(c(1:5)) -> my.df 

names(my.df)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Year'</li><li>'Quarter'</li><li>'Month'</li><li>'DayofMonth'</li><li>'DayOfWeek'</li></ol>



You can also use column numbers to select a mix of columns, some that may be sequential and others that may be not sequential.


```R
cmhflights %>% 
  select(c(1, 6:9, 12)) -> my.df

names(my.df)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Year'</li><li>'FlightDate'</li><li>'UniqueCarrier'</li><li>'AirlineID'</li><li>'Carrier'</li><li>'OriginAirportID'</li></ol>



### select() in other ways
We can also select columns in other ways, by specifying that  the column name `contain` some element. The code below shows you how this is done if I am looking for column names with the phrase "Carrier", then with "De", and then with "Num"  


```R
cmhflights %>% 
  select(contains("Carrier")) -> my.df

names(my.df)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'UniqueCarrier'</li><li>'Carrier'</li><li>'CarrierDelay'</li></ol>



You can also specify that the columns to be selected __start with__ some alphanumeric string, for example, 


```R
cmhflights %>% 
  select(starts_with("De")) -> my.df

names(my.df)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'DestAirportID'</li><li>'DestAirportSeqID'</li><li>'DestCityMarketID'</li><li>'Dest'</li><li>'DestCityName'</li><li>'DestState'</li><li>'DestStateFips'</li><li>'DestStateName'</li><li>'DestWac'</li><li>'DepTime'</li><li>'DepDelay'</li><li>'DepDelayMinutes'</li><li>'DepDel15'</li><li>'DepartureDelayGroups'</li><li>'DepTimeBlk'</li></ol>



The other option would be to choose columns that __end with__ a particular alphanumeruc string, for example, 


```R
cmhflights %>% 
  select(ends_with("Num")) -> my.df

names(my.df)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'TailNum'</li><li>'FlightNum'</li><li>'Div1TailNum'</li><li>'Div2TailNum'</li><li>'Div3TailNum'</li><li>'Div4TailNum'</li><li>'Div5TailNum'</li></ol>



There are two other options -- `matches()` and `num_range()`. Let us look at each in turn. 


```R
cmhflights %>%
    select(matches("el")) -> my.df

names(my.df)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'DepDelay'</li><li>'DepDelayMinutes'</li><li>'DepDel15'</li><li>'DepartureDelayGroups'</li><li>'WheelsOff'</li><li>'WheelsOn'</li><li>'ArrDelay'</li><li>'ArrDelayMinutes'</li><li>'ArrDel15'</li><li>'ArrivalDelayGroups'</li><li>'Cancelled'</li><li>'CancellationCode'</li><li>'CRSElapsedTime'</li><li>'ActualElapsedTime'</li><li>'CarrierDelay'</li><li>'WeatherDelay'</li><li>'NASDelay'</li><li>'SecurityDelay'</li><li>'LateAircraftDelay'</li><li>'DivActualElapsedTime'</li><li>'DivArrDelay'</li><li>'Div1WheelsOn'</li><li>'Div1WheelsOff'</li><li>'Div2WheelsOn'</li><li>'Div2WheelsOff'</li><li>'Div3WheelsOn'</li><li>'Div3WheelsOff'</li><li>'Div4WheelsOn'</li><li>'Div4WheelsOff'</li><li>'Div5WheelsOn'</li><li>'Div5WheelsOff'</li></ol>



Here is another dataset where some of the column names contain a number.


```R
head(billboard)
```


<table class="dataframe">
<caption>A tibble: 6 × 79</caption>
<thead>
	<tr><th scope=col>artist</th><th scope=col>track</th><th scope=col>date.entered</th><th scope=col>wk1</th><th scope=col>wk2</th><th scope=col>wk3</th><th scope=col>wk4</th><th scope=col>wk5</th><th scope=col>wk6</th><th scope=col>wk7</th><th scope=col>⋯</th><th scope=col>wk67</th><th scope=col>wk68</th><th scope=col>wk69</th><th scope=col>wk70</th><th scope=col>wk71</th><th scope=col>wk72</th><th scope=col>wk73</th><th scope=col>wk74</th><th scope=col>wk75</th><th scope=col>wk76</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;date&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>⋯</th><th scope=col>&lt;lgl&gt;</th><th scope=col>&lt;lgl&gt;</th><th scope=col>&lt;lgl&gt;</th><th scope=col>&lt;lgl&gt;</th><th scope=col>&lt;lgl&gt;</th><th scope=col>&lt;lgl&gt;</th><th scope=col>&lt;lgl&gt;</th><th scope=col>&lt;lgl&gt;</th><th scope=col>&lt;lgl&gt;</th><th scope=col>&lt;lgl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2 Pac       </td><td>Baby Don't Cry (Keep...</td><td>2000-02-26</td><td>87</td><td>82</td><td>72</td><td>77</td><td>87</td><td>94</td><td>99</td><td>⋯</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2Ge+her     </td><td>The Hardest Part Of ...</td><td>2000-09-02</td><td>91</td><td>87</td><td>92</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>⋯</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>3 Doors Down</td><td>Kryptonite             </td><td>2000-04-08</td><td>81</td><td>70</td><td>68</td><td>67</td><td>66</td><td>57</td><td>54</td><td>⋯</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>3 Doors Down</td><td>Loser                  </td><td>2000-10-21</td><td>76</td><td>76</td><td>72</td><td>69</td><td>67</td><td>65</td><td>55</td><td>⋯</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>504 Boyz    </td><td>Wobble Wobble          </td><td>2000-04-15</td><td>57</td><td>34</td><td>25</td><td>17</td><td>17</td><td>31</td><td>36</td><td>⋯</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>98^0        </td><td>Give Me Just One Nig...</td><td>2000-08-19</td><td>51</td><td>39</td><td>34</td><td>26</td><td>26</td><td>19</td><td> 2</td><td>⋯</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td><td>NA</td></tr>
</tbody>
</table>




```R
billboard %>%
    select(num_range("wk", 1:5)) -> my.df

names(my.df)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'wk1'</li><li>'wk2'</li><li>'wk3'</li><li>'wk4'</li><li>'wk5'</li></ol>



------------------

## filter()
Do you really want all the rows in the data-set or do you want to see only very specific rows that meet some criteria? Say we only want to look at certain months, or only flights on Saturdays and Sundays, or flights in a given month. For example, say we only want flights in January, i.e., `Month == 1`. 


```R
cmhflights %>% 
  filter(Month == 1) -> my.df

table(my.df$Month) # Show me a frequency table for Month in my.df
```


    
       1 
    3757 


What about only American Airline flights in January? Note that the UniqueCarrier code for this airline is AA.


```R
cmhflights %>% 
  filter(Month == 1, UniqueCarrier == "AA") -> my.df 

table(my.df$Month, my.df$UniqueCarrier) # a simple frequency table
```


       
         AA
      1 387


Note that `,` inside `filter` means `&`

What about United Airlines flights in January to CMH (the airport code for Columbus, OH), regardless of where the flight originated? 


```R
cmhflights %>% 
  filter(Month == 1, UniqueCarrier == "UA", Dest == "CMH") -> my.df
```

What if I wanted a more complicated filter, say, flights in January or February, either to CMH or to ORD (the airport code for O'Hare in Chicago)?


```R
cmhflights %>% 
  filter(
    Month %in% c(1, 2), UniqueCarrier == "UA", Dest %in% c("CMH", "ORD")
    ) -> my.df 

table(my.df$Month) # frequency table of Month
```


    
      1   2 
    106 145 



```R
table(my.df$UniqueCarrier) # frequency table of UniqueCarrier
```


    
     UA 
    251 



```R
table(my.df$Dest) # frequency table of Dest
```


    
    CMH ORD 
    132 119 


Beautiful, just beautiful. 

At this point it may not be readily apparent to you but using `%in% c(...)` makes applying complex filters easier than if you go some other route. 

Before we move on, note the operators  that work with `filter()` 

| **Operator** | **Meaning**  | **Operator** | **Meaning** |
| :---- | :---- | :---- |  :---- |
| $<$    | less than | $>$    | greater than |
| $==$   | equal to  | $\leq$ | less than or equal to |
| $\geq$ | greater than or equal to | `!=` | not equal to |
| `%in%`   | is a member of |  `is.na` | is NA |
| `!is.na` | is not NA  | `&,!,etc` | Boolean operators |

----------------

## arrange()

Now let us say I wanted to arrange the resulting data frame in `ascending order` of departure delays. How might I do that? Via `arrange()` 

Before we see this new command in action I am going to whittle the data-frame to only a few columns, and only flights to CMH or ORD. That will make it easier to see the result of executed commands.


```R
cmhflights %>% 
    select('Year', 'Month', 'DayofMonth', 'FlightDate', 'Carrier', 'TailNum', 'FlightNum', 
           'Origin', 'OriginCityName', 'Dest', 'DestCityName', 'CRSDepTime', 'DepTime', 
           'DepDelay', 'DepDelayMinutes', 'CRSArrTime', 'ArrTime', 'ArrDelay', 'ArrDelayMinutes') %>%
    filter(Dest %in% c("CMH", "ORD")) -> my.df

my.df %>% 
  arrange(DepDelayMinutes) -> my.df2

my.df2
```


<table class="dataframe">
<caption>A tibble: 18799 × 19</caption>
<thead>
	<tr><th scope=col>Year</th><th scope=col>Month</th><th scope=col>DayofMonth</th><th scope=col>FlightDate</th><th scope=col>Carrier</th><th scope=col>TailNum</th><th scope=col>FlightNum</th><th scope=col>Origin</th><th scope=col>OriginCityName</th><th scope=col>Dest</th><th scope=col>DestCityName</th><th scope=col>CRSDepTime</th><th scope=col>DepTime</th><th scope=col>DepDelay</th><th scope=col>DepDelayMinutes</th><th scope=col>CRSArrTime</th><th scope=col>ArrTime</th><th scope=col>ArrDelay</th><th scope=col>ArrDelayMinutes</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;date&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2017</td><td>1</td><td> 1</td><td>2017-01-01</td><td>AA</td><td>N802AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0945</td><td>0936</td><td> -9</td><td>0</td><td>1707</td><td>1653</td><td>-14</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td> 3</td><td>2017-01-03</td><td>AA</td><td>N760AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0945</td><td>0939</td><td> -6</td><td>0</td><td>1707</td><td>1630</td><td>-37</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td> 4</td><td>2017-01-04</td><td>AA</td><td>N753AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0945</td><td>0940</td><td> -5</td><td>0</td><td>1707</td><td>1642</td><td>-25</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td> 5</td><td>2017-01-05</td><td>AA</td><td>N744AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0945</td><td>0938</td><td> -7</td><td>0</td><td>1707</td><td>1645</td><td>-22</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td> 7</td><td>2017-01-07</td><td>AA</td><td>N762AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0945</td><td>0941</td><td> -4</td><td>0</td><td>1707</td><td>1700</td><td> -7</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td> 8</td><td>2017-01-08</td><td>AA</td><td>N808AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0945</td><td>0941</td><td> -4</td><td>0</td><td>1707</td><td>1655</td><td>-12</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>11</td><td>2017-01-11</td><td>AA</td><td>N756AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0940</td><td>0934</td><td> -6</td><td>0</td><td>1712</td><td>1641</td><td>-31</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>12</td><td>2017-01-12</td><td>AA</td><td>N805AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0940</td><td>0935</td><td> -5</td><td>0</td><td>1712</td><td>1652</td><td>-20</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>13</td><td>2017-01-13</td><td>AA</td><td>N766AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0940</td><td>0937</td><td> -3</td><td>0</td><td>1712</td><td>1644</td><td>-28</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>18</td><td>2017-01-18</td><td>AA</td><td>N825AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0940</td><td>0940</td><td>  0</td><td>0</td><td>1712</td><td>1658</td><td>-14</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>25</td><td>2017-01-25</td><td>AA</td><td>N815AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0940</td><td>0932</td><td> -8</td><td>0</td><td>1712</td><td>1635</td><td>-37</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>26</td><td>2017-01-26</td><td>AA</td><td>N755AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0940</td><td>0935</td><td> -5</td><td>0</td><td>1712</td><td>1637</td><td>-35</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>27</td><td>2017-01-27</td><td>AA</td><td>N834AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0940</td><td>0937</td><td> -3</td><td>0</td><td>1712</td><td>1649</td><td>-23</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>30</td><td>2017-01-30</td><td>AA</td><td>N815AA</td><td>508</td><td>LAX</td><td>Los Angeles, CA</td><td>CMH</td><td>Columbus, OH</td><td>0940</td><td>0937</td><td> -3</td><td>0</td><td>1712</td><td>1656</td><td>-16</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td> 1</td><td>2017-01-01</td><td>AA</td><td>N820AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1959</td><td>1956</td><td> -3</td><td>0</td><td>0133</td><td>0125</td><td> -8</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td> 3</td><td>2017-01-03</td><td>AA</td><td>N742AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1959</td><td>1955</td><td> -4</td><td>0</td><td>0133</td><td>0105</td><td>-28</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>11</td><td>2017-01-11</td><td>AA</td><td>N742AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1620</td><td>1620</td><td>  0</td><td>0</td><td>2159</td><td>2141</td><td>-18</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>12</td><td>2017-01-12</td><td>AA</td><td>N762AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1620</td><td>1617</td><td> -3</td><td>0</td><td>2159</td><td>2137</td><td>-22</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>13</td><td>2017-01-13</td><td>AA</td><td>N822AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1620</td><td>1617</td><td> -3</td><td>0</td><td>2159</td><td>2136</td><td>-23</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>15</td><td>2017-01-15</td><td>AA</td><td>N817AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1635</td><td>1629</td><td> -6</td><td>0</td><td>2214</td><td>2215</td><td>  1</td><td> 1</td></tr>
	<tr><td>2017</td><td>1</td><td>16</td><td>2017-01-16</td><td>AA</td><td>N755AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1620</td><td>1614</td><td> -6</td><td>0</td><td>2159</td><td>2149</td><td>-10</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>18</td><td>2017-01-18</td><td>AA</td><td>N840AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1620</td><td>1613</td><td> -7</td><td>0</td><td>2159</td><td>2147</td><td>-12</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>19</td><td>2017-01-19</td><td>AA</td><td>N819AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1620</td><td>1620</td><td>  0</td><td>0</td><td>2159</td><td>2200</td><td>  1</td><td> 1</td></tr>
	<tr><td>2017</td><td>1</td><td>22</td><td>2017-01-22</td><td>AA</td><td>N833AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1635</td><td>1629</td><td> -6</td><td>0</td><td>2214</td><td>2207</td><td> -7</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>23</td><td>2017-01-23</td><td>AA</td><td>N760AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1620</td><td>1618</td><td> -2</td><td>0</td><td>2159</td><td>2130</td><td>-29</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>25</td><td>2017-01-25</td><td>AA</td><td>N823AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1620</td><td>1614</td><td> -6</td><td>0</td><td>2159</td><td>2135</td><td>-24</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>26</td><td>2017-01-26</td><td>AA</td><td>N833AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1620</td><td>1615</td><td> -5</td><td>0</td><td>2159</td><td>2134</td><td>-25</td><td> 0</td></tr>
	<tr><td>2017</td><td>1</td><td>28</td><td>2017-01-28</td><td>AA</td><td>N755AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1620</td><td>1610</td><td>-10</td><td>0</td><td>2159</td><td>2207</td><td>  8</td><td> 8</td></tr>
	<tr><td>2017</td><td>1</td><td>29</td><td>2017-01-29</td><td>AA</td><td>N823AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1635</td><td>1633</td><td> -2</td><td>0</td><td>2214</td><td>2232</td><td> 18</td><td>18</td></tr>
	<tr><td>2017</td><td>1</td><td>30</td><td>2017-01-30</td><td>AA</td><td>N760AA</td><td>514</td><td>PHX</td><td>Phoenix, AZ    </td><td>CMH</td><td>Columbus, OH</td><td>1620</td><td>1613</td><td> -7</td><td>0</td><td>2159</td><td>2149</td><td>-10</td><td> 0</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>2017</td><td>9</td><td>20</td><td>2017-09-20</td><td>WN</td><td>N7726A</td><td> 494</td><td>BWI</td><td>Baltimore, MD </td><td>CMH</td><td>Columbus, OH</td><td>1810</td><td>NA</td><td>NA</td><td>NA</td><td>1925</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>10</td><td>2017-09-10</td><td>WN</td><td>NA    </td><td>3675</td><td>MCO</td><td>Orlando, FL   </td><td>CMH</td><td>Columbus, OH</td><td>1545</td><td>NA</td><td>NA</td><td>NA</td><td>1750</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>10</td><td>2017-09-10</td><td>WN</td><td>NA    </td><td>4643</td><td>MCO</td><td>Orlando, FL   </td><td>CMH</td><td>Columbus, OH</td><td>0910</td><td>NA</td><td>NA</td><td>NA</td><td>1115</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>10</td><td>2017-09-10</td><td>WN</td><td>N8327A</td><td>4930</td><td>MCO</td><td>Orlando, FL   </td><td>CMH</td><td>Columbus, OH</td><td>2025</td><td>NA</td><td>NA</td><td>NA</td><td>2235</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>10</td><td>2017-09-10</td><td>WN</td><td>N430WN</td><td>3714</td><td>RSW</td><td>Fort Myers, FL</td><td>CMH</td><td>Columbus, OH</td><td>1210</td><td>NA</td><td>NA</td><td>NA</td><td>1430</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>10</td><td>2017-09-10</td><td>WN</td><td>N7818L</td><td>4454</td><td>TPA</td><td>Tampa, FL     </td><td>CMH</td><td>Columbus, OH</td><td>1740</td><td>NA</td><td>NA</td><td>NA</td><td>1945</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>10</td><td>2017-09-10</td><td>WN</td><td>N915WN</td><td>4701</td><td>TPA</td><td>Tampa, FL     </td><td>CMH</td><td>Columbus, OH</td><td>1055</td><td>NA</td><td>NA</td><td>NA</td><td>1300</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N389SW</td><td>  17</td><td>ATL</td><td>Atlanta, GA   </td><td>CMH</td><td>Columbus, OH</td><td>2200</td><td>NA</td><td>NA</td><td>NA</td><td>2335</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N639SW</td><td> 188</td><td>ATL</td><td>Atlanta, GA   </td><td>CMH</td><td>Columbus, OH</td><td>1525</td><td>NA</td><td>NA</td><td>NA</td><td>1700</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N703SW</td><td>1816</td><td>DEN</td><td>Denver, CO    </td><td>CMH</td><td>Columbus, OH</td><td>1025</td><td>NA</td><td>NA</td><td>NA</td><td>1510</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N7878A</td><td>1792</td><td>MCO</td><td>Orlando, FL   </td><td>CMH</td><td>Columbus, OH</td><td>0905</td><td>NA</td><td>NA</td><td>NA</td><td>1115</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N248WN</td><td>1888</td><td>MCO</td><td>Orlando, FL   </td><td>CMH</td><td>Columbus, OH</td><td>1640</td><td>NA</td><td>NA</td><td>NA</td><td>1850</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N553WN</td><td> 213</td><td>MDW</td><td>Chicago, IL   </td><td>CMH</td><td>Columbus, OH</td><td>1905</td><td>NA</td><td>NA</td><td>NA</td><td>2110</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>20</td><td>2017-09-20</td><td>WN</td><td>N947WN</td><td>5861</td><td>MDW</td><td>Chicago, IL   </td><td>CMH</td><td>Columbus, OH</td><td>1530</td><td>NA</td><td>NA</td><td>NA</td><td>1735</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td> 5</td><td>2017-09-05</td><td>WN</td><td>N726SW</td><td> 494</td><td>BWI</td><td>Baltimore, MD </td><td>CMH</td><td>Columbus, OH</td><td>1810</td><td>NA</td><td>NA</td><td>NA</td><td>1925</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>NA    </td><td> 690</td><td>RSW</td><td>Fort Myers, FL</td><td>CMH</td><td>Columbus, OH</td><td>1215</td><td>NA</td><td>NA</td><td>NA</td><td>1435</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N484WN</td><td> 924</td><td>TPA</td><td>Tampa, FL     </td><td>CMH</td><td>Columbus, OH</td><td>1500</td><td>NA</td><td>NA</td><td>NA</td><td>1710</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>12</td><td>2017-09-12</td><td>WN</td><td>NA    </td><td> 192</td><td>ATL</td><td>Atlanta, GA   </td><td>CMH</td><td>Columbus, OH</td><td>0810</td><td>NA</td><td>NA</td><td>NA</td><td>0940</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>12</td><td>2017-09-12</td><td>WN</td><td>NA    </td><td>1623</td><td>DCA</td><td>Washington, DC</td><td>CMH</td><td>Columbus, OH</td><td>1240</td><td>NA</td><td>NA</td><td>NA</td><td>1355</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>12</td><td>2017-09-12</td><td>WN</td><td>NA    </td><td>1792</td><td>MCO</td><td>Orlando, FL   </td><td>CMH</td><td>Columbus, OH</td><td>0905</td><td>NA</td><td>NA</td><td>NA</td><td>1115</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>12</td><td>2017-09-12</td><td>WN</td><td>NA    </td><td> 690</td><td>RSW</td><td>Fort Myers, FL</td><td>CMH</td><td>Columbus, OH</td><td>1215</td><td>NA</td><td>NA</td><td>NA</td><td>1435</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>12</td><td>2017-09-12</td><td>WN</td><td>N426WN</td><td> 924</td><td>TPA</td><td>Tampa, FL     </td><td>CMH</td><td>Columbus, OH</td><td>1500</td><td>NA</td><td>NA</td><td>NA</td><td>1710</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>13</td><td>2017-09-13</td><td>WN</td><td>NA    </td><td> 938</td><td>MDW</td><td>Chicago, IL   </td><td>CMH</td><td>Columbus, OH</td><td>1345</td><td>NA</td><td>NA</td><td>NA</td><td>1555</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>13</td><td>2017-09-13</td><td>WN</td><td>NA    </td><td> 690</td><td>RSW</td><td>Fort Myers, FL</td><td>CMH</td><td>Columbus, OH</td><td>1215</td><td>NA</td><td>NA</td><td>NA</td><td>1435</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>13</td><td>2017-09-13</td><td>WN</td><td>NA    </td><td> 924</td><td>TPA</td><td>Tampa, FL     </td><td>CMH</td><td>Columbus, OH</td><td>1500</td><td>NA</td><td>NA</td><td>NA</td><td>1710</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>14</td><td>2017-09-14</td><td>WN</td><td>N240WN</td><td> 938</td><td>MDW</td><td>Chicago, IL   </td><td>CMH</td><td>Columbus, OH</td><td>1345</td><td>NA</td><td>NA</td><td>NA</td><td>1555</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>14</td><td>2017-09-14</td><td>WN</td><td>N430WN</td><td>1303</td><td>MDW</td><td>Chicago, IL   </td><td>CMH</td><td>Columbus, OH</td><td>1000</td><td>NA</td><td>NA</td><td>NA</td><td>1205</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>14</td><td>2017-09-14</td><td>WN</td><td>N403WN</td><td>5861</td><td>MDW</td><td>Chicago, IL   </td><td>CMH</td><td>Columbus, OH</td><td>1530</td><td>NA</td><td>NA</td><td>NA</td><td>1735</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>DL</td><td>N935DN</td><td>1646</td><td>ATL</td><td>Atlanta, GA   </td><td>CMH</td><td>Columbus, OH</td><td>1622</td><td>NA</td><td>NA</td><td>NA</td><td>1800</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>DL</td><td>N921DL</td><td>1695</td><td>ATL</td><td>Atlanta, GA   </td><td>CMH</td><td>Columbus, OH</td><td>1430</td><td>NA</td><td>NA</td><td>NA</td><td>1603</td><td>NA</td><td>NA</td><td>NA</td></tr>
</tbody>
</table>



And now in `descending order` of delays by adding the minus symbol `-` to the  column name. 


```R
my.df %>% 
  arrange(-DepDelayMinutes) -> my.df2

my.df2
```


<table class="dataframe">
<caption>A tibble: 18799 × 19</caption>
<thead>
	<tr><th scope=col>Year</th><th scope=col>Month</th><th scope=col>DayofMonth</th><th scope=col>FlightDate</th><th scope=col>Carrier</th><th scope=col>TailNum</th><th scope=col>FlightNum</th><th scope=col>Origin</th><th scope=col>OriginCityName</th><th scope=col>Dest</th><th scope=col>DestCityName</th><th scope=col>CRSDepTime</th><th scope=col>DepTime</th><th scope=col>DepDelay</th><th scope=col>DepDelayMinutes</th><th scope=col>CRSArrTime</th><th scope=col>ArrTime</th><th scope=col>ArrDelay</th><th scope=col>ArrDelayMinutes</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;date&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2017</td><td>6</td><td>18</td><td>2017-06-18</td><td>EV</td><td>N18556</td><td>4080</td><td>CMH</td><td>Columbus, OH         </td><td>ORD</td><td>Chicago, IL </td><td>1340</td><td>0644</td><td>1024</td><td>1024</td><td>1405</td><td>0705</td><td>1020</td><td>1020</td></tr>
	<tr><td>2017</td><td>6</td><td>29</td><td>2017-06-29</td><td>DL</td><td>N956DL</td><td>1897</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>2109</td><td>0555</td><td> 526</td><td> 526</td><td>2249</td><td>0713</td><td> 504</td><td> 504</td></tr>
	<tr><td>2017</td><td>8</td><td> 9</td><td>2017-08-09</td><td>F9</td><td>N223FR</td><td>1686</td><td>LAS</td><td>Las Vegas, NV        </td><td>CMH</td><td>Columbus, OH</td><td>1615</td><td>0007</td><td> 472</td><td> 472</td><td>2314</td><td>0650</td><td> 456</td><td> 456</td></tr>
	<tr><td>2017</td><td>1</td><td>29</td><td>2017-01-29</td><td>DL</td><td>N838DN</td><td>1897</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>2025</td><td>0330</td><td> 425</td><td> 425</td><td>2157</td><td>0503</td><td> 426</td><td> 426</td></tr>
	<tr><td>2017</td><td>4</td><td> 8</td><td>2017-04-08</td><td>DL</td><td>N984DL</td><td>1276</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>0925</td><td>1616</td><td> 411</td><td> 411</td><td>1058</td><td>1755</td><td> 417</td><td> 417</td></tr>
	<tr><td>2017</td><td>1</td><td>23</td><td>2017-01-23</td><td>WN</td><td>N731SA</td><td>1886</td><td>DCA</td><td>Washington, DC       </td><td>CMH</td><td>Columbus, OH</td><td>1125</td><td>1807</td><td> 402</td><td> 402</td><td>1250</td><td>1950</td><td> 420</td><td> 420</td></tr>
	<tr><td>2017</td><td>2</td><td>19</td><td>2017-02-19</td><td>UA</td><td>N812UA</td><td> 515</td><td>CMH</td><td>Columbus, OH         </td><td>ORD</td><td>Chicago, IL </td><td>1750</td><td>0024</td><td> 394</td><td> 394</td><td>1821</td><td>0031</td><td> 370</td><td> 370</td></tr>
	<tr><td>2017</td><td>1</td><td>16</td><td>2017-01-16</td><td>EV</td><td>N607LR</td><td>5440</td><td>LGA</td><td>New York, NY         </td><td>CMH</td><td>Columbus, OH</td><td>1559</td><td>2216</td><td> 377</td><td> 377</td><td>1804</td><td>2347</td><td> 343</td><td> 343</td></tr>
	<tr><td>2017</td><td>2</td><td>24</td><td>2017-02-24</td><td>UA</td><td>N849UA</td><td> 688</td><td>ORD</td><td>Chicago, IL          </td><td>CMH</td><td>Columbus, OH</td><td>1456</td><td>2112</td><td> 376</td><td> 376</td><td>1715</td><td>2349</td><td> 394</td><td> 394</td></tr>
	<tr><td>2017</td><td>8</td><td>17</td><td>2017-08-17</td><td>WN</td><td>N955WN</td><td>1031</td><td>STL</td><td>St. Louis, MO        </td><td>CMH</td><td>Columbus, OH</td><td>0820</td><td>1429</td><td> 369</td><td> 369</td><td>1045</td><td>1720</td><td> 395</td><td> 395</td></tr>
	<tr><td>2017</td><td>4</td><td>30</td><td>2017-04-30</td><td>WN</td><td>N937WN</td><td>5531</td><td>MSY</td><td>New Orleans, LA      </td><td>CMH</td><td>Columbus, OH</td><td>1540</td><td>2143</td><td> 363</td><td> 363</td><td>1850</td><td>0028</td><td> 338</td><td> 338</td></tr>
	<tr><td>2017</td><td>4</td><td> 3</td><td>2017-04-03</td><td>DL</td><td>N930DL</td><td>1193</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>1747</td><td>2347</td><td> 360</td><td> 360</td><td>1919</td><td>0117</td><td> 358</td><td> 358</td></tr>
	<tr><td>2017</td><td>1</td><td>24</td><td>2017-01-24</td><td>EV</td><td>N691CA</td><td>5333</td><td>MSP</td><td>Minneapolis, MN      </td><td>CMH</td><td>Columbus, OH</td><td>1015</td><td>1611</td><td> 356</td><td> 356</td><td>1328</td><td>1900</td><td> 332</td><td> 332</td></tr>
	<tr><td>2017</td><td>7</td><td>18</td><td>2017-07-18</td><td>WN</td><td>N7733B</td><td>3263</td><td>BOS</td><td>Boston, MA           </td><td>CMH</td><td>Columbus, OH</td><td>0910</td><td>1505</td><td> 355</td><td> 355</td><td>1120</td><td>1651</td><td> 331</td><td> 331</td></tr>
	<tr><td>2017</td><td>1</td><td>11</td><td>2017-01-11</td><td>WN</td><td>N492WN</td><td> 254</td><td>MDW</td><td>Chicago, IL          </td><td>CMH</td><td>Columbus, OH</td><td>1340</td><td>1934</td><td> 354</td><td> 354</td><td>1550</td><td>2133</td><td> 343</td><td> 343</td></tr>
	<tr><td>2017</td><td>3</td><td> 7</td><td>2017-03-07</td><td>F9</td><td>N910FR</td><td>1694</td><td>MCO</td><td>Orlando, FL          </td><td>CMH</td><td>Columbus, OH</td><td>0900</td><td>1442</td><td> 342</td><td> 342</td><td>1112</td><td>1645</td><td> 333</td><td> 333</td></tr>
	<tr><td>2017</td><td>6</td><td>14</td><td>2017-06-14</td><td>WN</td><td>N224WN</td><td>1639</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>1520</td><td>2102</td><td> 342</td><td> 342</td><td>1655</td><td>2314</td><td> 379</td><td> 379</td></tr>
	<tr><td>2017</td><td>4</td><td> 3</td><td>2017-04-03</td><td>DL</td><td>N946DL</td><td>1152</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>1315</td><td>1856</td><td> 341</td><td> 341</td><td>1445</td><td>2026</td><td> 341</td><td> 341</td></tr>
	<tr><td>2017</td><td>5</td><td>24</td><td>2017-05-24</td><td>WN</td><td>N7707C</td><td> 158</td><td>TPA</td><td>Tampa, FL            </td><td>CMH</td><td>Columbus, OH</td><td>1540</td><td>2119</td><td> 339</td><td> 339</td><td>1755</td><td>2312</td><td> 317</td><td> 317</td></tr>
	<tr><td>2017</td><td>4</td><td> 5</td><td>2017-04-05</td><td>WN</td><td>N635SW</td><td>2587</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>1600</td><td>2138</td><td> 338</td><td> 338</td><td>1730</td><td>0028</td><td> 418</td><td> 418</td></tr>
	<tr><td>2017</td><td>5</td><td>11</td><td>2017-05-11</td><td>AA</td><td>N4YLAA</td><td>2452</td><td>DFW</td><td>Dallas/Fort Worth, TX</td><td>CMH</td><td>Columbus, OH</td><td>1830</td><td>0006</td><td> 336</td><td> 336</td><td>2151</td><td>0310</td><td> 319</td><td> 319</td></tr>
	<tr><td>2017</td><td>4</td><td> 6</td><td>2017-04-06</td><td>WN</td><td>N429WN</td><td>1953</td><td>TPA</td><td>Tampa, FL            </td><td>CMH</td><td>Columbus, OH</td><td>1600</td><td>2132</td><td> 332</td><td> 332</td><td>1815</td><td>2400</td><td> 345</td><td> 345</td></tr>
	<tr><td>2017</td><td>5</td><td> 7</td><td>2017-05-07</td><td>WN</td><td>N8670A</td><td>2747</td><td>OAK</td><td>Oakland, CA          </td><td>CMH</td><td>Columbus, OH</td><td>1140</td><td>1704</td><td> 324</td><td> 324</td><td>1910</td><td>0031</td><td> 321</td><td> 321</td></tr>
	<tr><td>2017</td><td>4</td><td> 3</td><td>2017-04-03</td><td>DL</td><td>N329NW</td><td>1398</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>1941</td><td>0058</td><td> 317</td><td> 317</td><td>2116</td><td>0218</td><td> 302</td><td> 302</td></tr>
	<tr><td>2017</td><td>7</td><td> 9</td><td>2017-07-09</td><td>AA</td><td>N812AA</td><td> 508</td><td>LAX</td><td>Los Angeles, CA      </td><td>CMH</td><td>Columbus, OH</td><td>0940</td><td>1457</td><td> 317</td><td> 317</td><td>1715</td><td>2227</td><td> 312</td><td> 312</td></tr>
	<tr><td>2017</td><td>7</td><td> 2</td><td>2017-07-02</td><td>F9</td><td>N920FR</td><td>1694</td><td>MCO</td><td>Orlando, FL          </td><td>CMH</td><td>Columbus, OH</td><td>0950</td><td>1506</td><td> 316</td><td> 316</td><td>1204</td><td>1720</td><td> 316</td><td> 316</td></tr>
	<tr><td>2017</td><td>3</td><td> 9</td><td>2017-03-09</td><td>WN</td><td>N376SW</td><td>1782</td><td>BOS</td><td>Boston, MA           </td><td>CMH</td><td>Columbus, OH</td><td>0805</td><td>1318</td><td> 313</td><td> 313</td><td>1020</td><td>1530</td><td> 310</td><td> 310</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>DL</td><td>N938DN</td><td>1152</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>1208</td><td>1720</td><td> 312</td><td> 312</td><td>1345</td><td>1838</td><td> 293</td><td> 293</td></tr>
	<tr><td>2017</td><td>4</td><td> 9</td><td>2017-04-09</td><td>WN</td><td>N621SW</td><td>6325</td><td>BNA</td><td>Nashville, TN        </td><td>CMH</td><td>Columbus, OH</td><td>1135</td><td>1646</td><td> 311</td><td> 311</td><td>1340</td><td>1854</td><td> 314</td><td> 314</td></tr>
	<tr><td>2017</td><td>6</td><td>15</td><td>2017-06-15</td><td>AA</td><td>N4XMAA</td><td> 909</td><td>DFW</td><td>Dallas/Fort Worth, TX</td><td>CMH</td><td>Columbus, OH</td><td>1055</td><td>1606</td><td> 311</td><td> 311</td><td>1421</td><td>1930</td><td> 309</td><td> 309</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>2017</td><td>9</td><td>20</td><td>2017-09-20</td><td>WN</td><td>N7726A</td><td> 494</td><td>BWI</td><td>Baltimore, MD </td><td>CMH</td><td>Columbus, OH</td><td>1810</td><td>NA</td><td>NA</td><td>NA</td><td>1925</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>10</td><td>2017-09-10</td><td>WN</td><td>NA    </td><td>3675</td><td>MCO</td><td>Orlando, FL   </td><td>CMH</td><td>Columbus, OH</td><td>1545</td><td>NA</td><td>NA</td><td>NA</td><td>1750</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>10</td><td>2017-09-10</td><td>WN</td><td>NA    </td><td>4643</td><td>MCO</td><td>Orlando, FL   </td><td>CMH</td><td>Columbus, OH</td><td>0910</td><td>NA</td><td>NA</td><td>NA</td><td>1115</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>10</td><td>2017-09-10</td><td>WN</td><td>N8327A</td><td>4930</td><td>MCO</td><td>Orlando, FL   </td><td>CMH</td><td>Columbus, OH</td><td>2025</td><td>NA</td><td>NA</td><td>NA</td><td>2235</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>10</td><td>2017-09-10</td><td>WN</td><td>N430WN</td><td>3714</td><td>RSW</td><td>Fort Myers, FL</td><td>CMH</td><td>Columbus, OH</td><td>1210</td><td>NA</td><td>NA</td><td>NA</td><td>1430</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>10</td><td>2017-09-10</td><td>WN</td><td>N7818L</td><td>4454</td><td>TPA</td><td>Tampa, FL     </td><td>CMH</td><td>Columbus, OH</td><td>1740</td><td>NA</td><td>NA</td><td>NA</td><td>1945</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>10</td><td>2017-09-10</td><td>WN</td><td>N915WN</td><td>4701</td><td>TPA</td><td>Tampa, FL     </td><td>CMH</td><td>Columbus, OH</td><td>1055</td><td>NA</td><td>NA</td><td>NA</td><td>1300</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N389SW</td><td>  17</td><td>ATL</td><td>Atlanta, GA   </td><td>CMH</td><td>Columbus, OH</td><td>2200</td><td>NA</td><td>NA</td><td>NA</td><td>2335</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N639SW</td><td> 188</td><td>ATL</td><td>Atlanta, GA   </td><td>CMH</td><td>Columbus, OH</td><td>1525</td><td>NA</td><td>NA</td><td>NA</td><td>1700</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N703SW</td><td>1816</td><td>DEN</td><td>Denver, CO    </td><td>CMH</td><td>Columbus, OH</td><td>1025</td><td>NA</td><td>NA</td><td>NA</td><td>1510</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N7878A</td><td>1792</td><td>MCO</td><td>Orlando, FL   </td><td>CMH</td><td>Columbus, OH</td><td>0905</td><td>NA</td><td>NA</td><td>NA</td><td>1115</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N248WN</td><td>1888</td><td>MCO</td><td>Orlando, FL   </td><td>CMH</td><td>Columbus, OH</td><td>1640</td><td>NA</td><td>NA</td><td>NA</td><td>1850</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N553WN</td><td> 213</td><td>MDW</td><td>Chicago, IL   </td><td>CMH</td><td>Columbus, OH</td><td>1905</td><td>NA</td><td>NA</td><td>NA</td><td>2110</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>20</td><td>2017-09-20</td><td>WN</td><td>N947WN</td><td>5861</td><td>MDW</td><td>Chicago, IL   </td><td>CMH</td><td>Columbus, OH</td><td>1530</td><td>NA</td><td>NA</td><td>NA</td><td>1735</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td> 5</td><td>2017-09-05</td><td>WN</td><td>N726SW</td><td> 494</td><td>BWI</td><td>Baltimore, MD </td><td>CMH</td><td>Columbus, OH</td><td>1810</td><td>NA</td><td>NA</td><td>NA</td><td>1925</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>NA    </td><td> 690</td><td>RSW</td><td>Fort Myers, FL</td><td>CMH</td><td>Columbus, OH</td><td>1215</td><td>NA</td><td>NA</td><td>NA</td><td>1435</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>WN</td><td>N484WN</td><td> 924</td><td>TPA</td><td>Tampa, FL     </td><td>CMH</td><td>Columbus, OH</td><td>1500</td><td>NA</td><td>NA</td><td>NA</td><td>1710</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>12</td><td>2017-09-12</td><td>WN</td><td>NA    </td><td> 192</td><td>ATL</td><td>Atlanta, GA   </td><td>CMH</td><td>Columbus, OH</td><td>0810</td><td>NA</td><td>NA</td><td>NA</td><td>0940</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>12</td><td>2017-09-12</td><td>WN</td><td>NA    </td><td>1623</td><td>DCA</td><td>Washington, DC</td><td>CMH</td><td>Columbus, OH</td><td>1240</td><td>NA</td><td>NA</td><td>NA</td><td>1355</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>12</td><td>2017-09-12</td><td>WN</td><td>NA    </td><td>1792</td><td>MCO</td><td>Orlando, FL   </td><td>CMH</td><td>Columbus, OH</td><td>0905</td><td>NA</td><td>NA</td><td>NA</td><td>1115</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>12</td><td>2017-09-12</td><td>WN</td><td>NA    </td><td> 690</td><td>RSW</td><td>Fort Myers, FL</td><td>CMH</td><td>Columbus, OH</td><td>1215</td><td>NA</td><td>NA</td><td>NA</td><td>1435</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>12</td><td>2017-09-12</td><td>WN</td><td>N426WN</td><td> 924</td><td>TPA</td><td>Tampa, FL     </td><td>CMH</td><td>Columbus, OH</td><td>1500</td><td>NA</td><td>NA</td><td>NA</td><td>1710</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>13</td><td>2017-09-13</td><td>WN</td><td>NA    </td><td> 938</td><td>MDW</td><td>Chicago, IL   </td><td>CMH</td><td>Columbus, OH</td><td>1345</td><td>NA</td><td>NA</td><td>NA</td><td>1555</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>13</td><td>2017-09-13</td><td>WN</td><td>NA    </td><td> 690</td><td>RSW</td><td>Fort Myers, FL</td><td>CMH</td><td>Columbus, OH</td><td>1215</td><td>NA</td><td>NA</td><td>NA</td><td>1435</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>13</td><td>2017-09-13</td><td>WN</td><td>NA    </td><td> 924</td><td>TPA</td><td>Tampa, FL     </td><td>CMH</td><td>Columbus, OH</td><td>1500</td><td>NA</td><td>NA</td><td>NA</td><td>1710</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>14</td><td>2017-09-14</td><td>WN</td><td>N240WN</td><td> 938</td><td>MDW</td><td>Chicago, IL   </td><td>CMH</td><td>Columbus, OH</td><td>1345</td><td>NA</td><td>NA</td><td>NA</td><td>1555</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>14</td><td>2017-09-14</td><td>WN</td><td>N430WN</td><td>1303</td><td>MDW</td><td>Chicago, IL   </td><td>CMH</td><td>Columbus, OH</td><td>1000</td><td>NA</td><td>NA</td><td>NA</td><td>1205</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>14</td><td>2017-09-14</td><td>WN</td><td>N403WN</td><td>5861</td><td>MDW</td><td>Chicago, IL   </td><td>CMH</td><td>Columbus, OH</td><td>1530</td><td>NA</td><td>NA</td><td>NA</td><td>1735</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>DL</td><td>N935DN</td><td>1646</td><td>ATL</td><td>Atlanta, GA   </td><td>CMH</td><td>Columbus, OH</td><td>1622</td><td>NA</td><td>NA</td><td>NA</td><td>1800</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>DL</td><td>N921DL</td><td>1695</td><td>ATL</td><td>Atlanta, GA   </td><td>CMH</td><td>Columbus, OH</td><td>1430</td><td>NA</td><td>NA</td><td>NA</td><td>1603</td><td>NA</td><td>NA</td><td>NA</td></tr>
</tbody>
</table>



We could tweak this further, perhaps saying sort by departure delays to CMH, and then to ORD. 


```R
my.df %>% 
  arrange(Dest, -DepDelayMinutes) -> my.df2

my.df2
```


<table class="dataframe">
<caption>A tibble: 18799 × 19</caption>
<thead>
	<tr><th scope=col>Year</th><th scope=col>Month</th><th scope=col>DayofMonth</th><th scope=col>FlightDate</th><th scope=col>Carrier</th><th scope=col>TailNum</th><th scope=col>FlightNum</th><th scope=col>Origin</th><th scope=col>OriginCityName</th><th scope=col>Dest</th><th scope=col>DestCityName</th><th scope=col>CRSDepTime</th><th scope=col>DepTime</th><th scope=col>DepDelay</th><th scope=col>DepDelayMinutes</th><th scope=col>CRSArrTime</th><th scope=col>ArrTime</th><th scope=col>ArrDelay</th><th scope=col>ArrDelayMinutes</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;date&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2017</td><td>6</td><td>29</td><td>2017-06-29</td><td>DL</td><td>N956DL</td><td>1897</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>2109</td><td>0555</td><td>526</td><td>526</td><td>2249</td><td>0713</td><td>504</td><td>504</td></tr>
	<tr><td>2017</td><td>8</td><td> 9</td><td>2017-08-09</td><td>F9</td><td>N223FR</td><td>1686</td><td>LAS</td><td>Las Vegas, NV        </td><td>CMH</td><td>Columbus, OH</td><td>1615</td><td>0007</td><td>472</td><td>472</td><td>2314</td><td>0650</td><td>456</td><td>456</td></tr>
	<tr><td>2017</td><td>1</td><td>29</td><td>2017-01-29</td><td>DL</td><td>N838DN</td><td>1897</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>2025</td><td>0330</td><td>425</td><td>425</td><td>2157</td><td>0503</td><td>426</td><td>426</td></tr>
	<tr><td>2017</td><td>4</td><td> 8</td><td>2017-04-08</td><td>DL</td><td>N984DL</td><td>1276</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>0925</td><td>1616</td><td>411</td><td>411</td><td>1058</td><td>1755</td><td>417</td><td>417</td></tr>
	<tr><td>2017</td><td>1</td><td>23</td><td>2017-01-23</td><td>WN</td><td>N731SA</td><td>1886</td><td>DCA</td><td>Washington, DC       </td><td>CMH</td><td>Columbus, OH</td><td>1125</td><td>1807</td><td>402</td><td>402</td><td>1250</td><td>1950</td><td>420</td><td>420</td></tr>
	<tr><td>2017</td><td>1</td><td>16</td><td>2017-01-16</td><td>EV</td><td>N607LR</td><td>5440</td><td>LGA</td><td>New York, NY         </td><td>CMH</td><td>Columbus, OH</td><td>1559</td><td>2216</td><td>377</td><td>377</td><td>1804</td><td>2347</td><td>343</td><td>343</td></tr>
	<tr><td>2017</td><td>2</td><td>24</td><td>2017-02-24</td><td>UA</td><td>N849UA</td><td> 688</td><td>ORD</td><td>Chicago, IL          </td><td>CMH</td><td>Columbus, OH</td><td>1456</td><td>2112</td><td>376</td><td>376</td><td>1715</td><td>2349</td><td>394</td><td>394</td></tr>
	<tr><td>2017</td><td>8</td><td>17</td><td>2017-08-17</td><td>WN</td><td>N955WN</td><td>1031</td><td>STL</td><td>St. Louis, MO        </td><td>CMH</td><td>Columbus, OH</td><td>0820</td><td>1429</td><td>369</td><td>369</td><td>1045</td><td>1720</td><td>395</td><td>395</td></tr>
	<tr><td>2017</td><td>4</td><td>30</td><td>2017-04-30</td><td>WN</td><td>N937WN</td><td>5531</td><td>MSY</td><td>New Orleans, LA      </td><td>CMH</td><td>Columbus, OH</td><td>1540</td><td>2143</td><td>363</td><td>363</td><td>1850</td><td>0028</td><td>338</td><td>338</td></tr>
	<tr><td>2017</td><td>4</td><td> 3</td><td>2017-04-03</td><td>DL</td><td>N930DL</td><td>1193</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>1747</td><td>2347</td><td>360</td><td>360</td><td>1919</td><td>0117</td><td>358</td><td>358</td></tr>
	<tr><td>2017</td><td>1</td><td>24</td><td>2017-01-24</td><td>EV</td><td>N691CA</td><td>5333</td><td>MSP</td><td>Minneapolis, MN      </td><td>CMH</td><td>Columbus, OH</td><td>1015</td><td>1611</td><td>356</td><td>356</td><td>1328</td><td>1900</td><td>332</td><td>332</td></tr>
	<tr><td>2017</td><td>7</td><td>18</td><td>2017-07-18</td><td>WN</td><td>N7733B</td><td>3263</td><td>BOS</td><td>Boston, MA           </td><td>CMH</td><td>Columbus, OH</td><td>0910</td><td>1505</td><td>355</td><td>355</td><td>1120</td><td>1651</td><td>331</td><td>331</td></tr>
	<tr><td>2017</td><td>1</td><td>11</td><td>2017-01-11</td><td>WN</td><td>N492WN</td><td> 254</td><td>MDW</td><td>Chicago, IL          </td><td>CMH</td><td>Columbus, OH</td><td>1340</td><td>1934</td><td>354</td><td>354</td><td>1550</td><td>2133</td><td>343</td><td>343</td></tr>
	<tr><td>2017</td><td>3</td><td> 7</td><td>2017-03-07</td><td>F9</td><td>N910FR</td><td>1694</td><td>MCO</td><td>Orlando, FL          </td><td>CMH</td><td>Columbus, OH</td><td>0900</td><td>1442</td><td>342</td><td>342</td><td>1112</td><td>1645</td><td>333</td><td>333</td></tr>
	<tr><td>2017</td><td>6</td><td>14</td><td>2017-06-14</td><td>WN</td><td>N224WN</td><td>1639</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>1520</td><td>2102</td><td>342</td><td>342</td><td>1655</td><td>2314</td><td>379</td><td>379</td></tr>
	<tr><td>2017</td><td>4</td><td> 3</td><td>2017-04-03</td><td>DL</td><td>N946DL</td><td>1152</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>1315</td><td>1856</td><td>341</td><td>341</td><td>1445</td><td>2026</td><td>341</td><td>341</td></tr>
	<tr><td>2017</td><td>5</td><td>24</td><td>2017-05-24</td><td>WN</td><td>N7707C</td><td> 158</td><td>TPA</td><td>Tampa, FL            </td><td>CMH</td><td>Columbus, OH</td><td>1540</td><td>2119</td><td>339</td><td>339</td><td>1755</td><td>2312</td><td>317</td><td>317</td></tr>
	<tr><td>2017</td><td>4</td><td> 5</td><td>2017-04-05</td><td>WN</td><td>N635SW</td><td>2587</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>1600</td><td>2138</td><td>338</td><td>338</td><td>1730</td><td>0028</td><td>418</td><td>418</td></tr>
	<tr><td>2017</td><td>5</td><td>11</td><td>2017-05-11</td><td>AA</td><td>N4YLAA</td><td>2452</td><td>DFW</td><td>Dallas/Fort Worth, TX</td><td>CMH</td><td>Columbus, OH</td><td>1830</td><td>0006</td><td>336</td><td>336</td><td>2151</td><td>0310</td><td>319</td><td>319</td></tr>
	<tr><td>2017</td><td>4</td><td> 6</td><td>2017-04-06</td><td>WN</td><td>N429WN</td><td>1953</td><td>TPA</td><td>Tampa, FL            </td><td>CMH</td><td>Columbus, OH</td><td>1600</td><td>2132</td><td>332</td><td>332</td><td>1815</td><td>2400</td><td>345</td><td>345</td></tr>
	<tr><td>2017</td><td>5</td><td> 7</td><td>2017-05-07</td><td>WN</td><td>N8670A</td><td>2747</td><td>OAK</td><td>Oakland, CA          </td><td>CMH</td><td>Columbus, OH</td><td>1140</td><td>1704</td><td>324</td><td>324</td><td>1910</td><td>0031</td><td>321</td><td>321</td></tr>
	<tr><td>2017</td><td>4</td><td> 3</td><td>2017-04-03</td><td>DL</td><td>N329NW</td><td>1398</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>1941</td><td>0058</td><td>317</td><td>317</td><td>2116</td><td>0218</td><td>302</td><td>302</td></tr>
	<tr><td>2017</td><td>7</td><td> 9</td><td>2017-07-09</td><td>AA</td><td>N812AA</td><td> 508</td><td>LAX</td><td>Los Angeles, CA      </td><td>CMH</td><td>Columbus, OH</td><td>0940</td><td>1457</td><td>317</td><td>317</td><td>1715</td><td>2227</td><td>312</td><td>312</td></tr>
	<tr><td>2017</td><td>7</td><td> 2</td><td>2017-07-02</td><td>F9</td><td>N920FR</td><td>1694</td><td>MCO</td><td>Orlando, FL          </td><td>CMH</td><td>Columbus, OH</td><td>0950</td><td>1506</td><td>316</td><td>316</td><td>1204</td><td>1720</td><td>316</td><td>316</td></tr>
	<tr><td>2017</td><td>3</td><td> 9</td><td>2017-03-09</td><td>WN</td><td>N376SW</td><td>1782</td><td>BOS</td><td>Boston, MA           </td><td>CMH</td><td>Columbus, OH</td><td>0805</td><td>1318</td><td>313</td><td>313</td><td>1020</td><td>1530</td><td>310</td><td>310</td></tr>
	<tr><td>2017</td><td>9</td><td>11</td><td>2017-09-11</td><td>DL</td><td>N938DN</td><td>1152</td><td>ATL</td><td>Atlanta, GA          </td><td>CMH</td><td>Columbus, OH</td><td>1208</td><td>1720</td><td>312</td><td>312</td><td>1345</td><td>1838</td><td>293</td><td>293</td></tr>
	<tr><td>2017</td><td>4</td><td> 9</td><td>2017-04-09</td><td>WN</td><td>N621SW</td><td>6325</td><td>BNA</td><td>Nashville, TN        </td><td>CMH</td><td>Columbus, OH</td><td>1135</td><td>1646</td><td>311</td><td>311</td><td>1340</td><td>1854</td><td>314</td><td>314</td></tr>
	<tr><td>2017</td><td>6</td><td>15</td><td>2017-06-15</td><td>AA</td><td>N4XMAA</td><td> 909</td><td>DFW</td><td>Dallas/Fort Worth, TX</td><td>CMH</td><td>Columbus, OH</td><td>1055</td><td>1606</td><td>311</td><td>311</td><td>1421</td><td>1930</td><td>309</td><td>309</td></tr>
	<tr><td>2017</td><td>6</td><td>19</td><td>2017-06-19</td><td>OO</td><td>N935SW</td><td>4520</td><td>DTW</td><td>Detroit, MI          </td><td>CMH</td><td>Columbus, OH</td><td>0835</td><td>1331</td><td>296</td><td>296</td><td>0939</td><td>1424</td><td>285</td><td>285</td></tr>
	<tr><td>2017</td><td>7</td><td>14</td><td>2017-07-14</td><td>WN</td><td>N467WN</td><td>1922</td><td>DAL</td><td>Dallas, TX           </td><td>CMH</td><td>Columbus, OH</td><td>1930</td><td>0024</td><td>294</td><td>294</td><td>2250</td><td>0335</td><td>285</td><td>285</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>2017</td><td>9</td><td>21</td><td>2017-09-21</td><td>UA</td><td>N78506</td><td>1014</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1050</td><td>1045</td><td> -5</td><td> 0</td><td>1115</td><td>1050</td><td>-25</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td>20</td><td>2017-09-20</td><td>UA</td><td>N24729</td><td> 515</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1721</td><td>1717</td><td> -4</td><td> 0</td><td>1750</td><td>1721</td><td>-29</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td>20</td><td>2017-09-20</td><td>UA</td><td>N35260</td><td>1014</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1050</td><td>1043</td><td> -7</td><td> 0</td><td>1115</td><td>1058</td><td>-17</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td>19</td><td>2017-09-19</td><td>UA</td><td>N16713</td><td> 515</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1721</td><td>1711</td><td>-10</td><td> 0</td><td>1750</td><td>1803</td><td> 13</td><td>13</td></tr>
	<tr><td>2017</td><td>9</td><td>19</td><td>2017-09-19</td><td>UA</td><td>N76533</td><td>1014</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1050</td><td>1045</td><td> -5</td><td> 0</td><td>1115</td><td>1107</td><td> -8</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td>18</td><td>2017-09-18</td><td>UA</td><td>N24729</td><td> 515</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1721</td><td>1718</td><td> -3</td><td> 0</td><td>1750</td><td>1748</td><td> -2</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td> 3</td><td>2017-09-03</td><td>UA</td><td>N892UA</td><td> 515</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1611</td><td>1607</td><td> -4</td><td> 0</td><td>1644</td><td>1609</td><td>-35</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td> 2</td><td>2017-09-02</td><td>UA</td><td>N463UA</td><td> 515</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1757</td><td>1745</td><td>-12</td><td> 0</td><td>1828</td><td>1747</td><td>-41</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td> 2</td><td>2017-09-02</td><td>UA</td><td>N842UA</td><td> 805</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1045</td><td>1040</td><td> -5</td><td> 0</td><td>1109</td><td>1039</td><td>-30</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td> 1</td><td>2017-09-01</td><td>UA</td><td>N38268</td><td> 515</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1801</td><td>1759</td><td> -2</td><td> 0</td><td>1833</td><td>1842</td><td>  9</td><td> 9</td></tr>
	<tr><td>2017</td><td>9</td><td>18</td><td>2017-09-18</td><td>UA</td><td>N73276</td><td>1014</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1050</td><td>1045</td><td> -5</td><td> 0</td><td>1115</td><td>1110</td><td> -5</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td>15</td><td>2017-09-15</td><td>UA</td><td>N24729</td><td> 515</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1721</td><td>1717</td><td> -4</td><td> 0</td><td>1750</td><td>1714</td><td>-36</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td>15</td><td>2017-09-15</td><td>UA</td><td>N13248</td><td>1014</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1050</td><td>1045</td><td> -5</td><td> 0</td><td>1115</td><td>1104</td><td>-11</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td>14</td><td>2017-09-14</td><td>UA</td><td>N17752</td><td> 515</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1721</td><td>1705</td><td>-16</td><td> 0</td><td>1750</td><td>1708</td><td>-42</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td> 1</td><td>2017-09-01</td><td>UA</td><td>N17753</td><td>1802</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1928</td><td>1923</td><td> -5</td><td> 0</td><td>1959</td><td>2019</td><td> 20</td><td>20</td></tr>
	<tr><td>2017</td><td>9</td><td> 9</td><td>2017-09-09</td><td>EV</td><td>N13550</td><td>3795</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1345</td><td>1336</td><td> -9</td><td> 0</td><td>1413</td><td>1409</td><td> -4</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td>16</td><td>2017-09-16</td><td>EV</td><td>N13913</td><td>3795</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1345</td><td>1335</td><td>-10</td><td> 0</td><td>1413</td><td>1353</td><td>-20</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td>23</td><td>2017-09-23</td><td>EV</td><td>N14993</td><td>3795</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1345</td><td>1340</td><td> -5</td><td> 0</td><td>1413</td><td>1402</td><td>-11</td><td> 0</td></tr>
	<tr><td>2017</td><td>9</td><td>30</td><td>2017-09-30</td><td>EV</td><td>N26545</td><td>3795</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>1345</td><td>1335</td><td>-10</td><td> 0</td><td>1413</td><td>1400</td><td>-13</td><td> 0</td></tr>
	<tr><td>2017</td><td>5</td><td>18</td><td>2017-05-18</td><td>UA</td><td>NA    </td><td> 245</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>0730</td><td>NA  </td><td> NA</td><td>NA</td><td>0757</td><td>NA  </td><td> NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>6</td><td>13</td><td>2017-06-13</td><td>OO</td><td>N141SY</td><td>5551</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>2043</td><td>NA  </td><td> NA</td><td>NA</td><td>2113</td><td>NA  </td><td> NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>6</td><td>14</td><td>2017-06-14</td><td>OO</td><td>N128SY</td><td>5551</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>2043</td><td>NA  </td><td> NA</td><td>NA</td><td>2113</td><td>NA  </td><td> NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>7</td><td>20</td><td>2017-07-20</td><td>OO</td><td>N120SY</td><td>5386</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>2045</td><td>NA  </td><td> NA</td><td>NA</td><td>2113</td><td>NA  </td><td> NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>7</td><td>21</td><td>2017-07-21</td><td>OO</td><td>N148SY</td><td>5386</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>2045</td><td>NA  </td><td> NA</td><td>NA</td><td>2113</td><td>NA  </td><td> NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>7</td><td>23</td><td>2017-07-23</td><td>OO</td><td>N209SY</td><td>5386</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>2045</td><td>NA  </td><td> NA</td><td>NA</td><td>2113</td><td>NA  </td><td> NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>7</td><td>10</td><td>2017-07-10</td><td>OO</td><td>N140SY</td><td>5386</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>2045</td><td>NA  </td><td> NA</td><td>NA</td><td>2113</td><td>NA  </td><td> NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>7</td><td>12</td><td>2017-07-12</td><td>OO</td><td>N125SY</td><td>5386</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>2045</td><td>NA  </td><td> NA</td><td>NA</td><td>2113</td><td>NA  </td><td> NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>8</td><td> 1</td><td>2017-08-01</td><td>OO</td><td>N120SY</td><td>5386</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>2045</td><td>NA  </td><td> NA</td><td>NA</td><td>2113</td><td>NA  </td><td> NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>8</td><td> 3</td><td>2017-08-03</td><td>OO</td><td>N203SY</td><td>5386</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>2045</td><td>NA  </td><td> NA</td><td>NA</td><td>2113</td><td>NA  </td><td> NA</td><td>NA</td></tr>
	<tr><td>2017</td><td>8</td><td> 4</td><td>2017-08-04</td><td>OO</td><td>N145SY</td><td>5386</td><td>CMH</td><td>Columbus, OH</td><td>ORD</td><td>Chicago, IL</td><td>2040</td><td>NA  </td><td> NA</td><td>NA</td><td>2108</td><td>NA  </td><td> NA</td><td>NA</td></tr>
</tbody>
</table>



So far, we have seen each function in isolation. Now we streamline things a bit so that we only end up with the columns and rows we want to work with, arranged as we want the resulting data-set to be. 


```R
cmhflights %>% 
  select(Month, UniqueCarrier, Dest, DepDelayMinutes) %>% 
  filter(
    Month %in% c(1, 2), UniqueCarrier == "UA", Dest %in% c("CMH", "ORD")
    ) %>% 
  arrange(Month, Dest, -DepDelayMinutes) -> my.df3

my.df3
```


<table class="dataframe">
<caption>A tibble: 251 × 4</caption>
<thead>
	<tr><th scope=col>Month</th><th scope=col>UniqueCarrier</th><th scope=col>Dest</th><th scope=col>DepDelayMinutes</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>178</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td> 61</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td> 44</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td> 39</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td> 39</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td> 30</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td> 27</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td> 18</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td> 12</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td> 10</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  8</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  2</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  2</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  2</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  1</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  1</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  1</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  0</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  0</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  0</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  0</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  0</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  0</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  0</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  0</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  0</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  0</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  0</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  0</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td>  0</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
	<tr><td>2</td><td>UA</td><td>ORD</td><td>0</td></tr>
</tbody>
</table>



Here, the end result is a data frame arranged by Month, then within Month by Destination, and then finally by descending order of flight delays. This is the beauty of `dplyr`, allowing us to chain together various functions to get what we want. How is this helpful? Well, now you have a data frame that you can analyze. What do I want to calculate? Well, let us say we want to create a frequency table, something that we would have done with Excel via a Pivot Table. 

------------

## summarise() or summarize()
What if we need to calculate frequencies? For example, how many flights per month are there? What if we want the __mean__ DepDelay or __median__ ArrDelay? These can be easily calculated as shown below. 

Let us start with frequencies.


```R
cmhflights %>%
  count(Month) # Most flights were in July (n = 4295)
```


<table class="dataframe">
<caption>A tibble: 9 × 2</caption>
<thead>
	<tr><th scope=col>Month</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1</td><td>3757</td></tr>
	<tr><td>2</td><td>3413</td></tr>
	<tr><td>3</td><td>4101</td></tr>
	<tr><td>4</td><td>4123</td></tr>
	<tr><td>5</td><td>4098</td></tr>
	<tr><td>6</td><td>4138</td></tr>
	<tr><td>7</td><td>4295</td></tr>
	<tr><td>8</td><td>4279</td></tr>
	<tr><td>9</td><td>3789</td></tr>
</tbody>
</table>



What about by days of the week AND by month? 


```R
cmhflights %>%
  count(Month, DayOfWeek) # Output is sorted by Month and then DayOfWeek
```


<table class="dataframe">
<caption>A tibble: 63 × 3</caption>
<thead>
	<tr><th scope=col>Month</th><th scope=col>DayOfWeek</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1</td><td>1</td><td>660</td></tr>
	<tr><td>1</td><td>2</td><td>635</td></tr>
	<tr><td>1</td><td>3</td><td>516</td></tr>
	<tr><td>1</td><td>4</td><td>522</td></tr>
	<tr><td>1</td><td>5</td><td>523</td></tr>
	<tr><td>1</td><td>6</td><td>361</td></tr>
	<tr><td>1</td><td>7</td><td>540</td></tr>
	<tr><td>2</td><td>1</td><td>527</td></tr>
	<tr><td>2</td><td>2</td><td>506</td></tr>
	<tr><td>2</td><td>3</td><td>516</td></tr>
	<tr><td>2</td><td>4</td><td>531</td></tr>
	<tr><td>2</td><td>5</td><td>528</td></tr>
	<tr><td>2</td><td>6</td><td>371</td></tr>
	<tr><td>2</td><td>7</td><td>434</td></tr>
	<tr><td>3</td><td>1</td><td>549</td></tr>
	<tr><td>3</td><td>2</td><td>546</td></tr>
	<tr><td>3</td><td>3</td><td>674</td></tr>
	<tr><td>3</td><td>4</td><td>695</td></tr>
	<tr><td>3</td><td>5</td><td>685</td></tr>
	<tr><td>3</td><td>6</td><td>447</td></tr>
	<tr><td>3</td><td>7</td><td>505</td></tr>
	<tr><td>4</td><td>1</td><td>584</td></tr>
	<tr><td>4</td><td>2</td><td>569</td></tr>
	<tr><td>4</td><td>3</td><td>570</td></tr>
	<tr><td>4</td><td>4</td><td>580</td></tr>
	<tr><td>4</td><td>5</td><td>576</td></tr>
	<tr><td>4</td><td>6</td><td>581</td></tr>
	<tr><td>4</td><td>7</td><td>663</td></tr>
	<tr><td>5</td><td>1</td><td>683</td></tr>
	<tr><td>5</td><td>2</td><td>685</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>5</td><td>6</td><td>418</td></tr>
	<tr><td>5</td><td>7</td><td>507</td></tr>
	<tr><td>6</td><td>1</td><td>561</td></tr>
	<tr><td>6</td><td>2</td><td>567</td></tr>
	<tr><td>6</td><td>3</td><td>572</td></tr>
	<tr><td>6</td><td>4</td><td>701</td></tr>
	<tr><td>6</td><td>5</td><td>701</td></tr>
	<tr><td>6</td><td>6</td><td>482</td></tr>
	<tr><td>6</td><td>7</td><td>554</td></tr>
	<tr><td>7</td><td>1</td><td>703</td></tr>
	<tr><td>7</td><td>2</td><td>549</td></tr>
	<tr><td>7</td><td>3</td><td>584</td></tr>
	<tr><td>7</td><td>4</td><td>578</td></tr>
	<tr><td>7</td><td>5</td><td>574</td></tr>
	<tr><td>7</td><td>6</td><td>633</td></tr>
	<tr><td>7</td><td>7</td><td>674</td></tr>
	<tr><td>8</td><td>1</td><td>580</td></tr>
	<tr><td>8</td><td>2</td><td>696</td></tr>
	<tr><td>8</td><td>3</td><td>712</td></tr>
	<tr><td>8</td><td>4</td><td>718</td></tr>
	<tr><td>8</td><td>5</td><td>581</td></tr>
	<tr><td>8</td><td>6</td><td>458</td></tr>
	<tr><td>8</td><td>7</td><td>534</td></tr>
	<tr><td>9</td><td>1</td><td>548</td></tr>
	<tr><td>9</td><td>2</td><td>531</td></tr>
	<tr><td>9</td><td>3</td><td>539</td></tr>
	<tr><td>9</td><td>4</td><td>550</td></tr>
	<tr><td>9</td><td>5</td><td>689</td></tr>
	<tr><td>9</td><td>6</td><td>451</td></tr>
	<tr><td>9</td><td>7</td><td>481</td></tr>
</tbody>
</table>



I want to know the average departure delay and the average arrival delay for all flights, with the averages calculated in two ways -- as the `mean` and as the `median`. 


```R
cmhflights %>%
  summarise(
    mean_arr_delay = mean(ArrDelay, na.rm = TRUE),
    mean_dep_delay = mean(DepDelay, na.rm = TRUE),
    median_arr_delay = median(ArrDelay, na.rm = TRUE),
    median_dep_delay = median(DepDelay, na.rm = TRUE)
    )
```


<table class="dataframe">
<caption>A tibble: 1 × 4</caption>
<thead>
	<tr><th scope=col>mean_arr_delay</th><th scope=col>mean_dep_delay</th><th scope=col>median_arr_delay</th><th scope=col>median_dep_delay</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>3.258327</td><td>9.202072</td><td>-6</td><td>-2</td></tr>
</tbody>
</table>



Here, the `na.rm = TRUE` command is useful because R will not allow you to calculate any mean, median, variance, etc. if the clumn includes some rows with missing data. 

You can see this below, where I have a small data-set called `df` with 4 values of x, but one of the four is missing and recorded as `NA`. See what happens when I try to calculate the mean with/without `na.rm = TRUE`.


```R
df = data.frame(x = c(2, 4, 9, NA))
```


```R
df %>%
  summarise(mean.x = mean(x)) # You get no meaningful values 
```


<table class="dataframe">
<caption>A data.frame: 1 × 1</caption>
<thead>
	<tr><th scope=col>mean.x</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>NA</td></tr>
</tbody>
</table>




```R
df %>%
  summarise(mean.x = mean(x, na.rm = TRUE)) # Now you do get something meaningful
```


<table class="dataframe">
<caption>A data.frame: 1 × 1</caption>
<thead>
	<tr><th scope=col>mean.x</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>5</td></tr>
</tbody>
</table>



--------------

## group_by()
These summaries are fine if you want to calculate aggregate quantities of interest (i.e., means, medians, frequencies, variances, etc. for all rows of data) but what if you wanted to calculate the __number of flights per month, by airline? Average delays by airline?__ 

Now things get interesting because `group_by()` will open up this new world for us! The first thing I will calculate is the number of flights by airline per month. 


```R
cmhflights %>%
  group_by(Month, Carrier) %>%
  tally()
```


<table class="dataframe">
<caption>A grouped_df: 63 × 3</caption>
<thead>
	<tr><th scope=col>Month</th><th scope=col>Carrier</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1</td><td>AA</td><td> 387</td></tr>
	<tr><td>1</td><td>DL</td><td> 549</td></tr>
	<tr><td>1</td><td>EV</td><td> 436</td></tr>
	<tr><td>1</td><td>F9</td><td> 152</td></tr>
	<tr><td>1</td><td>OO</td><td> 123</td></tr>
	<tr><td>1</td><td>UA</td><td> 106</td></tr>
	<tr><td>1</td><td>WN</td><td>2004</td></tr>
	<tr><td>2</td><td>AA</td><td> 360</td></tr>
	<tr><td>2</td><td>DL</td><td> 511</td></tr>
	<tr><td>2</td><td>EV</td><td> 362</td></tr>
	<tr><td>2</td><td>F9</td><td> 136</td></tr>
	<tr><td>2</td><td>OO</td><td> 100</td></tr>
	<tr><td>2</td><td>UA</td><td> 156</td></tr>
	<tr><td>2</td><td>WN</td><td>1788</td></tr>
	<tr><td>3</td><td>AA</td><td> 418</td></tr>
	<tr><td>3</td><td>DL</td><td> 543</td></tr>
	<tr><td>3</td><td>EV</td><td> 408</td></tr>
	<tr><td>3</td><td>F9</td><td> 170</td></tr>
	<tr><td>3</td><td>OO</td><td> 116</td></tr>
	<tr><td>3</td><td>UA</td><td> 193</td></tr>
	<tr><td>3</td><td>WN</td><td>2253</td></tr>
	<tr><td>4</td><td>AA</td><td> 442</td></tr>
	<tr><td>4</td><td>DL</td><td> 554</td></tr>
	<tr><td>4</td><td>EV</td><td> 316</td></tr>
	<tr><td>4</td><td>F9</td><td> 192</td></tr>
	<tr><td>4</td><td>OO</td><td> 288</td></tr>
	<tr><td>4</td><td>UA</td><td> 142</td></tr>
	<tr><td>4</td><td>WN</td><td>2189</td></tr>
	<tr><td>5</td><td>AA</td><td> 442</td></tr>
	<tr><td>5</td><td>DL</td><td> 694</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>5</td><td>UA</td><td> 178</td></tr>
	<tr><td>5</td><td>WN</td><td>2052</td></tr>
	<tr><td>6</td><td>AA</td><td> 485</td></tr>
	<tr><td>6</td><td>DL</td><td> 646</td></tr>
	<tr><td>6</td><td>EV</td><td> 359</td></tr>
	<tr><td>6</td><td>F9</td><td> 180</td></tr>
	<tr><td>6</td><td>OO</td><td> 180</td></tr>
	<tr><td>6</td><td>UA</td><td> 184</td></tr>
	<tr><td>6</td><td>WN</td><td>2104</td></tr>
	<tr><td>7</td><td>AA</td><td> 500</td></tr>
	<tr><td>7</td><td>DL</td><td> 674</td></tr>
	<tr><td>7</td><td>EV</td><td> 251</td></tr>
	<tr><td>7</td><td>F9</td><td> 186</td></tr>
	<tr><td>7</td><td>OO</td><td> 316</td></tr>
	<tr><td>7</td><td>UA</td><td> 194</td></tr>
	<tr><td>7</td><td>WN</td><td>2174</td></tr>
	<tr><td>8</td><td>AA</td><td> 488</td></tr>
	<tr><td>8</td><td>DL</td><td> 666</td></tr>
	<tr><td>8</td><td>EV</td><td> 348</td></tr>
	<tr><td>8</td><td>F9</td><td> 186</td></tr>
	<tr><td>8</td><td>OO</td><td> 336</td></tr>
	<tr><td>8</td><td>UA</td><td> 178</td></tr>
	<tr><td>8</td><td>WN</td><td>2077</td></tr>
	<tr><td>9</td><td>AA</td><td> 369</td></tr>
	<tr><td>9</td><td>DL</td><td> 609</td></tr>
	<tr><td>9</td><td>EV</td><td> 294</td></tr>
	<tr><td>9</td><td>F9</td><td> 180</td></tr>
	<tr><td>9</td><td>OO</td><td> 318</td></tr>
	<tr><td>9</td><td>UA</td><td> 196</td></tr>
	<tr><td>9</td><td>WN</td><td>1823</td></tr>
</tbody>
</table>



You can get the same result if you use `summarize()`


```R
cmhflights %>%
  group_by(Month, Carrier) %>%
  summarize(
    frequency = n()
    )
```

    `summarise()` has grouped output by 'Month'. You can override using the `.groups` argument.
    



<table class="dataframe">
<caption>A grouped_df: 63 × 3</caption>
<thead>
	<tr><th scope=col>Month</th><th scope=col>Carrier</th><th scope=col>frequency</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1</td><td>AA</td><td> 387</td></tr>
	<tr><td>1</td><td>DL</td><td> 549</td></tr>
	<tr><td>1</td><td>EV</td><td> 436</td></tr>
	<tr><td>1</td><td>F9</td><td> 152</td></tr>
	<tr><td>1</td><td>OO</td><td> 123</td></tr>
	<tr><td>1</td><td>UA</td><td> 106</td></tr>
	<tr><td>1</td><td>WN</td><td>2004</td></tr>
	<tr><td>2</td><td>AA</td><td> 360</td></tr>
	<tr><td>2</td><td>DL</td><td> 511</td></tr>
	<tr><td>2</td><td>EV</td><td> 362</td></tr>
	<tr><td>2</td><td>F9</td><td> 136</td></tr>
	<tr><td>2</td><td>OO</td><td> 100</td></tr>
	<tr><td>2</td><td>UA</td><td> 156</td></tr>
	<tr><td>2</td><td>WN</td><td>1788</td></tr>
	<tr><td>3</td><td>AA</td><td> 418</td></tr>
	<tr><td>3</td><td>DL</td><td> 543</td></tr>
	<tr><td>3</td><td>EV</td><td> 408</td></tr>
	<tr><td>3</td><td>F9</td><td> 170</td></tr>
	<tr><td>3</td><td>OO</td><td> 116</td></tr>
	<tr><td>3</td><td>UA</td><td> 193</td></tr>
	<tr><td>3</td><td>WN</td><td>2253</td></tr>
	<tr><td>4</td><td>AA</td><td> 442</td></tr>
	<tr><td>4</td><td>DL</td><td> 554</td></tr>
	<tr><td>4</td><td>EV</td><td> 316</td></tr>
	<tr><td>4</td><td>F9</td><td> 192</td></tr>
	<tr><td>4</td><td>OO</td><td> 288</td></tr>
	<tr><td>4</td><td>UA</td><td> 142</td></tr>
	<tr><td>4</td><td>WN</td><td>2189</td></tr>
	<tr><td>5</td><td>AA</td><td> 442</td></tr>
	<tr><td>5</td><td>DL</td><td> 694</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>5</td><td>UA</td><td> 178</td></tr>
	<tr><td>5</td><td>WN</td><td>2052</td></tr>
	<tr><td>6</td><td>AA</td><td> 485</td></tr>
	<tr><td>6</td><td>DL</td><td> 646</td></tr>
	<tr><td>6</td><td>EV</td><td> 359</td></tr>
	<tr><td>6</td><td>F9</td><td> 180</td></tr>
	<tr><td>6</td><td>OO</td><td> 180</td></tr>
	<tr><td>6</td><td>UA</td><td> 184</td></tr>
	<tr><td>6</td><td>WN</td><td>2104</td></tr>
	<tr><td>7</td><td>AA</td><td> 500</td></tr>
	<tr><td>7</td><td>DL</td><td> 674</td></tr>
	<tr><td>7</td><td>EV</td><td> 251</td></tr>
	<tr><td>7</td><td>F9</td><td> 186</td></tr>
	<tr><td>7</td><td>OO</td><td> 316</td></tr>
	<tr><td>7</td><td>UA</td><td> 194</td></tr>
	<tr><td>7</td><td>WN</td><td>2174</td></tr>
	<tr><td>8</td><td>AA</td><td> 488</td></tr>
	<tr><td>8</td><td>DL</td><td> 666</td></tr>
	<tr><td>8</td><td>EV</td><td> 348</td></tr>
	<tr><td>8</td><td>F9</td><td> 186</td></tr>
	<tr><td>8</td><td>OO</td><td> 336</td></tr>
	<tr><td>8</td><td>UA</td><td> 178</td></tr>
	<tr><td>8</td><td>WN</td><td>2077</td></tr>
	<tr><td>9</td><td>AA</td><td> 369</td></tr>
	<tr><td>9</td><td>DL</td><td> 609</td></tr>
	<tr><td>9</td><td>EV</td><td> 294</td></tr>
	<tr><td>9</td><td>F9</td><td> 180</td></tr>
	<tr><td>9</td><td>OO</td><td> 318</td></tr>
	<tr><td>9</td><td>UA</td><td> 196</td></tr>
	<tr><td>9</td><td>WN</td><td>1823</td></tr>
</tbody>
</table>



Top recap, two ways to do this, first with `tally()` and the second with `summarize(frequency = n())` ... both yielding the same result. Remember `tally()` because it is shorter code. 

Now I want a table that gives us the number of flights per month per airline per destination. 


```R
cmhflights %>%
  group_by(Month, Carrier, Dest) %>%
  tally()
```


<table class="dataframe">
<caption>A grouped_df: 398 × 4</caption>
<thead>
	<tr><th scope=col>Month</th><th scope=col>Carrier</th><th scope=col>Dest</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>193</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>112</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td> 24</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td> 58</td></tr>
	<tr><td>1</td><td>DL</td><td>ATL</td><td>224</td></tr>
	<tr><td>1</td><td>DL</td><td>CMH</td><td>275</td></tr>
	<tr><td>1</td><td>DL</td><td>DTW</td><td>  2</td></tr>
	<tr><td>1</td><td>DL</td><td>LAX</td><td> 27</td></tr>
	<tr><td>1</td><td>DL</td><td>MSP</td><td> 21</td></tr>
	<tr><td>1</td><td>EV</td><td>CMH</td><td>218</td></tr>
	<tr><td>1</td><td>EV</td><td>EWR</td><td> 82</td></tr>
	<tr><td>1</td><td>EV</td><td>IAH</td><td> 72</td></tr>
	<tr><td>1</td><td>EV</td><td>LGA</td><td> 42</td></tr>
	<tr><td>1</td><td>EV</td><td>MSP</td><td> 18</td></tr>
	<tr><td>1</td><td>EV</td><td>ORD</td><td>  4</td></tr>
	<tr><td>1</td><td>F9</td><td>CMH</td><td> 76</td></tr>
	<tr><td>1</td><td>F9</td><td>DEN</td><td> 31</td></tr>
	<tr><td>1</td><td>F9</td><td>LAS</td><td> 31</td></tr>
	<tr><td>1</td><td>F9</td><td>MCO</td><td> 14</td></tr>
	<tr><td>1</td><td>OO</td><td>CMH</td><td> 61</td></tr>
	<tr><td>1</td><td>OO</td><td>DTW</td><td> 56</td></tr>
	<tr><td>1</td><td>OO</td><td>IAH</td><td>  1</td></tr>
	<tr><td>1</td><td>OO</td><td>MSP</td><td>  2</td></tr>
	<tr><td>1</td><td>OO</td><td>ORD</td><td>  3</td></tr>
	<tr><td>1</td><td>UA</td><td>CMH</td><td> 53</td></tr>
	<tr><td>1</td><td>UA</td><td>ORD</td><td> 53</td></tr>
	<tr><td>1</td><td>WN</td><td>ATL</td><td> 84</td></tr>
	<tr><td>1</td><td>WN</td><td>BNA</td><td> 54</td></tr>
	<tr><td>1</td><td>WN</td><td>BOS</td><td> 49</td></tr>
	<tr><td>1</td><td>WN</td><td>BWI</td><td> 90</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>9</td><td>F9</td><td>DEN</td><td> 30</td></tr>
	<tr><td>9</td><td>F9</td><td>LAS</td><td> 30</td></tr>
	<tr><td>9</td><td>F9</td><td>MCO</td><td> 30</td></tr>
	<tr><td>9</td><td>OO</td><td>CMH</td><td>159</td></tr>
	<tr><td>9</td><td>OO</td><td>DEN</td><td>  3</td></tr>
	<tr><td>9</td><td>OO</td><td>DTW</td><td> 65</td></tr>
	<tr><td>9</td><td>OO</td><td>MSP</td><td> 21</td></tr>
	<tr><td>9</td><td>OO</td><td>ORD</td><td> 70</td></tr>
	<tr><td>9</td><td>UA</td><td>CMH</td><td> 98</td></tr>
	<tr><td>9</td><td>UA</td><td>DEN</td><td> 49</td></tr>
	<tr><td>9</td><td>UA</td><td>IAH</td><td>  1</td></tr>
	<tr><td>9</td><td>UA</td><td>ORD</td><td> 48</td></tr>
	<tr><td>9</td><td>WN</td><td>ATL</td><td> 80</td></tr>
	<tr><td>9</td><td>WN</td><td>BNA</td><td> 55</td></tr>
	<tr><td>9</td><td>WN</td><td>BOS</td><td> 54</td></tr>
	<tr><td>9</td><td>WN</td><td>BWI</td><td>109</td></tr>
	<tr><td>9</td><td>WN</td><td>CMH</td><td>912</td></tr>
	<tr><td>9</td><td>WN</td><td>DAL</td><td> 30</td></tr>
	<tr><td>9</td><td>WN</td><td>DCA</td><td> 55</td></tr>
	<tr><td>9</td><td>WN</td><td>DEN</td><td> 32</td></tr>
	<tr><td>9</td><td>WN</td><td>FLL</td><td>  5</td></tr>
	<tr><td>9</td><td>WN</td><td>LAS</td><td> 58</td></tr>
	<tr><td>9</td><td>WN</td><td>MCO</td><td> 65</td></tr>
	<tr><td>9</td><td>WN</td><td>MDW</td><td>187</td></tr>
	<tr><td>9</td><td>WN</td><td>MSY</td><td>  4</td></tr>
	<tr><td>9</td><td>WN</td><td>OAK</td><td> 30</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td> 30</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td> 30</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td> 54</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td> 33</td></tr>
</tbody>
</table>



We could keep enriching the grouping structure. For example, let us add the day of the week to the mix ... 


```R
cmhflights %>%
  group_by(Month, Carrier, Dest, DayOfWeek) %>%
  tally()
```


<table class="dataframe">
<caption>A grouped_df: 2536 × 5</caption>
<thead>
	<tr><th scope=col>Month</th><th scope=col>Carrier</th><th scope=col>Dest</th><th scope=col>DayOfWeek</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>1</td><td>35</td></tr>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>2</td><td>23</td></tr>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>3</td><td>28</td></tr>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>4</td><td>28</td></tr>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>5</td><td>28</td></tr>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>6</td><td>21</td></tr>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>7</td><td>30</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>1</td><td>20</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>2</td><td>16</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>3</td><td>16</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>4</td><td>16</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>5</td><td>16</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>6</td><td> 9</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>7</td><td>19</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>1</td><td> 5</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>2</td><td> 1</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>3</td><td> 4</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>4</td><td> 4</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>5</td><td> 4</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>6</td><td> 1</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>7</td><td> 5</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>1</td><td>10</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>2</td><td> 6</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>3</td><td> 8</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>4</td><td> 8</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>5</td><td> 8</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>6</td><td> 8</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>7</td><td>10</td></tr>
	<tr><td>1</td><td>DL</td><td>ATL</td><td>1</td><td>40</td></tr>
	<tr><td>1</td><td>DL</td><td>ATL</td><td>2</td><td>40</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>9</td><td>WN</td><td>OAK</td><td>6</td><td> 5</td></tr>
	<tr><td>9</td><td>WN</td><td>OAK</td><td>7</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>1</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>2</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>3</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>4</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>5</td><td> 5</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>6</td><td> 5</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>7</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>1</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>2</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>3</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>4</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>5</td><td> 5</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>6</td><td> 5</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>7</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>1</td><td> 8</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>2</td><td> 8</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>3</td><td> 8</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>4</td><td> 8</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>5</td><td>10</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>6</td><td> 5</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>7</td><td> 7</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>1</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>2</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>3</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>4</td><td> 4</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>5</td><td> 5</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>6</td><td> 5</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>7</td><td> 7</td></tr>
</tbody>
</table>



Now say I am really curious about mean departure delays for the preceding grouping structure. That is, what does mean departure delay look like for flights by day of the week, by month, by carrier, and by destination?


```R
cmhflights %>%
  group_by(Month, Carrier, Dest, DayOfWeek) %>%
  summarise(mean_dep_delay = mean(DepDelay, na.rm = TRUE))
```

    `summarise()` has grouped output by 'Month', 'Carrier', 'Dest'. You can override using the `.groups` argument.
    



<table class="dataframe">
<caption>A grouped_df: 2536 × 5</caption>
<thead>
	<tr><th scope=col>Month</th><th scope=col>Carrier</th><th scope=col>Dest</th><th scope=col>DayOfWeek</th><th scope=col>mean_dep_delay</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>1</td><td>16.2857143</td></tr>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>2</td><td> 0.3043478</td></tr>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>3</td><td> 1.1428571</td></tr>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>4</td><td> 3.1071429</td></tr>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>5</td><td> 4.1071429</td></tr>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>6</td><td>24.8571429</td></tr>
	<tr><td>1</td><td>AA</td><td>CMH</td><td>7</td><td>21.8333333</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>1</td><td>25.4000000</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>2</td><td>-2.6250000</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>3</td><td>17.6000000</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>4</td><td>-0.1875000</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>5</td><td> 3.8750000</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>6</td><td>35.6666667</td></tr>
	<tr><td>1</td><td>AA</td><td>DFW</td><td>7</td><td>50.2105263</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>1</td><td> 3.4000000</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>2</td><td>-4.0000000</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>3</td><td>-0.5000000</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>4</td><td> 2.5000000</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>5</td><td>-1.5000000</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>6</td><td>-3.0000000</td></tr>
	<tr><td>1</td><td>AA</td><td>LAX</td><td>7</td><td>-5.8000000</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>1</td><td> 1.7000000</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>2</td><td>-4.6666667</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>3</td><td>-0.1250000</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>4</td><td>-0.7500000</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>5</td><td>-0.8750000</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>6</td><td>-5.6250000</td></tr>
	<tr><td>1</td><td>AA</td><td>PHX</td><td>7</td><td>27.9000000</td></tr>
	<tr><td>1</td><td>DL</td><td>ATL</td><td>1</td><td>29.9459459</td></tr>
	<tr><td>1</td><td>DL</td><td>ATL</td><td>2</td><td> 1.1500000</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>9</td><td>WN</td><td>OAK</td><td>6</td><td> 4.4000000</td></tr>
	<tr><td>9</td><td>WN</td><td>OAK</td><td>7</td><td> 1.7500000</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>1</td><td>22.0000000</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>2</td><td>13.5000000</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>3</td><td>29.5000000</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>4</td><td>16.7500000</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>5</td><td> 8.0000000</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>6</td><td>-1.4000000</td></tr>
	<tr><td>9</td><td>WN</td><td>PHX</td><td>7</td><td>15.7500000</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>1</td><td>-0.3333333</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>2</td><td> 2.3333333</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>3</td><td>-5.3333333</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>4</td><td>16.7500000</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>5</td><td>22.6000000</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>6</td><td>-1.2500000</td></tr>
	<tr><td>9</td><td>WN</td><td>RSW</td><td>7</td><td>-1.6666667</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>1</td><td>20.2500000</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>2</td><td>-3.0000000</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>3</td><td>-3.8750000</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>4</td><td>14.3750000</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>5</td><td>-1.2000000</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>6</td><td>-3.2000000</td></tr>
	<tr><td>9</td><td>WN</td><td>STL</td><td>7</td><td>-5.5714286</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>1</td><td> 5.3333333</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>2</td><td> 7.0000000</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>3</td><td> 2.0000000</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>4</td><td>13.0000000</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>5</td><td>14.8000000</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>6</td><td> 0.0000000</td></tr>
	<tr><td>9</td><td>WN</td><td>TPA</td><td>7</td><td> 6.8000000</td></tr>
</tbody>
</table>



But this is a complicated summary table. What if all I really want to know is what airline has the __highest mean departure delays__, regardless of month or destination or day of the week? This could be done by using `arrange()` to display the result in descending order of _mean_dep_delay_


```R
cmhflights %>%
  group_by(Carrier) %>%
  summarise(mean_dep_delay = mean(DepDelay, na.rm = TRUE)) %>%
  arrange(-mean_dep_delay) # ordered in descending order of delays
```


<table class="dataframe">
<caption>A tibble: 7 × 2</caption>
<thead>
	<tr><th scope=col>Carrier</th><th scope=col>mean_dep_delay</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>EV</td><td>15.207027</td></tr>
	<tr><td>F9</td><td>10.839665</td></tr>
	<tr><td>WN</td><td> 9.581221</td></tr>
	<tr><td>AA</td><td> 7.733938</td></tr>
	<tr><td>DL</td><td> 7.095979</td></tr>
	<tr><td>OO</td><td> 6.438614</td></tr>
	<tr><td>UA</td><td> 6.390420</td></tr>
</tbody>
</table>



EV is Express Jet; F9 is Frontier Airlines; WN is Southwest Airlines; OO is SkyWest Airlines; AA is American Airlines; DL is Delta Airlines; UA is United Airlines. So clearly United Airlines had the lowest average departure delays. Would this still be true if we repeated the calculation by Month? 


```R
cmhflights %>%
  group_by(Carrier, Month) %>%
  summarise(mean_dep_delay = mean(DepDelay, na.rm = TRUE)) %>%
  arrange(mean_dep_delay) # ordered in descending order of delays
```

    `summarise()` has grouped output by 'Carrier'. You can override using the `.groups` argument.
    



<table class="dataframe">
<caption>A grouped_df: 63 × 3</caption>
<thead>
	<tr><th scope=col>Carrier</th><th scope=col>Month</th><th scope=col>mean_dep_delay</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>OO</td><td>8</td><td>-0.8727273</td></tr>
	<tr><td>OO</td><td>2</td><td>-0.5700000</td></tr>
	<tr><td>OO</td><td>9</td><td>-0.1792453</td></tr>
	<tr><td>UA</td><td>9</td><td> 0.6989796</td></tr>
	<tr><td>DL</td><td>8</td><td> 1.8528529</td></tr>
	<tr><td>DL</td><td>2</td><td> 2.2367906</td></tr>
	<tr><td>AA</td><td>2</td><td> 2.3027778</td></tr>
	<tr><td>F9</td><td>9</td><td> 2.5086705</td></tr>
	<tr><td>AA</td><td>9</td><td> 2.6250000</td></tr>
	<tr><td>DL</td><td>3</td><td> 2.9815838</td></tr>
	<tr><td>UA</td><td>4</td><td> 3.1690141</td></tr>
	<tr><td>UA</td><td>8</td><td> 3.5337079</td></tr>
	<tr><td>WN</td><td>2</td><td> 3.6589758</td></tr>
	<tr><td>F9</td><td>2</td><td> 3.8308824</td></tr>
	<tr><td>DL</td><td>9</td><td> 3.9107438</td></tr>
	<tr><td>DL</td><td>5</td><td> 3.9927954</td></tr>
	<tr><td>UA</td><td>5</td><td> 4.2881356</td></tr>
	<tr><td>UA</td><td>1</td><td> 4.5754717</td></tr>
	<tr><td>UA</td><td>3</td><td> 4.8704663</td></tr>
	<tr><td>F9</td><td>4</td><td> 5.6789474</td></tr>
	<tr><td>WN</td><td>3</td><td> 6.0947226</td></tr>
	<tr><td>UA</td><td>2</td><td> 6.2243590</td></tr>
	<tr><td>OO</td><td>4</td><td> 6.4895833</td></tr>
	<tr><td>AA</td><td>5</td><td> 6.5609195</td></tr>
	<tr><td>WN</td><td>9</td><td> 6.6165627</td></tr>
	<tr><td>AA</td><td>8</td><td> 6.8822314</td></tr>
	<tr><td>F9</td><td>5</td><td> 7.4130435</td></tr>
	<tr><td>AA</td><td>4</td><td> 7.7597254</td></tr>
	<tr><td>AA</td><td>3</td><td> 7.8851675</td></tr>
	<tr><td>WN</td><td>1</td><td> 8.1796165</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>WN</td><td>5</td><td>10.30390</td></tr>
	<tr><td>OO</td><td>5</td><td>10.43561</td></tr>
	<tr><td>EV</td><td>8</td><td>10.80317</td></tr>
	<tr><td>OO</td><td>6</td><td>10.93750</td></tr>
	<tr><td>AA</td><td>1</td><td>10.99482</td></tr>
	<tr><td>AA</td><td>6</td><td>11.17537</td></tr>
	<tr><td>OO</td><td>3</td><td>11.21053</td></tr>
	<tr><td>DL</td><td>7</td><td>11.28338</td></tr>
	<tr><td>AA</td><td>7</td><td>11.33671</td></tr>
	<tr><td>F9</td><td>6</td><td>11.98333</td></tr>
	<tr><td>WN</td><td>8</td><td>12.13687</td></tr>
	<tr><td>EV</td><td>2</td><td>12.31858</td></tr>
	<tr><td>OO</td><td>7</td><td>12.73290</td></tr>
	<tr><td>DL</td><td>1</td><td>13.04833</td></tr>
	<tr><td>OO</td><td>1</td><td>13.59350</td></tr>
	<tr><td>EV</td><td>5</td><td>14.05495</td></tr>
	<tr><td>UA</td><td>6</td><td>14.09239</td></tr>
	<tr><td>F9</td><td>8</td><td>14.10870</td></tr>
	<tr><td>WN</td><td>6</td><td>14.18984</td></tr>
	<tr><td>WN</td><td>7</td><td>14.19621</td></tr>
	<tr><td>UA</td><td>7</td><td>14.45312</td></tr>
	<tr><td>F9</td><td>3</td><td>14.67059</td></tr>
	<tr><td>F9</td><td>7</td><td>14.96774</td></tr>
	<tr><td>EV</td><td>6</td><td>15.17094</td></tr>
	<tr><td>EV</td><td>3</td><td>16.45845</td></tr>
	<tr><td>EV</td><td>1</td><td>16.86744</td></tr>
	<tr><td>DL</td><td>4</td><td>17.72500</td></tr>
	<tr><td>EV</td><td>4</td><td>17.77199</td></tr>
	<tr><td>F9</td><td>1</td><td>22.70000</td></tr>
	<tr><td>EV</td><td>7</td><td>25.47034</td></tr>
</tbody>
</table>



All righty then! Looks like three of the lowest mean departure delays were for SkyWest. Do not let the negative numbers throw you for a loop; a negative value implies the flight left earlier than scheduled. 

So far so good. But now I am curious about what percent of flights operated by AA, DL, UA, and WN were delayed. How could I calculate this? 

(1) I need to use `filter()` to restrict the data-set to just these four airlines.  
(2) Then I need to generate a new column that identifies whether a flight was delayed or not (`late`).  
(3) Now we can calculate the total number of flights (`nflights`) and the total number of flights that were delayed (`nlate`).  
(4) If I then calculate $\left( \dfrac{nlate}{nflights} \right)\times 100$ we will end up with the percent of flights that were delayed.  


```R
cmhflights %>%
  select(c(Carrier, DepDelay)) %>%
  filter(
      Carrier %in% c("AA", "DL", "UA", "WN")
  ) %>%
  mutate(
      late = case_when(
          DepDelay > 0 ~ "Yes",
          DepDelay <= 0 ~ "No"
      )
  ) %>% 
  group_by(Carrier) %>%
  mutate(
      nflights = n()
  ) %>%
  group_by(Carrier, late) %>%
  mutate(
    nlate = n(),
    pct_late = (nlate / nflights) * 100
  ) -> df1

df1
```


<table class="dataframe">
<caption>A grouped_df: 29328 × 6</caption>
<thead>
	<tr><th scope=col>Carrier</th><th scope=col>DepDelay</th><th scope=col>late</th><th scope=col>nflights</th><th scope=col>nlate</th><th scope=col>pct_late</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>AA</td><td> -9</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> 24</td><td>Yes</td><td>3891</td><td>1162</td><td>29.86379</td></tr>
	<tr><td>AA</td><td> -6</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> -5</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> -7</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> 22</td><td>Yes</td><td>3891</td><td>1162</td><td>29.86379</td></tr>
	<tr><td>AA</td><td> -4</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> -4</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> 42</td><td>Yes</td><td>3891</td><td>1162</td><td>29.86379</td></tr>
	<tr><td>AA</td><td> -6</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> -5</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> -3</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td>106</td><td>Yes</td><td>3891</td><td>1162</td><td>29.86379</td></tr>
	<tr><td>AA</td><td>  4</td><td>Yes</td><td>3891</td><td>1162</td><td>29.86379</td></tr>
	<tr><td>AA</td><td>  0</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td>  6</td><td>Yes</td><td>3891</td><td>1162</td><td>29.86379</td></tr>
	<tr><td>AA</td><td>  7</td><td>Yes</td><td>3891</td><td>1162</td><td>29.86379</td></tr>
	<tr><td>AA</td><td> 22</td><td>Yes</td><td>3891</td><td>1162</td><td>29.86379</td></tr>
	<tr><td>AA</td><td> 31</td><td>Yes</td><td>3891</td><td>1162</td><td>29.86379</td></tr>
	<tr><td>AA</td><td> -8</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> -5</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> -3</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> 32</td><td>Yes</td><td>3891</td><td>1162</td><td>29.86379</td></tr>
	<tr><td>AA</td><td> -3</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> -3</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> 61</td><td>Yes</td><td>3891</td><td>1162</td><td>29.86379</td></tr>
	<tr><td>AA</td><td> -4</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td>  8</td><td>Yes</td><td>3891</td><td>1162</td><td>29.86379</td></tr>
	<tr><td>AA</td><td>  0</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>AA</td><td> -3</td><td>No </td><td>3891</td><td>2698</td><td>69.33950</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>DL</td><td>-2</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>87</td><td>Yes</td><td>5446</td><td>1510</td><td>27.72677</td></tr>
	<tr><td>DL</td><td>-6</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>36</td><td>Yes</td><td>5446</td><td>1510</td><td>27.72677</td></tr>
	<tr><td>DL</td><td>-3</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-5</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td> 6</td><td>Yes</td><td>5446</td><td>1510</td><td>27.72677</td></tr>
	<tr><td>DL</td><td>-7</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-6</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-8</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-2</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-3</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-3</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-6</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-4</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-3</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-3</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-3</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-4</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td> 9</td><td>Yes</td><td>5446</td><td>1510</td><td>27.72677</td></tr>
	<tr><td>DL</td><td>-1</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-5</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-3</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-2</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-3</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-2</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-6</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td> 1</td><td>Yes</td><td>5446</td><td>1510</td><td>27.72677</td></tr>
	<tr><td>DL</td><td>-5</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
	<tr><td>DL</td><td>-4</td><td>No </td><td>5446</td><td>3887</td><td>71.37349</td></tr>
</tbody>
</table>



There is a whole lot going on here so let us break it down. 

`filter(Carrier %in% c("AA", "DL", "UA", "WN"))` is keeping specified airlines' data while dropping the rest

`mutate(late = case_when(...)` is creating a new column called `late` and storing a value of "Yes" if `DepDelay > 0` (i.e., the flight was delayed by 1 or more minutes) and "No" if `DepDelay <= 0` (i.e., if the flight departed on time or earlier than the scheduled departure time) 

`group_by(Carrier)` is grouping by Carrier and then counting with `mutate(nflights = n())` how many flights there were per Carrier and storing this sum in a new column called `nflights`

`group_by(Carrier, late)` then regroups the data, this time by Carrier and if the flight was late or not


`mutate(nlate = n(), pct_late = (nlate / nflights) * 100)` is then creating two new columns, `nlate` -- the number of flights per late values of "Yes" and "No", respectively, and then `pct_late` -- the percent of flights per carrier that were late.  

Now, we only want the flights that were late so let us apply `select()` to keep just a few columns and then we use `filter()` to keep only rows corresponding to `late = "Yes"`. 

This will still leave us with duplicate rows but we can drop these duplicate rows via a new command, `distinct()`  -- which if left empty inside the parentheses `()` looks for all unique rows of data (each row ends up with a unique combination of all columns' values). If you want unique values of specific columns then those column names can be inserted inside the parentheses `()`


```R
df1 %>%
  filter(late == "Yes") %>%
  select(Carrier, pct_late) %>%
  distinct() %>%
  arrange(pct_late)
```

    Adding missing grouping variables: `late`
    



<table class="dataframe">
<caption>A grouped_df: 4 × 3</caption>
<thead>
	<tr><th scope=col>late</th><th scope=col>Carrier</th><th scope=col>pct_late</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Yes</td><td>UA</td><td>23.96857</td></tr>
	<tr><td>Yes</td><td>DL</td><td>27.72677</td></tr>
	<tr><td>Yes</td><td>AA</td><td>29.86379</td></tr>
	<tr><td>Yes</td><td>WN</td><td>41.06911</td></tr>
</tbody>
</table>



So! 24% of UA flights were late, the lowest in this group. 

What if we wanted to do this for all airlines, and we want the calculations to be done by Month as well?


```R
cmhflights %>%
  select(c(Carrier, Month, DepDelay)) %>%
  filter(Carrier %in% c("AA", "DL", "UA", "WN")) %>%
  mutate(late = case_when(
    DepDelay > 0 ~ "Yes",
    DepDelay <= 0 ~ "No"
      )
    ) %>% 
  group_by(Carrier, Month) %>%
  mutate(nflights = n()) %>%
  group_by(Carrier, Month, late) %>%
  mutate(
    nlate = n(),
    pct_late = (nlate / nflights) * 100) %>%
  filter(late == "Yes") %>%
  select(Carrier, Month, pct_late) %>%
  distinct() %>%
  arrange(pct_late)
```

    Adding missing grouping variables: `late`
    



<table class="dataframe">
<caption>A grouped_df: 36 × 4</caption>
<thead>
	<tr><th scope=col>late</th><th scope=col>Carrier</th><th scope=col>Month</th><th scope=col>pct_late</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Yes</td><td>UA</td><td>9</td><td>17.85714</td></tr>
	<tr><td>Yes</td><td>UA</td><td>2</td><td>18.58974</td></tr>
	<tr><td>Yes</td><td>DL</td><td>9</td><td>20.19704</td></tr>
	<tr><td>Yes</td><td>UA</td><td>5</td><td>20.22472</td></tr>
	<tr><td>Yes</td><td>UA</td><td>4</td><td>20.42254</td></tr>
	<tr><td>Yes</td><td>AA</td><td>9</td><td>21.40921</td></tr>
	<tr><td>Yes</td><td>DL</td><td>2</td><td>21.52642</td></tr>
	<tr><td>Yes</td><td>DL</td><td>8</td><td>22.52252</td></tr>
	<tr><td>Yes</td><td>AA</td><td>2</td><td>23.05556</td></tr>
	<tr><td>Yes</td><td>UA</td><td>3</td><td>23.31606</td></tr>
	<tr><td>Yes</td><td>UA</td><td>8</td><td>24.15730</td></tr>
	<tr><td>Yes</td><td>DL</td><td>3</td><td>25.23020</td></tr>
	<tr><td>Yes</td><td>AA</td><td>8</td><td>26.63934</td></tr>
	<tr><td>Yes</td><td>AA</td><td>4</td><td>28.28054</td></tr>
	<tr><td>Yes</td><td>WN</td><td>2</td><td>29.13870</td></tr>
	<tr><td>Yes</td><td>UA</td><td>6</td><td>29.34783</td></tr>
	<tr><td>Yes</td><td>UA</td><td>1</td><td>30.18868</td></tr>
	<tr><td>Yes</td><td>DL</td><td>4</td><td>30.32491</td></tr>
	<tr><td>Yes</td><td>DL</td><td>1</td><td>30.41894</td></tr>
	<tr><td>Yes</td><td>AA</td><td>3</td><td>30.86124</td></tr>
	<tr><td>Yes</td><td>DL</td><td>5</td><td>30.97983</td></tr>
	<tr><td>Yes</td><td>AA</td><td>5</td><td>32.35294</td></tr>
	<tr><td>Yes</td><td>UA</td><td>7</td><td>32.47423</td></tr>
	<tr><td>Yes</td><td>DL</td><td>6</td><td>32.97214</td></tr>
	<tr><td>Yes</td><td>AA</td><td>6</td><td>33.60825</td></tr>
	<tr><td>Yes</td><td>DL</td><td>7</td><td>33.67953</td></tr>
	<tr><td>Yes</td><td>WN</td><td>1</td><td>34.03194</td></tr>
	<tr><td>Yes</td><td>AA</td><td>1</td><td>34.36693</td></tr>
	<tr><td>Yes</td><td>AA</td><td>7</td><td>35.40000</td></tr>
	<tr><td>Yes</td><td>WN</td><td>9</td><td>36.20406</td></tr>
	<tr><td>Yes</td><td>WN</td><td>4</td><td>36.68342</td></tr>
	<tr><td>Yes</td><td>WN</td><td>3</td><td>38.39325</td></tr>
	<tr><td>Yes</td><td>WN</td><td>5</td><td>44.20078</td></tr>
	<tr><td>Yes</td><td>WN</td><td>8</td><td>48.96485</td></tr>
	<tr><td>Yes</td><td>WN</td><td>6</td><td>49.66730</td></tr>
	<tr><td>Yes</td><td>WN</td><td>7</td><td>49.81601</td></tr>
</tbody>
</table>



Before we move on, I want to point out something about `case_when()`. Specifically, we used it to create a new column called `late` from numeric values found in `DepDelay`. But what if we wanted to create a new column from a column that had categorical variables in it, like `Dest` or `Carrier`? Easy.


```R
cmhflights %>%
    mutate(
      carrier_name = case_when(
          Carrier == "AA" ~ "American Airlines",
          Carrier == "DL" ~ "Delta Airlines",
          Carrier == "UA" ~ "United Airlines",
          Carrier == "EV" ~ "Express Jet",
          Carrier == "F9" ~ "Frontier Airlines",
          Carrier == "WN" ~ "Southwest Airlines",
          Carrier == "OO" ~ "SkyWest Airlines"
      )
  ) %>%
    select(Carrier, carrier_name) %>%
    group_by(Carrier, carrier_name) %>%
    tally()
```


<table class="dataframe">
<caption>A grouped_df: 7 × 3</caption>
<thead>
	<tr><th scope=col>Carrier</th><th scope=col>carrier_name</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>AA</td><td>American Airlines </td><td> 3891</td></tr>
	<tr><td>DL</td><td>Delta Airlines    </td><td> 5446</td></tr>
	<tr><td>EV</td><td>Express Jet       </td><td> 3056</td></tr>
	<tr><td>F9</td><td>Frontier Airlines </td><td> 1568</td></tr>
	<tr><td>OO</td><td>SkyWest Airlines  </td><td> 2041</td></tr>
	<tr><td>UA</td><td>United Airlines   </td><td> 1527</td></tr>
	<tr><td>WN</td><td>Southwest Airlines</td><td>18464</td></tr>
</tbody>
</table>



Second, `case_when()` includes an option that cuts down on our work. In particular, say I want to create a new column and label its values as "Weekend" if the DayOfWeek is Saturday or Sunday and "Weekday" if DayOfWeek is any other day. In doing this, it would serve us well to remember that the week begins on Sunday so DayOfWeek == 1 is Sunday, not Monday. 


```R
cmhflights %>%
  mutate(weekend = case_when(
    DayOfWeek %in% c(7, 1) ~ "Yes",
    TRUE ~ "No"
        )
    ) %>%
  select(DayOfWeek, weekend) %>%
  distinct()
```


<table class="dataframe">
<caption>A tibble: 7 × 2</caption>
<thead>
	<tr><th scope=col>DayOfWeek</th><th scope=col>weekend</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>7</td><td>Yes</td></tr>
	<tr><td>1</td><td>Yes</td></tr>
	<tr><td>2</td><td>No </td></tr>
	<tr><td>3</td><td>No </td></tr>
	<tr><td>4</td><td>No </td></tr>
	<tr><td>5</td><td>No </td></tr>
	<tr><td>6</td><td>No </td></tr>
</tbody>
</table>



Notice how `TRUE` swept up all other values of `DayOfWeek` and coded them as "No."  

One final showcasing of `case_when()`. In **Module 01** we looked at the `hsb2` data and created some `factors` for columns such as female, ses, schtyp, and so on. Well, let us see how the same thing could be done with `case_when()`.


```R
read.table(
  'https://stats.idre.ucla.edu/stat/data/hsb2.csv',
  header = TRUE,
  sep = ","
  ) -> hsb2
```


```R
hsb2 %>%
  mutate(
    female.f = case_when(
      female == 0 ~ "Male",
      female == 1 ~ "Female"),
    race.f = case_when(
      race == 1 ~ "Hispanic",
      race == 2 ~ "Asian",
      race == 3 ~ "African-American",
      TRUE ~ "White"),
    ses.f = case_when(
      ses == 1 ~ "Low",
      ses == 2 ~ "Medium",
      TRUE ~ "High"),
    schtyp.f = case_when(
      schtyp == 1 ~ "Public",
      TRUE ~ "Private"),
    prog.f = case_when(
      prog == 1 ~ "General",
      prog == 2 ~ "Academic",
      TRUE ~ "Vocational")
    ) -> hsb2
```

-----------

## Some other `dplyr()` commands
We have seen `count()` in action but let us see it again, in a slightly different light. In particular, say I want to know how many unique destinations are there connected by air from Columbus. 

### count()


```R
cmhflights %>%
  filter(Origin == "CMH") %>%
  count(Dest, sort = TRUE)
```


<table class="dataframe">
<caption>A tibble: 26 × 2</caption>
<thead>
	<tr><th scope=col>Dest</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>ATL</td><td>2884</td></tr>
	<tr><td>MDW</td><td>1511</td></tr>
	<tr><td>MCO</td><td>1148</td></tr>
	<tr><td>DFW</td><td>1122</td></tr>
	<tr><td>DEN</td><td> 971</td></tr>
	<tr><td>BWI</td><td> 948</td></tr>
	<tr><td>LAS</td><td> 815</td></tr>
	<tr><td>PHX</td><td> 815</td></tr>
	<tr><td>ORD</td><td> 803</td></tr>
	<tr><td>EWR</td><td> 736</td></tr>
	<tr><td>MSP</td><td> 677</td></tr>
	<tr><td>TPA</td><td> 595</td></tr>
	<tr><td>BNA</td><td> 520</td></tr>
	<tr><td>RSW</td><td> 509</td></tr>
	<tr><td>DCA</td><td> 507</td></tr>
	<tr><td>STL</td><td> 495</td></tr>
	<tr><td>DTW</td><td> 485</td></tr>
	<tr><td>LAX</td><td> 483</td></tr>
	<tr><td>BOS</td><td> 475</td></tr>
	<tr><td>IAH</td><td> 460</td></tr>
	<tr><td>DAL</td><td> 273</td></tr>
	<tr><td>OAK</td><td> 273</td></tr>
	<tr><td>FLL</td><td> 240</td></tr>
	<tr><td>LGA</td><td> 173</td></tr>
	<tr><td>PHL</td><td>  57</td></tr>
	<tr><td>MSY</td><td>  22</td></tr>
</tbody>
</table>



Note: There is no need for `group_by()` here. And `sort = TRUE` arranges the result in descending order of the frequency (`n`). Here is another code example, this time adding Carrier to the mix. 


```R
cmhflights %>%
  filter(Origin == "CMH") %>%
  count(Carrier, Dest, sort = TRUE)
```


<table class="dataframe">
<caption>A tibble: 45 × 3</caption>
<thead>
	<tr><th scope=col>Carrier</th><th scope=col>Dest</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>DL</td><td>ATL</td><td>2141</td></tr>
	<tr><td>WN</td><td>MDW</td><td>1511</td></tr>
	<tr><td>AA</td><td>DFW</td><td>1122</td></tr>
	<tr><td>WN</td><td>BWI</td><td> 948</td></tr>
	<tr><td>WN</td><td>MCO</td><td> 929</td></tr>
	<tr><td>WN</td><td>ATL</td><td> 743</td></tr>
	<tr><td>EV</td><td>EWR</td><td> 736</td></tr>
	<tr><td>WN</td><td>TPA</td><td> 595</td></tr>
	<tr><td>UA</td><td>ORD</td><td> 577</td></tr>
	<tr><td>WN</td><td>LAS</td><td> 542</td></tr>
	<tr><td>AA</td><td>PHX</td><td> 526</td></tr>
	<tr><td>WN</td><td>BNA</td><td> 520</td></tr>
	<tr><td>WN</td><td>DCA</td><td> 507</td></tr>
	<tr><td>WN</td><td>STL</td><td> 495</td></tr>
	<tr><td>WN</td><td>RSW</td><td> 486</td></tr>
	<tr><td>WN</td><td>BOS</td><td> 475</td></tr>
	<tr><td>EV</td><td>IAH</td><td> 458</td></tr>
	<tr><td>OO</td><td>DTW</td><td> 428</td></tr>
	<tr><td>WN</td><td>DEN</td><td> 384</td></tr>
	<tr><td>DL</td><td>MSP</td><td> 336</td></tr>
	<tr><td>WN</td><td>PHX</td><td> 289</td></tr>
	<tr><td>F9</td><td>DEN</td><td> 273</td></tr>
	<tr><td>F9</td><td>LAS</td><td> 273</td></tr>
	<tr><td>WN</td><td>DAL</td><td> 273</td></tr>
	<tr><td>WN</td><td>OAK</td><td> 273</td></tr>
	<tr><td>OO</td><td>MSP</td><td> 264</td></tr>
	<tr><td>DL</td><td>LAX</td><td> 242</td></tr>
	<tr><td>AA</td><td>LAX</td><td> 241</td></tr>
	<tr><td>WN</td><td>FLL</td><td> 240</td></tr>
	<tr><td>F9</td><td>MCO</td><td> 217</td></tr>
	<tr><td>OO</td><td>ORD</td><td> 199</td></tr>
	<tr><td>UA</td><td>DEN</td><td> 185</td></tr>
	<tr><td>EV</td><td>LGA</td><td> 173</td></tr>
	<tr><td>OO</td><td>DEN</td><td> 129</td></tr>
	<tr><td>EV</td><td>MSP</td><td>  77</td></tr>
	<tr><td>AA</td><td>PHL</td><td>  57</td></tr>
	<tr><td>EV</td><td>DTW</td><td>  55</td></tr>
	<tr><td>EV</td><td>ORD</td><td>  27</td></tr>
	<tr><td>WN</td><td>MSY</td><td>  22</td></tr>
	<tr><td>F9</td><td>RSW</td><td>  21</td></tr>
	<tr><td>DL</td><td>DTW</td><td>   2</td></tr>
	<tr><td>DL</td><td>MCO</td><td>   2</td></tr>
	<tr><td>EV</td><td>RSW</td><td>   2</td></tr>
	<tr><td>OO</td><td>IAH</td><td>   1</td></tr>
	<tr><td>UA</td><td>IAH</td><td>   1</td></tr>
</tbody>
</table>



How does this help us? Well, now we know that if we were flying to Atlanta, Delta would have the most flights, but if we were flying to the Chicago area then Southwest should be our pick.  

### n_distinct() 
Another useful command is `n_distinct()`, useful in the sense of allowing us to calculate the the number of distinct values of any column. For example, say I want to know how many unique aircraft (not airlines) are there in this data-set. 


```R
cmhflights %>%
  summarise(n_distinct(TailNum))
```


<table class="dataframe">
<caption>A tibble: 1 × 1</caption>
<thead>
	<tr><th scope=col>n_distinct(TailNum)</th></tr>
	<tr><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2248</td></tr>
</tbody>
</table>



### top_n()
If you want to see the top 'n' number of observations, for example the 4 airlines with the most aircraft, you can lean on `top_n()`, as shown below.


```R
cmhflights %>%
  group_by(Carrier) %>%
  summarise(num.flights = n_distinct(TailNum)) %>%
  arrange(-num.flights) %>% 
  top_n(4)
```

    Selecting by num.flights
    



<table class="dataframe">
<caption>A tibble: 4 × 2</caption>
<thead>
	<tr><th scope=col>Carrier</th><th scope=col>num.flights</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>WN</td><td>751</td></tr>
	<tr><td>DL</td><td>539</td></tr>
	<tr><td>UA</td><td>289</td></tr>
	<tr><td>OO</td><td>222</td></tr>
</tbody>
</table>



I am also curious about which aircraft has flown the most, and then maybe 9 other aircraft that follow in descending order.


```R
cmhflights %>%
  filter(!is.na(TailNum)) %>% # Removing some missing cases 
  group_by(TailNum) %>%
  tally() %>% 
  arrange(-n) %>%
  top_n(4)
```

    Selecting by n
    



<table class="dataframe">
<caption>A tibble: 4 × 2</caption>
<thead>
	<tr><th scope=col>TailNum</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>N396SW</td><td>74</td></tr>
	<tr><td>N601WN</td><td>66</td></tr>
	<tr><td>N646SW</td><td>64</td></tr>
	<tr><td>N635SW</td><td>62</td></tr>
</tbody>
</table>



### join()
You will, from time to time, need to merge multiple data-sets together. For example, say I have the following data-sets I have created for demonstration purposes. 


```R
tibble(
  Name = c("Tim", "Tammy", "Bubbles", "Panda"),
  Score = c(5, 8, 9, 10)
    ) -> df1
```


```R
tibble(
  Name = c("Tim", "Tammy", "Bubbles"),
  Age = c(25, 78, 19)
    ) -> df2
```


```R
tibble(
  Name = c("Tim", "Tammy", "Panda"),
  Education = c("BA", "PhD", "JD")
    ) -> df3
```


```R
df1; df2; df3
```


<table class="dataframe">
<caption>A tibble: 4 × 2</caption>
<thead>
	<tr><th scope=col>Name</th><th scope=col>Score</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Tim    </td><td> 5</td></tr>
	<tr><td>Tammy  </td><td> 8</td></tr>
	<tr><td>Bubbles</td><td> 9</td></tr>
	<tr><td>Panda  </td><td>10</td></tr>
</tbody>
</table>




<table class="dataframe">
<caption>A tibble: 3 × 2</caption>
<thead>
	<tr><th scope=col>Name</th><th scope=col>Age</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Tim    </td><td>25</td></tr>
	<tr><td>Tammy  </td><td>78</td></tr>
	<tr><td>Bubbles</td><td>19</td></tr>
</tbody>
</table>




<table class="dataframe">
<caption>A tibble: 3 × 2</caption>
<thead>
	<tr><th scope=col>Name</th><th scope=col>Education</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Tim  </td><td>BA </td></tr>
	<tr><td>Tammy</td><td>PhD</td></tr>
	<tr><td>Panda</td><td>JD </td></tr>
</tbody>
</table>



Notice that Panda is absent from `df2` and Bubbles is absent from `df3`. So if we wanted to build ONE data-set with all data for Tim, Tammy, Bubbles, and Panda, some of the information would be missing for some of these folks. But how could we construct ONE data-set? Via one of a few `join()` commands. 

#### full_join() 
Let us start with a simple full_join, where we link up every individual in df1 or df2 or df3 **regardless of whether they are seen in both data-sets**. 


```R
df1 %>%
  full_join(df2, by = "Name") %>%
  full_join(df3, by = "Name") 
```


<table class="dataframe">
<caption>A tibble: 4 × 4</caption>
<thead>
	<tr><th scope=col>Name</th><th scope=col>Score</th><th scope=col>Age</th><th scope=col>Education</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Tim    </td><td> 5</td><td>25</td><td>BA </td></tr>
	<tr><td>Tammy  </td><td> 8</td><td>78</td><td>PhD</td></tr>
	<tr><td>Bubbles</td><td> 9</td><td>19</td><td>NA </td></tr>
	<tr><td>Panda  </td><td>10</td><td>NA</td><td>JD </td></tr>
</tbody>
</table>



Pay attention to two things: (i) Name connects the records in each data-set, and so it must be spelled exactly the same for a link to be made, and (ii) the `full_join()` links up all individuals regardless of whether they are missing any information in any of the data-sets. This is usually how most folks will link up multiple files unless they only want records found in a master file. For example, say I want to link up df2 and df3 but only such that the final result will include all records found in BOTH df2 and df3, with df2 serving as the master data-set. Eh?


```R
df2 %>%
  left_join(df3, by = "Name")  
```


<table class="dataframe">
<caption>A tibble: 3 × 3</caption>
<thead>
	<tr><th scope=col>Name</th><th scope=col>Age</th><th scope=col>Education</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Tim    </td><td>25</td><td>BA </td></tr>
	<tr><td>Tammy  </td><td>78</td><td>PhD</td></tr>
	<tr><td>Bubbles</td><td>19</td><td>NA </td></tr>
</tbody>
</table>



Notice that Panda is dropped because it is not found in df2. 

Maybe you want df3 to be the master file, in which case you would see a different result (with Bubbles not seen in the result since Bubbles is found in df2 but not in df3): 


```R
df3 %>%
  left_join(df2, by = "Name")  
```


<table class="dataframe">
<caption>A tibble: 3 × 3</caption>
<thead>
	<tr><th scope=col>Name</th><th scope=col>Education</th><th scope=col>Age</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Tim  </td><td>BA </td><td>25</td></tr>
	<tr><td>Tammy</td><td>PhD</td><td>78</td></tr>
	<tr><td>Panda</td><td>JD </td><td>NA</td></tr>
</tbody>
</table>



Rarely, but definitely not "never," you may want to see the records that are not found in both. Here, anti_join() comes in handy, thus:


```R
df2 %>%
  anti_join(df3, by = "Name")
```


<table class="dataframe">
<caption>A tibble: 1 × 2</caption>
<thead>
	<tr><th scope=col>Name</th><th scope=col>Age</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Bubbles</td><td>19</td></tr>
</tbody>
</table>




```R
df3 %>%
  anti_join(df2, by = "Name")
```


<table class="dataframe">
<caption>A tibble: 1 × 2</caption>
<thead>
	<tr><th scope=col>Name</th><th scope=col>Education</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Panda</td><td>JD</td></tr>
</tbody>
</table>



--------------------

## Two other useful commands 
### {santoku}
Every now and then you may want to or need to create a grouped version of some numeric variable. For example, we have DepDelay for all flights but want to group this into `quartiles`. How can we do that? In many ways but the easiest might be to use a specific library -- `{santoku}`. Say, for example, I want to create 4 groups of `dep_delay`, and I want these such that we are grouping `DepDelay` into the bottom 25%, next 25%, the next 25%, and finally the highest 25%. Wait, these are the `quartiles`! Fair enough, but how can I do this? 


```R
library(santoku)

cmhflights %>%
  mutate(
    depdelay_groups = chop_equally(DepDelay, groups = 4)
      ) %>%
  group_by(depdelay_groups) %>%
  tally()
```

    
    Attaching package: ‘santoku’
    
    
    The following object is masked from ‘package:tidyr’:
    
        chop
    
    



<table class="dataframe">
<caption>A tibble: 5 × 2</caption>
<thead>
	<tr><th scope=col>depdelay_groups</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>[0%, 25%)  </td><td> 6887</td></tr>
	<tr><td>[25%, 50%) </td><td> 9267</td></tr>
	<tr><td>[50%, 75%) </td><td>10143</td></tr>
	<tr><td>[75%, 100%]</td><td> 9225</td></tr>
	<tr><td>NA         </td><td>  471</td></tr>
</tbody>
</table>



What if we wanted to slice up DepDelay in specific intervals, first at 0, then at 15, then at 30, and then at 45? 


```R
cmhflights %>%
  mutate(
    depdelay_groups = chop(DepDelay, breaks = c(0, 15, 30, 45))
      ) %>%
  group_by(depdelay_groups) %>%
  tally()
```


<table class="dataframe">
<caption>A tibble: 6 × 2</caption>
<thead>
	<tr><th scope=col>depdelay_groups</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>[-27, 0)  </td><td>21108</td></tr>
	<tr><td>[0, 15)   </td><td> 7883</td></tr>
	<tr><td>[15, 30)  </td><td> 2567</td></tr>
	<tr><td>[30, 45)  </td><td> 1240</td></tr>
	<tr><td>[45, 1323]</td><td> 2724</td></tr>
	<tr><td>NA        </td><td>  471</td></tr>
</tbody>
</table>



We could also create quintiles (5 groups) or deciles (10 groups) as shown below: 


```R
cmhflights %>%
  filter(!is.na(DepDelay)) %>%
  mutate(
    depdelay_groups = chop_quantiles(
      DepDelay, c(0.2, 0.4, 0.6, 0.8)
    )
  ) %>%
  group_by(depdelay_groups) %>%
  tally()
```


<table class="dataframe">
<caption>A tibble: 5 × 2</caption>
<thead>
	<tr><th scope=col>depdelay_groups</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>[0%, 20%)  </td><td>6887</td></tr>
	<tr><td>[20%, 40%) </td><td>6342</td></tr>
	<tr><td>[40%, 60%) </td><td>7879</td></tr>
	<tr><td>[60%, 80%] </td><td>7362</td></tr>
	<tr><td>(80%, 100%]</td><td>7052</td></tr>
</tbody>
</table>




```R
cmhflights %>%
  filter(!is.na(DepDelay)) %>%
  mutate(
    depdelay_groups = chop_quantiles(
      DepDelay, seq(0.1, 0.9, by = 0.1)
    )
  ) %>%
  group_by(depdelay_groups) %>%
  tally()
```


<table class="dataframe">
<caption>A tibble: 10 × 2</caption>
<thead>
	<tr><th scope=col>depdelay_groups</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>[0%, 10%)  </td><td>3006</td></tr>
	<tr><td>[10%, 20%) </td><td>3881</td></tr>
	<tr><td>[20%, 30%) </td><td>3344</td></tr>
	<tr><td>[30%, 40%) </td><td>2998</td></tr>
	<tr><td>[40%, 50%) </td><td>2925</td></tr>
	<tr><td>[50%, 60%) </td><td>4954</td></tr>
	<tr><td>[60%, 70%) </td><td>3262</td></tr>
	<tr><td>[70%, 80%) </td><td>3773</td></tr>
	<tr><td>[80%, 90%] </td><td>3904</td></tr>
	<tr><td>(90%, 100%]</td><td>3475</td></tr>
</tbody>
</table>



### ordered()
more often than we would like to see happen, we end up with categorical variables that should follow a certain order but do not. For example, say you have survey data where people were asked to respond whether they Agree, are Neutral, or Disagree with some statement. Let us also assume that the frequencies are as follows:


```R
tibble(
  response = c(
      rep("Agree", 25), 
      rep("Neutral", 30), 
      rep("Disagree", 45)
      )
    ) -> mydf
```


```R
mydf %>%
  group_by(response) %>%
  tally()
```


<table class="dataframe">
<caption>A tibble: 3 × 2</caption>
<thead>
	<tr><th scope=col>response</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Agree   </td><td>25</td></tr>
	<tr><td>Disagree</td><td>45</td></tr>
	<tr><td>Neutral </td><td>30</td></tr>
</tbody>
</table>



Notice how the responses are out of order, with Agree followed by Disagree, then Neutral, since R defaults to alphabetic ordering for anything that is a categorical variable. One way to ensure the correct ordering of categorical variables is via `ordered`, as shown below. 


```R
mydf %>%
  mutate(
      ordered.response = ordered(
          response,
          levels = c("Agree", "Neutral", "Disagree")
      )
    ) %>%
  group_by(ordered.response) %>%
  tally()
```


<table class="dataframe">
<caption>A tibble: 3 × 2</caption>
<thead>
	<tr><th scope=col>ordered.response</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;ord&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Agree   </td><td>25</td></tr>
	<tr><td>Neutral </td><td>30</td></tr>
	<tr><td>Disagree</td><td>45</td></tr>
</tbody>
</table>



## Concluding thoughts 
We have a covered a lot of ground here but every inch has been critical space. Google any question we have tackled and you will see how many R-users ask the same questions ... how do I calculate mean for groups in R? What you have seen is the heart of the `dplyr()` package. We saw grouped operations, we saw the use of summarise, mutate, case_when, distinct, filter, arrange, select, count, and tally. I will let you in on a secret; while these are core functions, there are others you could experiment with. Look up the cheat-sheet [here](https://rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf). 

Practice what we have done, with the `{nycflights13}` data-set perhaps to get something familiar yet sufficiently different to test your fundamentals. Maybe pick a non-travel data-set altogether, perhaps one of the `tidytuesday` data-sets. What is that you ask? Discover it for yourself [here](https://github.com/rfordatascience/tidytuesday). Bon voyage! Don't go too far because we will be working with two new packages next week -- `{tidyr}` and `{lubridate}`. 

----------

# Exercises for Practice 
## Exercise 01
Why are our best and most experienced employees leaving prematurely? 

The data [available here](https://aniruhil.github.io/avsr/teaching/dataviz/HR_comma_sep.csv) includes information on several current and former employees of an anonymous organization.** Fields in the data-set include: 

| Variable | Description |
| :---- | :---- |
| satisfaction_level | = Level of satisfaction (numeric; 0-1) |
| last_evaluation | = Evaluation score of the employee (numeric; 0-1) |
| number_project | = Number of projects completed while at work (numeric) |
| average_monthly_hours | = Average monthly hours spent at the workplace (numeric)  |
| time_spend_company | = Number of years spent in the company (numeric) |
| Work_accident | = Whether the employee had a workplace accident (categorical; 1 = yes or 0 = no) |
| left | = Whether the employee left the workplace or not (categorical; 1 = left or 0 = stayed)  |
| promotion_last_5years | = Whether the employee was promoted in the last five years (categorical; 1 = yes or 0 = no) |
| sales | = Department in which they work (categorical) |
| salary | = Relative level of salary (categorical; low, med, and high) |

(a) Read in the csv-format data-set, naming it `hrdata` and save it in RData format as `hrdata.RData` 

(b) Create new variables that add labels to Work_accident, left, promotion_last_5years, and create a new variable that orders salary from low to high, and add these to `hrdata`.  

(c) Convert satisfaction_level from a 0-1 scale to a 0-100 scale, making sure to create a new variable of course.  

(d) Now retain only employees who left the company, and had not been promoted in the last five years. Save this result as `hr01`

(e) In this `hr01` data-set, how many employees do you have per sales department? What sales department has the most number of employees? 

(f) By sales department, calculate mean and standard deviation of (i) satisfaction_level, and (ii) last_evaluation. 

(g) What department has the lowest mean satisfaction? How much difference is there in mean satisfaction between departments?  

Accounting has the lowest mean satisfaction level. All departments have mean satisfaction in the 0.403 to 0.482 range, so some difference but a huge one. 

(h) Create a new variable that groups average monthly hours into 4 groups. You can let the group cut-points be chosen automatically with `chop_evenly()`. Then show the frequencies of each group.

## Exercise 02
Thanks to the frenetic work of many individuals, the global spread of the Novel Coronavirus (COVID-19) has been tracked and the data made available for analysis. [Yanchang Zhao](https://rdatamining.wordpress.com/2020/03/10/coronavirus-data-analysis-with-r-tidyverse-and-ggplot2/) is one such individual and for this exercise we will use a spcific version of his data that I have named `cvdata.RData` and made available via Slack. Make sure to upload that data-set to your RStudio Cloud `data` folder, and then to read it in via the `load()` command. We can then answer a few questions. Note the contents: 

+ `country =` name of the country 
+ `date =` date of indidents as recorded 
+ `confirmed =` cumulative count of the number of people who tested positive  
+ `deaths =` cumulative count of the number of people lost to Covid-19 
+ `deaths =` cumulative count of the number of people recovered  

(a) Filter the data-set so that we have only one row per country, the data from March 10, 2020 and call it `cv0310`. 

(b) How many countries have lost `at least one` person to this tragedy? "Others" should not show up as one of the countries.  

(c) What 10 countries have had the most number of confirmed cases? "Others" should not show up as one of the countries. Also ensure the result is organized in descending order of the number of confirmed cases. 

(d) Calculate the `fatality_rate`, defined for our purposes as the percent of deaths. excluding "Others", and only keeping countried that have had `at least 10` confirmed cases, arrange the result to show the top-10 countries in descending order of `fatality_rate`.  

(e) Say we only want to focus on the Baltic countries (Estonia, Latvia, and Lithuania) as a unified group and compare this group to the ASEAN nations (Brunei, Cambodia, Indonesia, Laos, Malaysia, Myanmar, Philippines, Singapore, Thailand, and Vietnam). Use `cv0310` to complete the followng tasks: 

(i) Create a new variable called `region` that only takes on two values -- "Baltic" if the country is a Baltic country and "Asean" if the country is an ASEAN country. 
(ii) Use this variable to calculate the total number of confirmed cases in each region. 
