# Level 13

## Description

A page with the message:

>

## Solution

Guessing at some common backup filenames, (.bak, etc), we can find the original PHP code at: `http://ctf.infosecinstitute.com/levelthirteen.php.old`

If we compare this to the original file, we can see some comments are now missing:

```diff
--- levelthirteen.php   2015-03-15 16:49:37.798172357 -0400
+++ levelthirteen.php.old       2015-03-15 16:49:34.082172505 -0400
@@ -81,12 +81,24 @@
     I'm sorry for messing up :(

     </h1>
-
+    <?php

+    /* <img src="img/clippy1.jpg" class="imahe" /> <br /> <br />
+
+    <p>Do you want to download this mysterious file?</p>
+
+    <a href="misc/imadecoy">
+      <button class="btn">Yes</button>
+    </a>
+
+    <a href="index.php">
+      <button class="btn">No</button>
+    </a>
+    */
+
+    ?>
   </div>

-<center> <br /><br /><br /><p style="font-size:.9em;font-weight:normal;">Bounty: $130</p></center>
-
   <script src="js/jquery.js"></script>
   <script src="js/bootstrap.min.js"></script>
   <script src="js/custom.js"></script>
```

So, let's fetch the mysterious file!

```shell
$ curl -s -O http://ctf.infosecinstitute.com/misc/imadecoy
$ file imadecoy
imadecoy: tcpdump capture file (little-endian) - version 2.4 (Linux "cooked", capture length 65535)
$
```

Cool, a tcpdump file.  TCPDUMP is a *ix utility that allows you to snoop on network traffic on a machine.  We can open this file in [Wireshark](https://www.wireshark.org/).

## Wireshark
