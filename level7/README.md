# Level 7

# Description

Level 7 starts with a challenge in itself - if you browse to the "Level 7" link from the menu, you end up at `http://ctf.infosecinstitute.com/404.php`, which is a funny message:

```
f00 not found 
Something is not right here???
btw...bounty $70
```

So, using the pattern from the previous levels, you might try hitting `http://ctf.infosecinstitute.com/levelseven.php`, but this produces a blank page.  In fact, no content whatsoever:

```shell
$ curl http://ctf.infosecinstitute.com/levelseven.php
$
```

## Solution

Borrowing some tricks from [Level 4](../level4/), lets look at the server headers:

```shell
$ curl -v -s -o /dev/null http://ctf.infosecinstitute.com/levelseven.php
* Hostname was NOT found in DNS cache
*   Trying 52.10.161.229...
* Connected to ctf.infosecinstitute.com (52.10.161.229) port 80 (#0)
> GET /levelseven.php HTTP/1.1
> User-Agent: curl/7.37.1
> Host: ctf.infosecinstitute.com
> Accept: */*
>
* HTTP 1.0, assume close after body
< HTTP/1.0 200 aW5mb3NlY19mbGFnaXNfeW91Zm91bmRpdA==
< Date: Sun, 15 Mar 2015 20:34:15 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.6
< Content-Length: 0
< Connection: close
< Content-Type: text/html
<
* Closing connection 0
$
```

Combining what we also know about base64 from [Level 2](../level2/), we can decode this:

```shell
$ echo "aW5mb3NlY19mbGFnaXNfeW91Zm91bmRpdA==" | openssl base64 -d; echo
infosec_flagis_youfoundit
$
```

## Flag

`infosec_flagis_youfoundit`
