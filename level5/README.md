# Level 5

## Description

The introductory page contains a for loop that constantly pops up a dialog box saying "Hacker!!!".

Because this is an endless loop, in my browser, the page fails to render the later part.  If we retrieve the source code for the page, we see that later in the source is a reference to `img/aliens.jpg`.

## Solution

Looking closely at the source, there appears to be nothing other than `aliens.jpg` in the form of clues.  In this situation, chances are `aliens.jpg` is our actual clue.

In level 4, we mentioned searching the image's [EXIF](http://en.wikipedia.org/wiki/Exchangeable_image_file_format) for clues, but once again we turn up empty.

However, searching around the InfoSec Institute's site, we can find [this article](http://resources.infosecinstitute.com/steganalysis-x-ray-vision-hidden-data/) on steganography (hiding things within other file formats).

Sure enough, we can retrieve something via `steghide`.

```shell
$ steghide extract -sf aliens.jpg
Enter passphrase:
wrote extracted data to "all.txt".
$ cat all.txt
01101001011011100110011001101111011100110110010101100011010111110110011001101100011000010110011101101001011100110101111101110011011101000110010101100111011000010110110001101001011001010110111001110011
```

This looks like binary data.  There are 200 bits in this data, which conveniently is also a multiple of 8 (the number of bits in a byte).

If we break this string on byte boundaries, and convert them to characters, we get a message:

```python
>>> s = open("all.txt").read()
>>> s = s.strip()
>>> bin = [s[i:i+8] for i in range(0, len(s), 8)]
>>> bin
['01101001', '01101110', '01100110', '01101111', '01110011', '01100101', '01100011', '01011111', '01100110', '01101100', '01100001', '01100111', '01101001', '01110011', '01011111', '01110011', '01110100', '01100101', '01100111', '01100001', '01101100', '01101001', '01100101', '01101110', '01110011']
>>> ''.join([chr(int(x, 2)) for x in bin])
'infosec_flagis_stegaliens'
>>>
```

## Flag

`infosec_flagis_stegaliens`
