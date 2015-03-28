# Level 9

## Description

In this level, we are presented with a login dialog with the title "CISCO IDS WEB LOGIN SYSTEM".

## Solution

If we search Google for "Cisco IDS default password", the first hit summary mentions a default username of `netrangr` and `root`, and a default password of `attack`.  Google is nice enough to show this in the page summary, too, so we don't even have to click through. (see: http://www.cisco.com/c/en/us/support/docs/security/ips-4200-series-sensors/13837-34.html)

If we enter `root` in the username field and `attack`, we get a popup that says: `ssaptluafed_sigalf_cesofni`, which if we reverse gives us our flag.

Also, as mentioned in the *Bonus* section in the top level write-up, you can use `level15` to solve this level by viewing the source code of the page.

## Flag

`infosec_flagis_defaultpass`
