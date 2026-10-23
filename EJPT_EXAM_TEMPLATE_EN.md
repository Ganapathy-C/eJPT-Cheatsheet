# 🧭 eJPT Exam Documentation Template

> Template designed to organize information during an eJPT exam/lab where there may be multiple machines, internal networks, credentials, routes, services, and different topologies.
>
> Use it as a working notebook: document quickly, organize evidence, and avoid losing important information during enumeration, exploitation, and pivoting.

---

## 📌 General information

| Field | Value |
|---|---|
| Exam / Lab | eJPT / Practice Lab |
| Date | `DD/MM/YYYY` |
| VPN / Environment | `VPN or environment name` |
| Attacker IP | `YOUR_IP` |
| Initial network | `X.X.X.X/XX` |
| Main objective | `Enumerate, exploit, and document evidence` |
| Status | `In progress / Completed` |

---

## 🧠 Personal documentation rules

Before starting, define how you are going to work so you do not get lost during the exam:

- Save all scans in `scans/`.
- Save important screenshots in `assets/images/`.
- Save credentials and tokens in the credentials section, not scattered throughout the document.
- Document each machine separately.
- Always note which commands worked and which did not.
- Keep a list of next steps to avoid repeating tests.

---

## 🗂️ Recommended folder structure

```bash
exam-ejpt/
├── README.md
├── EJPT_EXAM_TEMPLATE.md
├── scans/
│   ├── host_1/
│   ├── host_2/
│   └── network/
├── assets/
│   └── images/
│       ├── topology.png
│       ├── host_1_nmap.png
│       └── host_1_access.png
├── loot/
│   ├── credentials.txt
│   ├── hashes.txt
│   └── notes.txt
└── exploits/
    ├── scripts/
    └── payloads/
```

---

# 1. 🗺️ Environment topology

## 1.1 Summary of detected networks

| Network | Description | Access from | Status |
|---|---|---|---|
| `X.X.X.X/XX` | Initial network | Attacker | ✅ Detected |
| `X.X.X.X/XX` | Internal network | Compromised host | ⏳ Pending |
| `X.X.X.X/XX` | Additional network | Pivoting | ❌ Not validated |

## 1.2 Topology diagram

> Insert an image or a simple network diagram here.

```text
[Attacker]
    |
    | VPN
    v
[Initial network X.X.X.X/XX]
    |
    +--> [Host 1 - X.X.X.X]
    |
    +--> [Host 2 - X.X.X.X]
              |
              v
        [Internal network X.X.X.X/XX]
```


## 1.3 General host table

| Host | IP | Approximate system | Main ports | Status | Priority |
|---|---|---|---|---|---|
| Host 1 | `X.X.X.X` | Linux/Windows | `21,22,80` | Enumerated | High |
| Host 2 | `X.X.X.X` | Linux/Windows | `445,3389` | Pending | Medium |
| Host 3 | `X.X.X.X` | Unknown | `-` | Not scanned | Low |

---

# 2. 🔎 Initial reconnaissance

## 2.1 Identify interface and own IP

```bash
ip a
ip route
```

**Notes:**

- Attacker IP: `YOUR_IP`
- VPN interface: `tun0 / tap0 / other`
- Initial target network: `X.X.X.X/XX`

## 2.2 Discover active hosts

```bash
sudo nmap -sn X.X.X.X/XX -oN scans/network/host_discovery.txt
```

| Argument | Explanation |
|---|---|
| `-sn` | Performs host discovery without port scanning. |
| `X.X.X.X/XX` | Target network range. |
| `-oN` | Saves the output in normal format. |

### Hosts found

| IP | Name/Hostname | Observations |
|---|---|---|
| `X.X.X.X` | `host.local` | Responds to ping / open ports |
| `X.X.X.X` | `unknown` | Requires enumeration |

---

# 3. 📡 Scanning by machine

> Repeat this section for each discovered host.

---

## 🖥️ Host 1 - `X.X.X.X`

### 3.1 General information

| Field | Value |
|---|---|
| IP | `X.X.X.X` |
| Hostname | `HOSTNAME` |
| Operating system | `Linux / Windows / Unknown` |
| Possible role | `Web server / SMB / FTP / AD / DB / Other` |
| Status | `Pending / Enumerated / Compromised` |

### 3.2 Quick port scan

```bash
sudo nmap -p- --min-rate 5000 -T4 X.X.X.X -oN scans/host_1/all_ports.txt
```

**Objective:** detect all open TCP ports and avoid relying only on Nmap's top 1000 ports.

### 3.3 Detailed scan

```bash
sudo nmap -sC -sV -p PORTS X.X.X.X -oN scans/host_1/service_scan.txt
```

