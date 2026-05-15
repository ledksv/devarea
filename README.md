# Devarea — HackTheBox

**Platform:** HackTheBox

## Enumeration

```
nmap -sC -sV -p- <TARGET_IP>

PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.5 (anonymous login allowed)
22/tcp   open  ssh     OpenSSH 9.6p1 Ubuntu
80/tcp   open  http    Apache httpd 2.4.58
8080/tcp open  http    Jetty 9.4.27.v20200227
8500/tcp open  http    Golang net/http server (proxy)
8888/tcp open  http    Golang net/http server (Hoverfly Dashboard)
```

Added `devarea.htb` to `/etc/hosts` and proceeded with further enumeration.

## 🔒 Walkthrough Locked

This machine is still active on HackTheBox. The full writeup will be published upon retirement.

Full walkthrough available at [l3dsec.vercel.app](https://l3dsec.vercel.app) upon retirement.
