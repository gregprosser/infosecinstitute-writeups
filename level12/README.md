# Level 12

## Description

This level consists of an image of [Yoda](http://en.wikipedia.org/wiki/Yoda) with the text "Dig deeper!".  There appear to be no links to other content or hints.

## Solution

Checking the image with `strings` or `exiftool` (see other levels) for hidden data doesn't give us any more information.

However, if we download this level's HTML source code, and compare it to previous levels, we notice more than just the image is different!  Level one had the same image, so if we use the UNIX `diff` utility to compare the two files, we see a difference that isn't the image or bounty:

```shell
$ diff -u <(curl -s http://ctf.infosecinstitute.com/levelone.php) <(curl -s http://ctf.infosecinstitute.com/leveltwelve.php)
```

```diff
@@ -8,6 +7,7 @@
     <title>Infosec Institute n00bs CTF Labs</title>
     <link href="css/bootstrap.css" rel="stylesheet">
     <link href="css/custom.css" rel="stylesheet">
+    <link href="css/design.css" rel="stylesheet">
   </head>

 <body>
```

This is a reference to an additional file that isn't in the other levels.

We can see the contents of the file [here](http://ctf.infosecinstitute.com/css/design.css):

```css
.thisloveis{
	color: #696e666f7365635f666c616769735f686579696d6e6f7461636f6c6f72;
}
```

This string looks like a collection of hex digits - a number system that adds the letters `a` to `f` to express numbers up to 16 - and is even.  In HTML and CSS, Colors are represented as [hexadecimal colours](http://en.wikipedia.org/wiki/Web_colors), but hex colours are only 3 bytes (6 characters) and this string is much longer!

It's likely a string of bytes represented by 2 digit [Hexadecimal](http://en.wikipedia.org/wiki/Hexadecimal).  Using python we can decode this string to get our flag:

```python
>>> s = "696e666f7365635f666c616769735f686579696d6e6f7461636f6c6f72"
>>> print ''.join([chr(int(s[i:i+2], 16)) for i in range(0, len(s), 2)])
infosec_flagis_heyimnotacolor
```

## Flag

`infosec_flagis_heyimnotacolor`
