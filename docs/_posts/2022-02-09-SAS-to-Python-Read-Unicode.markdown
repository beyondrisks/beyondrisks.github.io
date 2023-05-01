---
layout: single
title:  "SAS to Python: Different Length of Special Characters"
last_modified_at: 2022-02-10
classes: wide
categories:
  - code
  - SAS
  - Python
---
I would like to share the difference between SAS and Python in dealing with special characters in this artical. Please note that this article is not a comprehensive discussion about the unicode, but just focuses on a tiny issue. 

The lengths of the special characters are different in SAS and Python. For example, the length of __ö__ is three using `length` function in SAS, but is 2 using `len` function in Python. Another example is __♞__ whose length is 3 in SAS and 1 in Python. 

## Why it matters?

In my case, I need to replicate the results in SAS. The SAS code takes the first 5 characters of one string, 'a♞dicefkdl', for example. Since the length of ♞ is 3, then `substr('a♞dicefkdl', 1, 5)` in SAS gives 'a♞d'. If I use string slicing in Python, `'a♞dicefkdl'[:5]`, it gives 'a♞dic', which is different from result in SAS.

## Why it is different?

It turns out that in SAS the length of a string is calculated on UTF-8 encoding. The UTF-8 encoding of ♞ is '\xe2\x99\x9e', so the length is 3 in SAS. I am not sure why the length is 2 in Python. 

## How to solve?

**Method 1** encode the whole string in Python first, then take the first 5, and then decode it back.

{% highlight python %}
'a♞dicefkdl'.encode()[:5].decode()
{% endhighlight %}

**Method 2** adjust the slicing length first. Find the length difference between UTF-8 encoding and normal form first and then subtract the difference from the requested length of the string. The following code gives the correct answer.
{% highlight python %}
len_new = 5 - len('a♞dicefkdl'.encode()) - len('a♞dicefkdl')
'a♞dicefkdl'[:len_new]
{% endhighlight %}

## Unicode
In [Python's document](https://docs.python.org/3/howto/unicode.html), it says 

> [Unicode] (https://www.unicode.org/) is a specification that aims to list every character used by human languages and give each character its own unique code. 

Python uses unicode standard to represent characters.
