---
title: Let's start with automating reading data files in R!
subtitle: "Hi there \U0001F44B Here is an example code to efficiently read data files in R from a folder 
by automating the whole process without needing to know the names of the files."
summary: "Hi there \U0001F44B Here is an example code to efficiently read data files in R from a folder 
by automating the whole process without needing to know the names of the files."
author: "Gulsah Gurkan"
date: "2021-12-05T00:00:00Z"
categories:
- R
tags:
- Demo
draft: false
featured: false
image:
  caption: 
  focal_point: 
  placement: 
  preview_only: 
lastmod: 
projects:
---

Sometimes you need to read in the data file (or more importantly file**s**) from a folder without knowing what they are named. If it is a single file you need for just one time, you may think: “OK, I’ll just get the file path. What is the big deal?!” But, you may be in a situation where the data file gets updated consistently with some tweaks in its name (e.g., the date that the data was updated on, initials of the person uploading it etc.) and you need to account for that change in the file path in your code. Another situation may be when reading in multiple data files from the same folder. In that case, you may be reading in each file separately, one by one. Do I hear: “Repeating almost the same line of code multiple times, I hate that!” 

Well, you are in luck. Try this:

```r
# this is the path to the folder you’d like to read the data files in.
fpath <- “…” 

# this is where you get the names of all the files in that folder.
dfnames <- list.files(fpath) 
```

Let’s read in the file that is alphabetically the first in the folder. Assuming you know the file format, let's say it is a .csv file:

```r
df <- readr::read_csv(paste0(fpath, "\\", dfnames[1]))
```

👉 Be sure to put the separator that is consistent with your fpath format (could be either “\\\” or “/”).

Now, if you would like to read in multiple files with the same file type from the same folder at once into a list, you can try this:


```r
# Create an empty list to append the data files
dflist <- list()

# Read in the csv files only
for(i in dfnames[stringr::str_detect(dfnames, ".csv")]){
  tt <- readr::read_csv(paste0(fpath,"\\",i))
  dflist[[i]] <- tt
}
```

Note that reading the name of the files from the folder directly gives you the opportunity to detect the file type and read them in accordingly. You can conditionally read the files in to account for each file type in the folder. For example, you can conditionally read the data files that are either .csv or .xlsx like this:

```r
# Create an empty list to append the data files
dflist <- list()

# Read in the data files based on type
for(i in dfnames){
  if(stringr::str_detect(i, ".csv")){
    tt <- readr::read_csv(paste0(fpath,"\\",i))
    dflist[[i]] <- tt
  }else if(stringr::str_detect(i, ".xlsx")){
    tt <- readxl::read_xlsx(paste0(fpath,"\\",i))
    dflist[[i]] <- tt
  }
}
```


💡 Bonus:

Do you need to read in data file/s from a zipped file? Here is a lifesaver using the unzip() and unz() functions to unzip files:


```r
# read in a data file ("my_data.csv") from a zipped folder ("my_zipped_file.zip"):
filenames_zipped <- as.character(unzip("...\\my_zipped_file.zip", list = TRUE)$Name)
my_data <- readr::read_csv(unz("...\\my_zipped_file.zip", "my_data.csv"))
```

If you would like to see an example of these types of automation in full action, check out this R script from a project that I worked on a while ago:

👉 [**Pre-processing NCES Common Core of Data**](https://github.com/Gulsah-G/CCD_data_prep) 











