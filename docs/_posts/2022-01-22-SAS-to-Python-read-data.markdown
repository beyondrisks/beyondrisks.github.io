---
layout: single
title:  "SAS to Python: Read Data"
date:   2022-01-22
categories: code, SAS, Python
---

As much as we love SAS, we cannot deny the trend that Python is replacing SAS in a very fast pace. In this artical we will discuss how to use Python to do some most common SAS usages, such as reading and cleaning data, statistical analysis, and statistical or econometric modeling. The main packages used in Python are Pandas and Numpy for data related tasks and some statistical analysis, and Siklearn for modeling. 

## Read Data
The first thing of data analysis is usually to read data. There are tons of scenarios regarding reading how to read data in SAS depending on the data format. The two methods used in SAS are `proc import` and the combination of `infile` and `input` statements. `Proc import` can be used for well formatted data such as CSV file, Excel file while infile and input can be more flexible.  

### Formatted Data with Delimiter
This type data is separated by as delimiter, such as comma, space, tab or other characters. The following data is separated by comma. 


|id,name,age|
|---------|
|1,Mike,23|
|2,Bob,49|
|3,Eric,10|

It can be easily read into SAS data set by using either `Proc import` or `infile` and `input`.
```SAS
/* SAS Version */
Proc import datafile = 'to_be_read_file.txt' 
    out = employee
    dbms = csv
    replace;
run;
```
Here option `dbms` specifies the type of the file to be read. `csv` means the input file is a file with comma separated values. It can take other values such as `DLM` (delimted files), `EXCEL` (EXCEL files). Please refer to [SAS documentation](https://documentation.sas.com/doc/en/pgmsascdc/9.4_3.5/acpcref/p0jf3o1i67m044n1j0kz51ifhpvs.htm) for details.

The Python script to read such file is even easier with `Pandas read_csv` function. Please note that this function cannot read Excel files. Use function `read_excel` for Excel files.
```Python
# Python Version
import pandas as pd
employee = pd.read_csv('to_be_read_file.txt', sep=',')
```
**Delimiters** The `sep=','` option here is similar to the `dbms` option in `Proc import.` Depending on the data file, one can choose space (`sep = ' '`), tab (`sep = r'\t'`), multiple space (`sep = r'\s+'`), or a combination of space and tab (`sep = r'\s+|\t+`) or any other characters. It can be a regular string or a regular expression.

**Reading Columns** It is not necessary to read all the columns in the data file. One can use `usecols` option to pick desired columns. This option takes value of a list of integers or strings. For example, in the previous example of data file, if we just want to read the first and the third column, we can use the following command. Note that the indexing starts with 0 instead of 1. 
```Python
# read the first and third columns
employee = pd.read_csv('to_be_read_file.txt', sep=',', usecols=[0, 2])
```

**Column Names** If the column names are included in the first row of the raw file as in the example above, `read_csv` function reads them as column names by default. On the other side, if there are no column names in the raw data file, one can specify the column names using option `names` in the function. Let's assume another example without the first line, the following command can read the file and assign names to the columns.
```Python
employee = pd.read_csv('to_be_read_file.txt', sep=',', names=['id', 'name', 'age'])
```


