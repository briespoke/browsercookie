# Browser Cookie #

* ***What does it do?*** Loads cookies used by your web browser into a cookiejar object. 
* ***Why is it useful?*** This means you can use python to download and get the same content you see in the web browser without needing to login.
* ***Which browsers are supported?*** Currently Chrome and Firefox.
* ***How are the cookies stored?*** In a sqlite database in your home directory.

## Install ##
```
#!bash

    pip install browser-cookie

```


## Usage ##

Here is a *dangerous* hack to extract the title from a webpage:
```
#!python
>>> import re
>>> get_title = lambda html: re.findall('<title>(.*?)</title>', html, flags=re.DOTALL)[0].strip()
```

And here is the webpage title when downloaded normally:
```
#!python
>>> import urllib2
>>> url = 'https://bitbucket.org/'
>>> public_html = urllib2.urlopen(url).read()
>>> get_title(public_html)
'Git and Mercurial code management for teams'
```

Now let's try with browser_cookie - make sure you are logged into Bitbucket in Firefox before trying this example:
```
#!python

>>> import browser_cookie
>>> cj = browser_cookie.firefox()
>>> opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cj))
>>> login_html = opener.open(url).read()
>>> get_title(login_html)
'richardpenman / home &mdash; Bitbucket'
```

You should see your own username here, meaning the module successfully loaded the cookies from Firefox.

Here is an alternative example with [requests](http://docs.python-requests.org/en/latest/), this time loading the Chrome cookies. Again make sure you are logged into Bitbucket in Chrome before running this:
```
#!python

>>> import requests
>>> cj = browser_cookie.chrome()
>>> r = requests.get(url, cookies=cj)
>>> get_title(r.content)
'richardpenman / home &mdash; Bitbucket'
```


## Contribute ##
So far the following platforms are supported:

* **Chrome:** Linux, OSX
* **Firefox:** Linux, OSX, Windows
 

However I only tested on a single version of each browser and so am not sure if the cookie sqlite format changes location or format in earlier/later versions. If you experience a problem please [open an issue](https://bitbucket.org/richardpenman/browser_cookie/issues/new) which includes details of the browser version and operating system. Also patches to support other browsers are very welcome, particularly for Chrome and Internet Explorer on Windows.