| Argument | Explanation |
|---|---|
| `-sC` | Runs basic enumeration scripts. |
| `-sV` | Detects service versions. |
| `-p` | Indicates the specific ports found. |
| `-oN` | Saves the result for review later. |

### 3.4 Detected services

| Port | Service | Version | Possible vector | Status |
|---|---|---|---|---|
| `21` | FTP | `vsftpd / ProFTPD / other` | Anonymous / credentials | Pending |
| `22` | SSH | `OpenSSH` | Credential reuse | Pending |
| `80` | HTTP | `Apache / Nginx / IIS` | Web enumeration | Pending |
| `445` | SMB | `Samba / Windows SMB` | Shares / users | Pending |

### 3.5 Important evidence

```text
Paste banners, versions, interesting paths, or relevant findings here.
```

---

# 4. 🧩 Enumeration by service

## 4.1 FTP

```bash
ftp X.X.X.X
```

### Tests performed

| Test | Command | Result |
|---|---|---|
| Anonymous access | `anonymous:anonymous` | `Works / Does not work` |
| File listing | `ls -la` | `Files found` |
| Download | `get file.txt` | `Downloaded / Error` |

### Interesting files

| File | Path | Relevant content |
|---|---|---|
| `note.txt` | `/` | Possible users / clues |
| `backup.zip` | `/backups` | May contain credentials |

---

## 4.2 SMB

```bash
smbclient -L //X.X.X.X -N
smbclient -L //X.X.X.X -U user
```

### Shared resources

| Share | Access | User used | Observations |
|---|---|---|---|
| `public` | Read | Guest | Contains useful files |
| `users` | Read/Write | `user` | Review documents |

### Additional enumeration

```bash
enum4linux-ng X.X.X.X
crackmapexec smb X.X.X.X
crackmapexec smb X.X.X.X -u user -p password --shares
```

### SMB findings

```text
- Users found:
- Accessible shares:
- Files downloaded:
- Possible credentials:
```

---

## 4.3 HTTP / HTTPS

### Basic information

| Field | Value |
|---|---|
| URL | `http://X.X.X.X` |
| Technology | `Apache / Nginx / IIS / PHP / WordPress / Joomla / Other` |
| Main code | `200 / 301 / 403 / 500` |
| Virtual hosts | `Yes / No / Pending` |

### Web enumeration

```bash
whatweb http://X.X.X.X
nikto -h http://X.X.X.X -o scans/host_1/nikto.html -Format htm
gobuster dir -u http://X.X.X.X -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x php,txt,html -o scans/host_1/gobuster.txt
```

### Found paths

| Path | Code | Size | Interest |
|---|---|---|---|
| `/admin` | `200` | `-` | Login panel |
| `/backup` | `403` | `-` | Check bypass |
| `/uploads` | `200` | `-` | Possible file upload |

### Forms / panels

| URL | Type | Users tested | Result |
|---|---|---|---|
| `/login` | Login | `admin`, `user` | Pending |

---

## 4.4 SSH

SSH is usually not exploited directly in eJPT unless valid credentials, reused private keys, or a misconfiguration exist.

```bash
ssh user@X.X.X.X
ssh -i id_rsa user@X.X.X.X
```

| User | Method | Result |
|---|---|---|
| `user` | Password | `Access / Failure` |
| `user` | Private key | `Access / Failure` |

---

## 4.5 Databases

### MySQL / MariaDB

```bash
mysql -h X.X.X.X -u user -p
```

### MSSQL

```bash
impacket-mssqlclient domain/user:password@X.X.X.X
```

### PostgreSQL

```bash
psql -h X.X.X.X -U user
```

| Service | Credentials | Access | Interesting data |
|---|---|---|---|
| MySQL | `user:pass` | Yes/No | Users / hashes |
| MSSQL | `domain/user:pass` | Yes/No | xp_cmdshell / tables |

---

# 5. 🔐 Credentials and reuse

> This section is critical in eJPT. Many times, a credential discovered on one service is valid for another.

## 5.1 Credentials found

| User | Password / Hash | Source | Tested on | Status |
|---|---|---|---|---|
| `user1` | `password123` | FTP / file | SSH, SMB, Web | Valid in SMB |
| `admin` | `hash` | Database | Cracking pending | Pending |

## 5.2 Credential validation

```bash
crackmapexec smb X.X.X.X -u users.txt -p passwords.txt
hydra -L users.txt -P passwords.txt ssh://X.X.X.X
```

## 5.3 Reuse notes

```text
- Password X works for user Y in SMB.
- The same password does not work in SSH.
- Try credentials against the web panel.
```

