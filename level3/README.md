# Level 3

## Description

The introductory page contains a large [QR code](http://en.wikipedia.org/wiki/QR_code), which is obvious by the alignment boxes around the image and a progress bar.

## Solution

If you take the URL of the QR code image, `http://ctf.infosecinstitute.com/img/qrcode.png`, and paste it into a [QR code decoder](http://zxing.org/w/decode.jspx), you get the following blob back:

```.. -. ..-. --- ... . -.-. ..-. .-.. .- --. .. ... -- --- .-. ... .. -. --.```

This is morse code - an encoding algorithm that represents the alphabet as a series of short and long pulses (dots and dashes) used before we could send complex data over the Internet (think telegraphs).

Putting this text into a [morse code decoder](http://morsecode.scphillips.com/translator.html) you get the text:

```INFOSECFLAGISMORSING```

## Flag

INFOSECFLAGISMORSING
