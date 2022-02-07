---
layout: single
title:  "SAS to Python: Read Binary Numerical Data"
last_modified_at: 2022-02-04
classes: wide
categories:
  - code
  - SAS
  - Python
---

In this article we will discuss the difference in reading binary numerical data between SAS and Python.

## Byte Order

Integer values for integer binary data are typically stored in one of three sizes: 1 byte, 2 bytes, or 4 bytes. The ordering of the bytes for the integer varies depending on the platform (operating environment) on which the integers were produced. There are two types of byte orders, ["big-endian" and "little-endian"](https://en.wikipedia.org/wiki/Endianness). In short, the main difference lies in where most significant byte is stored. For example, if an integer is stored in 4 bytes, then 2 is stored as 10 00 00 00 in little-endian and as 00 00 00 10 in big-endian. In this article we will just focus on the little-endian, since it is used in Windows and Linux.

## SAS

SAS uses informats to read the binary data and the corresponding formats for writing the binary data. The common informats are IB*w.d*, PIB*w.d*. 

__IB*w.d*__: for integer binary values. *w* specifies the width of the input fields and *d* is a scaling factor and specifies the power of 10 that divides the integer.

__PIB*w.d*__: for positive integer binary values.

> Note that when using `infile` statement to open a binary file, one has to specify option `recfm` to value `n` so that SAS knows the file is a binary file.

## Python

Use `open(file_name, 'rb')` to open the binary file, remember to include `b` to indicate a binary file. There are two ways commonly used in Python to read binary numerical data. The first one is to use function `int.from_bytes()`. It is simple and quick. The following example converts the bytes b'\xf8' to the integer 248. 

{% highlight python %}
int.from_bytes(b'\xf8')
{% endhighlight %}

However, it needs a loop if one is converting a series of bytes. In such case, it is easier to use the built in package [`struct`](https://docs.python.org/3/library/struct.html). The specific function used to read binary data is `struct.unpack`. Let's say there is a stream of bytes containing 4 integers each of which is stored in 4 bytes. The following one command can read all 4 integers at the same time. 

{% highlight python %}
import struct
struct.unpack('iiii', b'\xf8\x17\x00\x00\x00\x00\x00\x00\x01\x00\x00\x00\xf6\x0b\x00\x00')
{% endhighlight %}

It returns a tuple of 4 integers. Here `iiii` is the format string similar to informats/formats in SAS, which tells Python how the integers are stored. It can also be written as `'i'*4`. The following table compares some formats with SAS informats. 

|Format|Python Type|Standard Size|SAS Informat/Format|
|---|---|---|---|
|b|integer|1|IB1.|
|B|integer|1|PIB1.|
|h|integer|2|IB2.|
|H|integer|2|PIB2.|
|i|integer|4|IB4.|
|I|integer|4|PIB4.|
|q|integer|8|IB8.|
|Q|integer|8|PIB8.|

## Example

We will be using [this binary file]({{ site.url }}/../../assets/data/binary_integer). Feel free to download the file for your own test. The file has 21 bytes or 9 integers. The first 4 integers are stored in 4 bytes each and the last 5 integers are store in 1 byte each. 

- SAS

{% highlight SAS %}
data test;
  infile 'binary_integer' recfm=n;
  array y{5} y1-y5;
  input (x1 x2 x3 x4) (ib4.);
  do j = 1 to 5;
    input y{j} pib1.;
  end;
  drop j;
run;
{% endhighlight %}

- Python

{% highlight python %}
import struct
with open('binary_inter', 'rb') as f:
    x = f.read() # read all the content
    x1, x2, x3, x4 = struct.unpack('i'*4, x[:16])
    y1, y2, y3, y4, y5 = struct.unpack('b'*5, x[17:])
{% endhighlight %}


