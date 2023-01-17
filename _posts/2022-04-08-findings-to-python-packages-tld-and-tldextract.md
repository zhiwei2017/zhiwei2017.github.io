---
title: "Findings to Python Packages tld and tldextract"
date: 2022-04-08T01:56:15+01:00
categories:
  - blog
tags:
  - Python
  - tld
  - tldextract
---

Python Package **tldextract** is used by the framework **scrapy** for getting the top level domain. 
However, on the very first call of the function **tldextract.extract**, it will try to download the suffix list 
from [https://publicsuffix.org](https://publicsuffix.org) via HTTP request by default. 

In my opinion, this is a very dangerous design for the projects using its default configuration 
in Prod environment. Even though the author of the package **tldextract** stated it in the README.md 
from [https://github.com/john-kurkowski/tldextract](https://github.com/john-kurkowski/tldextract), but the 
**scrapy** framework didn't mention it in their documentation. 

The solution to use a local downloaded suffix list is very simple, you can just simply add 
those two lines in your **Dockerfile** after installing all the dependencies listed in your requirements.txt file:

```docker
ENV TLDEXTRACT_CACHE="$HOME/.cache/python-tldextract"
RUN tldextract --update
```

The code above will specify the cache path and download the suffix list while building docker images.
