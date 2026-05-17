# DevArea — HackTheBox

**Platform:** HackTheBox

**Status: Active — walkthrough locked pending machine retirement.**

## Enumeration

```
nmap -sC -sV -p- <TARGET_IP>

PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.5 (anonymous login allowed)
22/tcp   open  ssh     OpenSSH 9.6p1 Ubuntu
80/tcp   open  http    Apache httpd 2.4.58
8080/tcp open  http    Jetty 9.4.27.v20200227
8500/tcp open  http    Golang net/http server (proxy)
8888/tcp open  http    Golang net/http server
```

Added `devarea.htb` to `/etc/hosts` and proceeded with further enumeration.

## Full Walkthrough

Full writeup published at [Devarea — L3dSec](https://l3dsec.vercel.app/walkthroughs/devarea-htb)

---

> ⚠️ For educational purposes only. Only test systems you own or have explicit written permission to test.
