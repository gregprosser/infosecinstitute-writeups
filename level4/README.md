# Level 4

## Description

The introductory page contains a large image of cookie monster, and a JavaScript mouseover event that pops up a (rather annoying) dialog box that says "Stop poking me!".  There's also the text:

> HTTP means Hypertext Transfer Protocol

## Solution

Initially, it appears that the image is the clue.  If you fetch the image `http://ctf.infosecinstitute.com/img/thumb.jpg` and view its properties (sometimes additional data can be given away via an image's [EXIF](http://en.wikipedia.org/wiki/Exchangeable_image_file_format) data) no clues appear.  If you search the entire file for `flag` nothing appears.

However, the text gives us another hint.  Every webpage and image is served to you via [HTTP](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol).  If you use a tool to fetch the page in a verbose mode (to show the web header information), you'll notice something mildly suspicious.

```shell
$ curl -s -v http://ctf.infosecinstitute.com/levelfour.php -o /dev/null
* Hostname was NOT found in DNS cache
*   Trying 52.10.161.229...
* Connected to ctf.infosecinstitute.com (52.10.161.229) port 80 (#0)
> GET /levelfour.php HTTP/1.1
> User-Agent: curl/7.37.1
> Host: ctf.infosecinstitute.com
> Accept: */*
>
< HTTP/1.1 200 OK
< Date: Sun, 15 Mar 2015 20:07:42 GMT
* Server Apache/2.4.7 (Ubuntu) is not blacklisted
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.6
< Set-Cookie: fusrodah=vasbfrp_syntvf_jrybirpbbxvrf
< Vary: Accept-Encoding
< Content-Length: 3514
< Content-Type: text/html
<
{ [data not shown]
* Connection #0 to host ctf.infosecinstitute.com left intact
```

Notice the `Set-Cookie` header.  The underscores make this look like the flags from the earlier levels...

Using a utility like `tr` (to translate letters), if we start by assuming "vasbfrp_syntvf" is "infosec_flagis", we can see we're on to something:

```shell
$ echo "fusrodah=vasbfrp_syntvf_jrybirpbbxvrf" | tr "vasbfrp_syntvf" "INFOSEC_FLAGIS"
SuFEodNh=INFOSEC_FLAGIS_jELOiECOOxIES
$
```

This doesn't get us very far, however, as we're running out of clues.  It was at this point I remember that another common *ix trick is rotating an alphabet my 13 characters, so called [ROT13](http://en.wikipedia.org/wiki/ROT13).  Sure enough, all the characters we've found so far are 13 characters apart.  Using `tr` with our new information, we can decode the entire string:

```shell
$ echo "fusrodah=vasbfrp_syntvf_jrybirpbbxvrf" | tr "n-za-m" "a-z"
shfebqnu=infosec_flagis_welovecookies
```

## Flag

`infosec_flagis_welovecookies`
