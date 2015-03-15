# Level 8

## Description

This page contains a simple prompt that asks if I want to download `app.exe`.

## Solution

Downloading this file, and running `strings` against it yields our flag:

```shell
$ strings app.exe| grep infosec_flag
infosec_flagis_0x1a
$
```

## Flag

`infosec_flagis_0x1a`

