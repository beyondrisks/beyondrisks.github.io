---
layout: single
title:  "SAS to Python: Read Data"
date:   2022-01-22
categories: code SAS Python
---

As much as we love SAS, we cannot deny the trend that Python is replacing SAS in a very fast pace. In this artical we will discuss how to use Python to read data. The data Python package used here is `pandas`. 

The first thing of data analysis is usually to read data. There are tons of scenarios regarding reading how to read data in SAS depending on the data format. The two methods used in SAS are `proc import` and the combination of `infile` and `input` statements. `Proc import` can be used for well formatted data such as CSV file, Excel file while infile and input can be more flexible.  

## read_csv
One of the most common data files is the one in which data are separated by some delimiters, such as comma, space, tab or other characters. The following data are separated by comma. 

|id,name,age|
|---------|
|1,Mike,23|
|2,Bob,49|
|3,Eric,10|

It can be easily read into SAS data set by using either `Proc import` or `infile` and `input`.

{% highlight SAS %}
/* SAS Version */
Proc import datafile = 'to_be_read_file.txt' 
    out = employee
    dbms = csv
    replace;
run;
{% endhighlight %}

Here option `dbms` specifies the type of the file to be read. `csv` means the input file is a file with comma separated values. It can take other values such as `DLM` (delimted files), `EXCEL` (EXCEL files). Please refer to [SAS documentation](https://documentation.sas.com/doc/en/pgmsascdc/9.4_3.5/acpcref/p0jf3o1i67m044n1j0kz51ifhpvs.htm) for details.

The Python script to read such file is even easier with `Pandas read_csv` function. Please note that this function cannot read Excel files. Use function `read_excel` for Excel files.

{% highlight python %}
# Python Version
import pandas as pd
employee = pd.read_csv('to_be_read_file.txt', sep=',')
{% endhighlight %}
**Delimiters** The `sep=','` option here is similar to the `dbms` option in `Proc import.` Depending on the data file, one can choose space (`sep = ' '`), tab (`sep = r'\t'`), multiple space (`sep = r'\s+'`), or a combination of space and tab (`sep = r'\s+|\t+`) or any other characters. It can be a regular string or a regular expression.

**Reading Columns** It is not necessary to read all the columns in the data file. One can use `usecols` option to pick desired columns. This option takes value of a list of integers or strings. For example, in the previous example of data file, if we just want to read the first and the third column, we can use the following command. Note that the indexing starts with 0 instead of 1. 

{% highlight python %}
# read the first and third columns
employee = pd.read_csv('to_be_read_file.txt', sep=',', usecols=[0, 2])
{% endhighlight %}

**Column Names** If the column names are included in the first row of the raw file as in the example above, `read_csv` function reads them as column names by default. On the other side, if there are no column names in the raw data file, one can specify the column names using option `names` in the function. Let's assume another example without the first line, the following command can read the file and assign names to the columns.

{% highlight python %}
# specify the column names
employee = pd.read_csv('to_be_read_file.txt', sep=',', names=['id', 'name', 'age'])
{% endhighlight %}

**Data Type** In SAS `import` proc the data type of one column is inferred from the raw data. One can specify the number of rows used to determined the data type by using the statement `guessingrows=100;`. Here 100 is the number of lines one can specify. In the Python `read_csv` function, one can actually specify the data type for individual columns by using the option `dtype` which takes a dictionary with column name as a key and data type as the correspoonding value. The following example specifies the data type of 'int32' for the column id and 'int16' for the column age.

{% highlight python %}
# specify the date types
employee = pd.read_csv('to_be_read_file.txt', sep=',', dtype={'id': 'int32', 'age': 'int16'})
{% endhighlight %}

When one need more flexibility in handling data, one would switch the combination of `input` and `infile` statements in SAS. One example is that when we need to read a string or a number as a date. This can be achieved by using `informat` statement together with `input` and `infile` in SAS. But it can be easily done in Python using the option `date_parse` in the function `read_csv`. Let's add one more column 'birthday' to the above example.

|id,name,age,birthday|
|------------|
|1,Mike,23,02131998|
|2,Bob,49,02131997|
|3,Eric,10,10301988|

The code below reads the birthday into a date column. The value of `parse_date` can be a list of column numbers or column names.

{% highlight python %}
# read birthday to a date
employee = pd.read_csv('to_be_read_file.txt', sep=',', parse_date=['birthday'])
{% endhighlight %}
## read_fwf
Another example is that data files are not delimiter separated, but each column starts from fixed position and has fixed length. In SAS we use `@n` to specify the starting position of one column in the `input` statement. There is another function in pandas called `read_fwf`. The main syntax is as follows.

{% highlight python %}
# read_fwf
pd.read_fwf('to_be_read_file.txt', colspecs=[(0, 5), (10, 14)])
{% endhighlight %}
The parameter `colspecs` is a list of tuples and each tuple includes the starting and ending position of each column.

## Summary
1. `read_csv` reads delimited data. One can specify the columns to be read, set the column names, specify the delimiters, specify the data type and a lot more.
2. `read_fwf` reads data with fixed starting and ending positions.

There are lots of other complicated data files that cannot be read either by `read_csv` or `read_fwf` such as unformated data file. In those cases, one need to use `open()` to read file line by line and handle them case by case.
