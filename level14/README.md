# Level 14

## Description

A rather bland page asking if I'd like to download level14 file.  Well, OK, guess this must be the place. (file: `http://ctf.infosecinstitute.com/misc/level14`)

## Solution

Downloading this file and examining it, it looks like a database dump from a MySQL database.  Lets look at the table names:

```shell
$ grep 'CREATE TABLE' level14
CREATE TABLE IF NOT EXISTS `administrator` (
CREATE TABLE IF NOT EXISTS `ads_links` (
CREATE TABLE IF NOT EXISTS `am_commentmeta` (
CREATE TABLE IF NOT EXISTS `app_options` (
CREATE TABLE IF NOT EXISTS `articles` (
CREATE TABLE IF NOT EXISTS `flag?` (
CREATE TABLE IF NOT EXISTS `friends` (
CREATE TABLE IF NOT EXISTS `money_transfer` (
CREATE TABLE IF NOT EXISTS `wp_comments` (
CREATE TABLE IF NOT EXISTS `wp_postmeta` (
CREATE TABLE IF NOT EXISTS `wp_posts` (
CREATE TABLE IF NOT EXISTS `wp_terms` (
CREATE TABLE IF NOT EXISTS `wp_term_relationships` (
CREATE TABLE IF NOT EXISTS `wp_term_taxonomy` (
CREATE TABLE IF NOT EXISTS `wp_usermeta` (
$
```

Perhaps this is too obvious!  Lets check out the `flag?` table.

```sql
INSERT INTO `flag?` (`ID`, `user_login`, `user_pass`, `user_nicename`, `user_email`, `user_url`, `user_registered`, `user_activation_key`, `user_status`, `display_name`) VALUES
(1, 'admin', '$P$B8p.TUJAbjULMWrNXm8GsH4fb2PWfF.', 'admin', 'christyhaigcreations@gmail.com', '', '2012-09-06 20:09:55', '', 0, 'admin');
```

One of the first things I tend to do when I find something like this is pump it in to Google with quotes around it, to see if this is something obvious.  If you do that, one of the results is `http://www.tcmark.net/ss_database_backup.sql` which contains this exact same table contents as `wp_user`.  This is too good to be true.  While browsing the file I noticed that the table `friends` contains some interesting values:

```sql
INSERT INTO `friends` (`id`, `name`, `address`, `status`) VALUES
(102, 'Sasha Grey', 'Vatican City', 'Active'),
(101, 'Andres Bonifacio', 'Tondo, Manila', 'Active'),
(103, 'lol', 'what the???', 'Inactive'),
(104, '\\u0069\\u006e\\u0066\\u006f\\u0073\\u0065\\u0063\\u005f\\u0066\\u006c\\u0061\\u0067\\u0069\\u0073\\u005f\\u0077\\u0068\\u0061\\u0074\\u0073\\u006f\\u0072\\u0063\\u0065\\u0072\\u0079\\u0069\\u0073\\u0074\\u0068\\u0069\\u0073', 'annoying', '0x0a');
```

Sasha Grey lives in the Vatican City?  That doesn't seem right.  Wonder what that unicode string is?

Starting up `python`, we can break up this string and parse it:

```python
>>> s = '\\u0069\\u006e\\u0066\\u006f\\u0073\\u0065\\u0063\\u005f\\u0066\\u006c\\u0061\\u0067\\u0069\\u0073\\u005f\\u0077\\u0068\\u0061\\u0074\\u0073\\u006f\\u0072\\u0063\\u0065\\u0072\\u0079\\u0069\\u0073\\u0074\\u0068\\u0069\\u0073'
>>> s = s.replace('\\u00', '')
>>> hexes = [s[i:i+2] for i in range(0, len(s), 2)]
>>> ''.join([chr(int(x,16)) for x in hexes])
'infosec_flagis_whatsorceryisthis'
>>>
```

## Flag

`infosec_flagis_whatsorceryisthis`

