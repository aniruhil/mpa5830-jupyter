## Exercises for practice 
These are some exercises you can use to practice and build your R skills. They are not for grade. 

#### Exercise 01: Reading in some data files 

1. Create a new Notebook by going to File -> New -> Notebook.

2. When prompted to select a kernel, use the dropdown menu to select the _R_ kernel. 

3. The notebook will be untitled, so go ahead and save it with a name, something like `yourlastname_ex01` and you will see `yourlastname_ex01.ipynb` as the name. 

4. Now read in the `Stata` data file found [here](http://www.stata.com/data/jwooldridge/eacsap/mroz.dta) 

5. Create a new cell and run the `summary` command to check the contents of this data file and look at the first 6 rows of data with the appropriate `head` command and look at the first 6 rows of data with the appropriate `head` command.

6. In a new cell, read in the `SPSS` file found [here](http://calcnet.mth.cmich.edu/org/spss/V16_materials/DataSets_v16/airline_passengers.sav) 

7. In a new cell, run the `summary` command and look at the first 6 rows of data with the appropriate `head` command.


```R
library(tidyverse)

library(haven)

read_dta(
    "http://www.stata.com/data/jwooldridge/eacsap/mroz.dta"
    ) -> mroz
```


```R
summary(mroz)
```


```R
head(mroz)
```


```R
read_sav(
    "http://calcnet.mth.cmich.edu/org/spss/V16_materials/DataSets_v16/airline_passengers.sav"
    ) -> airline_passengers
```


```R
summary(airline_passengers)
```


```R
head(airline_passengers)
```

#### Exercise 02: Reading in local data and labeling some values

1. Download [this dataset](https://s3.amazonaws.com/tripdata/201502-citibike-tripdata.zip), extract the file inside the zip archive and upload it to the `data` folder. 

2. In a new cell, read in this uploaded data file with the appropriate commands. 

3. The variable `gender` has the following codes: `Zero = unknown; 1 = male; 2 = female`.  Use this coding scheme to create a new column that shows `gender` as a `factor` with these value labels 

4. Check the first 6 rows of the dataset and also run `summary` to check the new column was created as desired. 

5. In a new cell, write the commands necessary to save each of the three data-sets as separate `RData` files. 


```R
read_csv(
    "data/201502-citibike-tripdata.csv"
    ) -> citibike
```


```R
factor(citibike$gender,
      levels = c(0, 1, 2),
      labels = c("Unknown", "Male", "Female")
      ) -> citibike$gender.f
```


```R
head(citibike)
```


```R
summary(citibike)
```

#### Exercise 03: Welcome to Kaggle & Mass Shootings 

Go to [this page on Kaggle](https://www.kaggle.com/zusmani/us-mass-shootings-last-50-years) and read the description of the data-set on mass shootings in the United States that occurred during the 1966-2017 period. once you have read the overview of the data, click the "Data" tab and download the file called `Mass Shootings Dataset.csv`. Be careful; there are several versions so the one you want is the very last one and not any that have a version number attached, such as "Mass Shootings Dataset Ver 2.csv" for example. 

Now read this file, perhaps naming it `shootings` and run the `summary()` command on it. Note the number of observations and the number of variables in the data-set. 

Make sure you save the file in RData format as well. 


```R
read_csv(
    "data/Mass Shootings Dataset.csv"
    ) -> shootings
```


```R
summary(shootings)
```


```R
save(shootings, file = "data/shootings.RData")
```

#### Exercise 04: Animal Shelters 

Go to [this page on Kaggle](https://www.kaggle.com/aaronschlegel/austin-animal-center-shelter-outcomes-and) and download the file called `aac_shelter_outcomes.zip`, unzip it, and AFTER reading the data overview, read in the file and generate the usual `summary` and also save it as an RData file.


```R
read_csv(
    "data/aac_shelter_outcomes.csv"
    ) -> shelter

summary(shelter)

save(shelter, file = "data/shelter.RData")
```


```R

```