---

# 6. 🚪 Initial access

## 6.1 Entry vector

| Field | Value |
|---|---|
| Compromised host | `X.X.X.X` |
| Service used | `HTTP / SMB / FTP / SSH / Other` |
| Technique | `Valid credentials / web shell upload / CVE exploitation / misconfiguration` |
| User obtained | `user` |
| Shell type | `reverse shell / bind shell / SSH / web shell` |

## 6.2 Commands used

```bash
# Paste the commands that granted access here
```

## 6.3 Shell stabilization

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
stty rows 40 columns 120
```

## 6.4 Access evidence

```bash
whoami
hostname
ip a
```

```text
Current user:
Hostname:
Internal IP:
```

---

# 7. 🧱 Internal host enumeration

## 7.1 System

```bash
whoami
id
hostname
uname -a
cat /etc/os-release
```

## 7.2 Users and permissions

```bash
cat /etc/passwd
sudo -l
find / -perm -4000 2>/dev/null
```

## 7.3 Internal network

```bash
ip a
ip route
ss -tulnp
netstat -tulnp
```

## 7.4 Interesting files

```bash
find / -name "*.txt" 2>/dev/null
find / -name "*.conf" 2>/dev/null
find / -name "*.bak" 2>/dev/null
```

### Internal findings

| Finding | Path / Command | Importance |
|---|---|---|
| Credential | `/var/www/html/config.php` | May be useful for DB |
| SUID binary | `/usr/bin/find` | Possible privilege escalation |
| Internal network | `10.10.10.0/24` | Requires pivoting |

---

# 8. 🔺 Privilege escalation

## 8.1 Summary

| Field | Value |
|---|---|
| Initial user | `www-data / user / other` |
| Final user | `root / administrator / other` |
| Technique | `sudo -l / SUID / credentials / kernel / misconfigured service` |
| Evidence | `whoami`, `id`, screenshot |

## 8.2 Checks performed

| Technique | Command | Result |
|---|---|---|
| Sudo | `sudo -l` | `Allows running X` |
| SUID | `find / -perm -4000 2>/dev/null` | `Interesting binary` |
| Credentials | `grep -Ri "password" /var/www` | `Credentials found` |
| Processes | `ps aux` | `Vulnerable service` |

## 8.3 Escalation exploitation

```bash
# Commands used to escalate privileges
```

## 8.4 Final evidence

```bash
whoami
id
hostname
```

```text
Result:
```

---

# 9. 🔁 Pivoting and internal networks

> Use this only if you discover new networks from a compromised machine.

## 9.1 Networks discovered from the compromised host

| Network | Interface | How it was discovered | Accessible |
|---|---|---|---|
| `10.10.10.0/24` | `eth1` | `ip route` | Yes/No |
| `172.16.0.0/24` | `tunnel` | `arp -a` | Pending |

## 9.2 Internal hosts found

| Internal IP | Hostname | Ports | Notes |
|---|---|---|---|
| `10.10.10.5` | `internal-host` | `80,445` | New target |

## 9.3 Proxy / Port forwarding

### Proxychains

```bash
proxychains nmap -sT -Pn -p 80,445 10.10.10.5
```

### Port redirection

```bash
portfwd add -l 8080 -p 80 -r 192.168.0.50
```

## 9.4 Pivoting notes

```text
- Internal route detected:
- Host used as pivot:
- Tunnel method:
- Reachable services:
```

---

# 10. 🧾 Evidence and answers

> In eJPT, it is important to have clear and locatable answers.

## 10.1 Evidence by host

| Host | Evidence | Command / Path | Value |
|---|---|---|---|
| `X.X.X.X` | Current user | `whoami` | `user` |
| `X.X.X.X` | Hostname | `hostname` | `machine01` |
| `X.X.X.X` | Credential | `config.php` | `user:pass` |
| `X.X.X.X` | Privileges | `sudo -l` | `file as sudo` |

## 10.2 Exam questions / answers

| No. | Question | Answer | Evidence |
|---|---|---|---|
| 1 | `Exam question` | `Answer` | `Command / screenshot` |
| 2 | `Exam question` | `Answer` | `Command / screenshot` |
| 3 | `Exam question` | `Answer` | `Command / screenshot` |

---

# 11. ✅ Control checklist

## Reconnaissance

- [ ] Attacker IP identified.
- [ ] Initial network identified.
- [ ] Active hosts discovered.
- [ ] Full port scan completed per host.
- [ ] Version and basic script scan completed.

## Enumeration

- [ ] FTP enumerated.
- [ ] SMB enumerated.
- [ ] HTTP/HTTPS enumerated.
- [ ] SSH tested with valid credentials.
- [ ] Databases reviewed if present.
- [ ] Downloaded files reviewed.

## Credentials

- [ ] Users collected.
- [ ] Passwords collected.
- [ ] Hashes saved.
- [ ] Credential reuse tested.
- [ ] Credentials documented with their source.

## Exploitation

- [ ] Initial access vector identified.
- [ ] Shell obtained.
- [ ] Shell stabilized.
- [ ] Access evidence saved.

## Privilege escalation

- [ ] Local enumeration performed.
- [ ] `sudo -l` checked.
- [ ] SUID reviewed.
- [ ] Configuration files reviewed.
- [ ] Privileges escalated if applicable.

## Pivoting

- [ ] Internal routes reviewed.
- [ ] Internal interfaces identified.
- [ ] Internal hosts discovered.
- [ ] Tunnel configured if applicable.
- [ ] Internal services enumerated.

## Closure

- [ ] Exam answers filled in.
- [ ] Evidence organized.
- [ ] Important commands saved.
- [ ] Final notes completed.

---

# 12. 🧠 Quick notes during the exam

```text
[ ] Review unusual ports.
[ ] Test credentials on all services.
[ ] Review web configuration files.
[ ] Check for another network interface.
[ ] Review internal routes.
[ ] Do not get stuck too long on a single machine.
[ ] Document each finding immediately.
```

---

# 13. 📝 Final environment summary

## Compromised machines

| Host | IP | Initial user | Final user | Main technique |
|---|---|---|---|---|
| Host 1 | `X.X.X.X` | `user` | `root` | `Credentials + sudo` |
| Host 2 | `X.X.X.X` | `www-data` | `root` | `Webshell + SUID` |

## Useful final credentials

| User | Password | Service | Host |
|---|---|---|---|
| `user` | `password` | SSH/SMB/Web | `X.X.X.X` |

## Confirmed internal routes

| Network | Access via | Relevant hosts |
|---|---|---|
| `X.X.X.X/XX` | `Host 1` | `Host 2, Host 3` |

---

# 14. 📚 Lessons learned

```text
- Which technique was most important:
- Which service gave the most information:
- What mistake cost me time:
- What should I review next time:
- Commands I want to remember:
```

---

## ⚠️ Usage note

This template is intended for personal documentation, study, and organization during authorized practical labs or exams. It does not include exam answers and does not replace the methodology: it serves to keep information organized and avoid losing important evidence.

---

## <img src="./assets/images/logoactual.jpg" width=25 style="; border-radius:50%;">  Hello! My name is Saúl Ruiz
### Cybersecurity Analyst | Offensive Security and Pentesting

![YouTube](https://img.shields.io/youtube/channel/subscribers/UCcOkvgreZrXauRHyXlii0JA)
![Followers](https://img.shields.io/github/followers/saulruizplaza)
[![Twitter Follow](https://img.shields.io/twitter/follow/plasysx?style=social)](https://twitter.com/plasysx)

I am a Cybersecurity Analyst and Higher Technician in Networked Computer Systems Administration. I am currently developing my career in SOC environments, participating in tasks of analysis, monitoring, and investigation of security events.

My main interest is oriented toward offensive security, pentesting, and technical analysis, areas in which I continue to train constantly to grow professionally within the sector.

Through my personal project <b>[@PlaSysX](https://linktr.ee/PlaSysx)</b>, I share content related to IT, cybersecurity, and hands-on learning, with the goal of adding value to those who also want to continue growing in the technological world.

[![Website](https://img.shields.io/badge/Website-plasysx.com-7B3FF2?style=for-the-badge&logo=google-chrome&logoColor=white&labelColor=101010)](https://plasysx.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Saúl_Ruiz_Plaza-0077B5?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=101010)](https://www.linkedin.com/in/saulruizplaza) [![Instagram](https://img.shields.io/badge/Instagram-@PlaSysX-E4405F?style=for-the-badge&logo=instagram&logoColor=white&labelColor=101010)](https://instagram.com/plasysx)
[![TikTok](https://img.shields.io/badge/TikTok-@plasysx_es-69C9D0?style=for-the-badge&logo=tiktok&logoColor=white&labelColor=101010)](https://tiktok.com/@plasysx_es)
[![YouTube](https://img.shields.io/badge/YouTube-Plasysx-FF0000?style=for-the-badge&logo=youtube&logoColor=white&labelColor=101010)](https://youtube.com/@Plasysx)
[![Twitter](https://img.shields.io/badge/Twitter-@plasysx-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white&labelColor=101010)](https://twitter.com/plasysx)
