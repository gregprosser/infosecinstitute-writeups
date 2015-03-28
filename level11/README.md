# Level 11

## Descrption

This page contains nothing but a (what looks to be) corrupted image of the logo for the PHP programming language.

## Solution

The graininess of the image makes me immediately think of Steganography - hiding data inside an image by using unused space or random pixels - but it would be quite the bad Steganography if it was that obvious.

One of the tools useful for analyzing files is a UNIX utility called `strings`.  It scans the file for any blocks of ASCII characters, regardless of file format.  As it doesn't know anything about the file format, it's safe to run this command on, for example, an unknown executable file to try to deduce from error messages what it might do.

In this case, running strings and using the UNIX grep utility to find a flag.. finds a flag.

```shell
$ strings php-logo-virus.jpg | grep -i flag
infosec_flagis_aHR0cDovL3d3dy5yb2xsZXJza2kuY28udWsvaW1hZ2VzYi9wb3dlcnNsaWRlX2xvZ29fbGFyZ2UuZ2lm
```

It turns out that this is stored in the file's [EXIF](http://en.wikipedia.org/wiki/Exchangeable_image_file_format) information in the "Document Name" field:

```shell
$ exiftool php-logo-virus.jpg | grep 'Document Name'
Document Name                   : infosec_flagis_aHR0cDovL3d3dy5yb2xsZXJza2kuY28udWsvaW1hZ2VzYi9wb3dlcnNsaWRlX2xvZ29fbGFyZ2UuZ2lm��.
```

It's tempting to stop now, but the long blob at the end of the flag is suspicious.  Given the character set (lack of anything other than alphabetic and numeric characters) one immediately thinks about [Base64](http://en.wikipedia.org/wiki/Base64) encoding.  Sure enough, this yields more information.

```python
>>> import base64
>>> print base64.b64decode('aHR0cDovL3d3dy5yb2xsZXJza2kuY28udWsvaW1hZ2VzYi9wb3dlcnNsaWRlX2xvZ29fbGFyZ2UuZ2lm')
http://www.rollerski.co.uk/imagesb/powerslide_logo_large.gif
```

Which is this image:

![Powerslide](http://www.rollerski.co.uk/imagesb/powerslide_logo_large.gif "Powerslide")

I'm assuming that modifying files on external sites as clues is probably too far for this challenge, and this is probably more of a clue.  I'm going with the assumption that the flag is `infosec_flagis_powerslide`.

## Flag

`infosec_flagis_powerslide`
