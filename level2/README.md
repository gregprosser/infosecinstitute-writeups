# Level 2

## Description

The introductory page contains a missing image icon, and a large button with the following text:

```
It seems like the image is broken..Can you check the file?
```

## Solution

Most modern browsers include a "DOM Inspector" allowing you to play around in the [Document Object Model](http://en.wikipedia.org/wiki/Document_Object_Model) which represents the source of the page.

If you right click the broken image and pick "Inspect Element" (in Chrome - or just find the &lt;img&gt; tag in the source view of another browser), you see:

```
<img src="img/leveltwo.jpeg" />
```

If you put the full URL `http://ctf.infosecinstitute.com/img/leveltwo.jpeg` into your browser, you still get the broken image icon, not some sort of error.  There must be something to this file.

Going back to our friend cURL, if we fetch the image we can immediately see this isn't an image:

```shell
$ curl -s http://ctf.infosecinstitute.com/img/leveltwo.jpeg
aW5mb3NlY19mbGFnaXNfd2VhcmVqdXN0c3RhcnRpbmc=
```

To most experienced hackers, this should be immediately recognizable as a base64 string.  The lack of characters other than letters and numbers and = is a dead giveaway.

## Base64

Base64 encoding is a way of encoding binary (or really any data) using a small subset of "safe" characters to allow it to be transferred easier in places that might have trouble transfering high-ascii characters, for example.  See: [Wikipedia](http://en.wikipedia.org/wiki/Base64)

## Decoding

Thankfully, most *ix OSs include openssl which lets you decode this pretty easily.

```shell
$ curl -s http://ctf.infosecinstitute.com/img/leveltwo.jpeg | openssl base64 -d; echo
infosec_flagis_wearejuststarting
$
```

## Flag

`infosec_flagis_wearejuststarting`
