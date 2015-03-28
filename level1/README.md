# Level 1

## Description

The introductory page contains a giant stylized yoda picture, and the following text:

```
May the source be with you!
```

## Solution

The solution for this one is pretty basic, but it's a good start to anyone entering this CTF as their first foray into the security world.

The most common place for starting hackers to start playing with sites is by viewing the site's source code and attempting to decipher what may be the weak points in the site's design that would be ripe for hacking.  The text based clue is essentially saying "view source".

If you open up the page's source code using your browser, you can very easily find the flag at the top of the file.

If you want to, you can do this pretty easily on the commandline:

```shell
$ curl -s http://ctf.infosecinstitute.com/levelone.php|grep flag
<!-- infosec_flagis_welcome -->
```

I'll admit that this stumped me for far too long than it should have, because I didn't know the format of the flag.  Sitting there for a while pouring over the source until I got annoyed and scrolled to the top and saw the flag.

## Flag

`infosec_flagis_welcome`
