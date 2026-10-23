# 🧪 eJPT Documentation Template / Pentesting Labs

> Template created to document practices, labs, vulnerable machines, and preparation exercises for eJPT in an organized, professional, and reusable way.
>
> Use it to study, create private write-ups, reinforce methodology, and build technical evidence without depending on a specific machine.

---

## 📌 General information

| Field | Information |
|---|---|
| **Lab name** | `[Lab / machine / scenario name]` |
| **Platform** | `[INE / TryHackMe / Hack The Box / DockerLab / Local / Other]` |
| **Estimated difficulty** | `[Easy / Medium / Hard]` |
| **Target system** | `[Linux / Windows / Web / Network / Mixed]` |
| **Target IP** | `[X.X.X.X]` |
| **Date** | `[DD/MM/YYYY]` |
| **Author** | `Saúl Ruiz` |
| **Status** | `[In progress / Completed / Reviewed]` |

---

## 🎯 Lab objective

Briefly describe what this lab aims to practice.

Example:

> The objective of this lab is to practice a basic pentesting methodology oriented toward eJPT, including reconnaissance, service enumeration, vulnerability identification, controlled exploitation, initial access, and privilege escalation.

### Skills worked on

- `[Network reconnaissance]`
- `[Service enumeration]`
- `[Web analysis]`
- `[Vulnerability exploitation]`
- `[Controlled brute force]`
- `[File transfer]`
- `[Linux/Windows privilege escalation]`
- `[Pivoting / routing / port forwarding, if applicable]`
- `[Technical documentation]`

---

## 🧭 Methodology used

During the lab, a structured methodology will be followed to avoid skipping important phases:

1. **Environment preparation**
2. **Host discovery**
3. **Port and service scanning**
4. **Specific enumeration per service**
5. **Analysis of possible vulnerabilities**
6. **Controlled exploitation**
7. **Initial access**
8. **Internal enumeration**
9. **Privilege escalation**
10. **Basic post-exploitation and evidence gathering**
11. **Conclusions and learning**

---

## 🧱 Environment preparation

Before starting, ensure that the lab environment is correctly configured.

### Environment data

| Element | Value |
|---|---|
| **Attacker machine** | `[Kali Linux / Parrot / Ubuntu]` |
| **Network interface** | `[tun0 / eth0 / docker0 / other]` |
| **Lab network** | `[X.X.X.0/24]` |
| **Main tools** | `[nmap, gobuster, smbclient, hydra, metasploit, netcat, linpeas...]` |

### Initial checks

```bash
ip a
ip route
ping -c 1 <IP_OBJECTIVE>
```

**Explanation:**

- `ip a` allows checking the available network interfaces.
- `ip route` helps verify the active routes.
- `ping` is used to confirm basic connectivity with the target, although some systems may block ICMP.

### Evidence

```text
[Add relevant output or screenshot here]
```


---

## 🔎 Phase 1: Host discovery

This phase is used to identify which machines are active within the network range.

### Command used

```bash
nmap -sn <NETWORK_RANGE>
```

### Explanation

The `-sn` parameter performs host discovery without scanning ports. It is useful for detecting active systems before carrying out more detailed scans.

| Argument | Meaning |
|---|---|
| `-sn` | Performs host discovery without port scanning |
| `<NETWORK_RANGE>` | Target range, for example `192.168.1.0/24` |

### Result

```text
Discovered host: <TARGET_IP>
```

### Notes

- If there is no ICMP response, try alternative techniques.
- In eJPT-style labs, it may be necessary to identify several machines within a network.
- Always save the IPs found for later phases.


---

## 🚪 Phase 2: Port scanning

Once the target has been identified, a port scan is performed to discover exposed services.

### Initial scan

```bash
nmap -p- --open -T4 <TARGET_IP> -oN scans/all_ports.txt
```

### Explanation

This scan looks for all open TCP ports on the target. Saving the result allows it to be consulted later without repeating the scan.

| Argument | Meaning |
|---|---|
| `-p-` | Scans all TCP ports, from 1 to 65535 |
| `--open` | Shows mainly open ports |
| `-T4` | Increases scanning speed in controlled environments |
| `-oN` | Saves the output in normal format |

### Ports found

| Port | Service | Status | Observations |
|---|---|---|---|
| `[21]` | `[FTP]` | `[open]` | `[Anonymous login / version / note]` |
| `[22]` | `[SSH]` | `[open]` | `[OpenSSH version]` |
| `[80]` | `[HTTP]` | `[open]` | `[Web detected]` |
| `[445]` | `[SMB]` | `[open]` | `[Shared resources]` |


---

## 🧬 Phase 3: Version detection and basic scripts

With the detected ports, a more specific scan is performed to obtain versions, banners, and common script results.

### Command used

```bash
nmap -sC -sV -p <PORTS> <TARGET_IP> -oN scans/services.txt
```

### Explanation

This command helps determine the active services more accurately and can reveal information such as vulnerable versions, insecure configurations, or anonymous access.

| Argument | Meaning |
|---|---|
| `-sC` | Runs default NSE scripts |
| `-sV` | Detects service versions |
| `-p` | Specifies the ports to analyze |
| `-oN` | Saves the output to a file |

### Summary result

```text
[Add the services, versions, and relevant data here]
```

### Possible findings

- `[Service with anonymous access]`
- `[Vulnerable version]`
- `[Banner with sensitive information]`
- `[Misconfigured service]`
- `[Exposed path or resource]`


---

## 📚 Phase 4: Enumeration by service

In this section, the specific enumeration of each discovered service is documented.

---

### 🌐 HTTP / HTTPS enumeration

Use this section if there is a web service on ports such as `80`, `443`, `8080`, `8000`, etc.

#### Initial check

```bash
whatweb http://<TARGET_IP>
curl -I http://<TARGET_IP>
```

**Explanation:**

- `whatweb` identifies web technologies.
- `curl -I` shows HTTP headers.

#### Directory enumeration

```bash
gobuster dir -u http://<TARGET_IP> -w /usr/share/wordlists/dirb/common.txt -x php,txt,html -o scans/gobuster.txt
```

| Argument | Meaning |
|---|---|
| `dir` | Directory enumeration mode |
| `-u` | Target URL |
| `-w` | Wordlist used |
| `-x` | Extensions to test |
| `-o` | Saves the output |

#### Relevant results

| Path | Code | Observation |
|---|---|---|
| `/admin` | `[200/301/403]` | `[Panel / restricted resource]` |
| `/uploads` | `[200]` | `[Possible file upload]` |
| `/robots.txt` | `[200]` | `[Interesting routes]` |


---

### 📁 FTP enumeration

Use this section if port `21` is open.

#### Anonymous access

```bash
ftp <TARGET_IP>
# User: anonymous
# Password: anonymous
```

#### Useful commands inside FTP

```bash
ls
pwd
get <file>
put <file>
bye
```

### Explanation

FTP can expose sensitive files, credentials, backups, or internal notes. If anonymous access is allowed, it is an important finding that should be documented.

#### Evidence found

```text
[Downloaded files]
[Relevant content]
[Credentials or clues found]
```


---

### 🧩 SMB enumeration

Use this section if ports `139` or `445` are open.

#### List shared resources

```bash
smbclient -L //<TARGET_IP>/ -N
smbclient -L //<TARGET_IP>/ -U <USER>
```

#### Access a shared resource

```bash
smbclient //<TARGET_IP>/<RESOURCE> -U <USER>
```

#### Useful commands inside SMB

```bash
ls
cd <directory>
get <file>
put <file>
exit
```

### Explanation

SMB can reveal shared folders, insecure permissions, internal files, or reusable credentials. In eJPT, it is important to enumerate SMB with and without credentials.

#### Resources found

| Resource | Permissions | Observations |
|---|---|---|
| `[share]` | `[READ / WRITE]` | `[Relevant information]` |
| `[users]` | `[READ]` | `[Possible users]` |


---

### 🔐 SSH enumeration

Use this section if port `22` is open.

#### Version check

```bash
nmap -sV -p 22 <TARGET_IP>
```

#### SSH access

```bash
ssh <USER>@<TARGET_IP>
ssh -i <PRIVATE_KEY> <USER>@<TARGET_IP>
```

### Explanation

SSH is often a route to take when valid credentials, private keys, or reused usernames are obtained from other services.

#### Credentials tested

| User | Password / key | Result |
|---|---|---|
| `[user]` | `[password]` | `[valid/invalid]` |


---

### 🗄️ Database enumeration

Use this section if services such as MySQL, PostgreSQL, MSSQL, or Redis are detected.

#### MySQL

```bash
mysql -h <TARGET_IP> -u <USER> -p
```

#### PostgreSQL

```bash
psql -h <TARGET_IP> -U <USER>
```

#### MSSQL

```bash
impacket-mssqlclient <DOMAIN>/<USER>:<PASSWORD>@<TARGET_IP>
```

### Explanation

Databases may contain credentials, hashes, users, internal paths, or information useful for escalating access.

#### Information found

```text
[Databases]
[Relevant tables]
[Credentials]
[Hashes]
```


---

## 🔍 Phase 5: Vulnerability analysis

In this phase, the collected information is linked to possible attack vectors.

### Identified findings

| ID | Finding | Service | Risk | Evidence |
|---|---|---|---|---|
| `H-01` | `[Anonymous FTP access]` | `FTP` | `[Medium]` | `[Screenshot / output]` |
| `H-02` | `[Exposed credentials]` | `[HTTP/SMB]` | `[High]` | `[File found]` |
| `H-03` | `[Vulnerable version]` | `[Service]` | `[High]` | `[nmap / exploit-db]` |

### Search for exploits or references

```bash
searchsploit <SERVICE> <VERSION>
```

### Explanation

Not every finding is directly exploitable. In this phase, priority is given to what is most likely to provide access or useful information.

### Chosen vector

```text
[Explain why this vector is chosen and not another]
```


---

## ⚔️ Phase 6: Controlled exploitation

In this phase, the vector used to obtain initial access or extract sensitive information is documented.

> [!IMPORTANT]
> The entire process must be performed only in authorized environments, private labs, or practice platforms.

### Exploitation vector

```text
[Service or vulnerability used]
```

### Command / procedure

```bash
[command used]
```

### Explanation

```text
[Explain what the command does, why it works, and what result is expected]
```

### Result obtained

```text
[Credentials / shell / sensitive file / token / hash / access]
```


---

## 🖥️ Phase 7: Initial access

Once a valid vector is obtained, it is documented how access to the system is achieved.

### Type of access obtained

- `[Reverse shell]`
- `[SSH access]`
- `[Web panel]`
- `[Meterpreter session]`
- `[Valid credentials]`
- `[Other]`

### Command used

```bash
[access command]
```

### Shell stabilization, if applicable

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
stty rows 40 columns 120
```

### Explanation

A stable shell allows for better work inside the system, using interactive commands and avoiding problems with editors, `sudo`, autocompletion, or keyboard signals.

### Compromised user

```bash
whoami
hostname
id
```

```text
User: [user]
Host: [hostname]
Groups: [groups]
```


---

## 🧾 Phase 8: Internal enumeration

After initial access, system information is gathered to look for privilege escalation paths.

### Basic system information

```bash
whoami
id
hostname
uname -a
cat /etc/os-release
```

### Users and directories

```bash
cat /etc/passwd
ls -la /home
find /home -type f -name "*.txt" 2>/dev/null
```

### Sudo permissions

```bash
sudo -l
```

### SUID binaries

```bash
find / -perm -4000 -type f 2>/dev/null
```

### Capabilities

```bash
getcap -r / 2>/dev/null
```

### Scheduled tasks

```bash
cat /etc/crontab
ls -la /etc/cron.*
```

### Processes and internal ports

```bash
ps aux
ss -tulnp
```

### Interesting files

```bash
find / -name "*.bak" 2>/dev/null
find / -name "*.conf" 2>/dev/null
find / -name "*password*" 2>/dev/null
```

### Explanation

Internal enumeration is one of the most important phases. The goal is to find misconfigurations, credentials, insecure permissions, exploitable binaries, or internal services not visible from outside.


---

## 🧗 Phase 9: Privilege escalation

In this phase, it is documented how access is moved from a limited user to one with greater privileges.

### Identified escalation vector

```text
[Misconfigured sudo permission / SUID / reused credentials / vulnerable service / cron task / PATH hijacking / other]
```

### Evidence of the vector

```bash
[command demonstrating the vector]
```

### Exploitation of the vector

```bash
[command used to escalate privileges]
```

### Explanation

```text
[Explain why this vector allows privilege escalation]
```

### Privilege confirmation

```bash
whoami
id
```

```text
Final user: [root / Administrator / privileged user]
```


---

## 🏁 Phase 10: Final evidence

In this section, the main compromise tests of the lab are documented.

### Flags / relevant files

| File | Path | Content / evidence |
|---|---|---|
| `user.txt` | `[path]` | `[value or screenshot]` |
| `root.txt` | `[path]` | `[value or screenshot]` |
| `[other]` | `[path]` | `[value or screenshot]` |

### Final screenshots



---

## 🔁 Pivoting / internal networks, if applicable

Use this section only if the lab includes several networks or internal machines.

### Network information found

```bash
ip a
ip route
arp -a
```

### Internal hosts detected

| Internal IP | Service | Observation |
|---|---|---|
| `[IP]` | `[Service]` | `[Description]` |

### Techniques used

- `[Port forwarding]`
- `[Proxychains]`
- `[Chisel]`
- `[SSH tunneling]`
- `[Metasploit autoroute]`

### Commands

```bash
[commands used]
```

### Explanation

Pivoting allows access to networks or services that are not directly reachable from the attacker machine by using a compromised machine as an intermediate hop.


---

## 🧹 Lab cleanup

At the end, the actions taken to properly close the environment are documented.

```bash
[command to stop container / close VPN / remove temporary files]
```

### Notes

- Stop machines or containers that are no longer used.
- Save important screenshots and results.
- Remove temporary credentials or generated keys if they are not needed.
- Organize the evidence files.


---

## 🧠 Technical summary

### Attack chain

```text
[Discovery] → [Enumeration] → [Finding] → [Exploitation] → [Initial access] → [Internal enumeration] → [Privilege escalation]
```

### Summary in one sentence

> In this lab, the system was compromised using `[initial vector]`, obtaining access as `[initial user]` and escalating privileges to `[final user]` via `[escalation vector]`.

### Summary table

| Phase | Result |
|---|---|
| Discovery | `[Target IP identified]` |
| Open ports | `[21, 22, 80, 445...]` |
| Vulnerable service | `[Service]` |
| Initial access | `[User / method]` |
| Escalation | `[Vector]` |
| Final privileges | `[root / Administrator]` |

---

## 📖 Lessons learned

Document the most important things you learned here.

- `[Lesson 1]`
- `[Lesson 2]`
- `[Lesson 3]`

### Mistakes made

- `[Error or blockage encountered]`
- `[How it was resolved]`

### Topics to review

- `[Service or technique to study more]`
- `[Command to memorize]`
- `[Concept to practice]`

---

## 🧰 Useful lab commands

```bash
# Discovery
nmap -sn <RANGE>

# Full scan
nmap -p- --open -T4 <IP> -oN scans/all_ports.txt

# Versions and scripts
nmap -sC -sV -p <PORTS> <IP> -oN scans/services.txt

# Web
whatweb http://<IP>
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirb/common.txt -x php,txt,html

# FTP
ftp <IP>

# SMB
smbclient -L //<IP>/ -N
smbclient //<IP>/<SHARE> -U <USER>

# SSH
ssh <USER>@<IP>
ssh -i id_rsa <USER>@<IP>

# Linux privesc
sudo -l
find / -perm -4000 -type f 2>/dev/null
getcap -r / 2>/dev/null
```

---

## 🗂️ Recommended repository structure

```text
lab-name/
├── README.md
├── scans/
│   ├── all_ports.txt
│   ├── services.txt
│   └── gobuster.txt
├── loot/
│   ├── credentials.txt
│   ├── hashes.txt
│   └── notes.txt
├── exploits/
│   └── exploit.py
└── assets/
    └── images/
        ├── 01-environment-preparation.png
        ├── 02-host-discovery.png
        ├── 03-port-scan.png
        └── ...
```

---

## ✅ eJPT checklist to document labs

- [ ] I identified the target IP.
- [ ] I scanned all TCP ports.
- [ ] I detected service versions.
- [ ] I enumerated each relevant service.
- [ ] I saved the important results.
- [ ] I documented users, credentials, or clues found.
- [ ] I explained the exploitation vector.
- [ ] I documented the initial access.
- [ ] I performed internal enumeration.
- [ ] I explained privilege escalation.
- [ ] I added final evidence.
- [ ] I wrote lessons learned.

---

## <img src="./assets/images/logoactual.jpg" width=25 style="; border-radius:50%;">  Hello! My name is Saúl Ruiz
### Cybersecurity Analyst | Offensive Security and Pentesting

![YouTube](https://img.shields.io/youtube/channel/subscribers/UCcOkvgreZrXauRHyXlii0JA)
![Followers](https://img.shields.io/github/followers/saulruizplaza)
[![Twitter Follow](https://img.shields.io/twitter/follow/plasysx?style=social)](https://twitter.com/plasysx)

I am a Cybersecurity Analyst and Higher Technician in Networked Computer Systems Administration. I am currently developing my career in SOC environments, participating in analysis, monitoring, and investigation of security events.

My main interest is oriented toward offensive security, pentesting, and technical analysis, areas in which I continue to train constantly to grow professionally within the sector.

Through my personal project <b>[@PlaSysX](https://linktr.ee/PlaSysx)</b>, I share content related to IT, cybersecurity, and hands-on learning, with the goal of adding value to those who also want to continue growing in the technological world.

[![Website](https://img.shields.io/badge/Website-plasysx.com-7B3FF2?style=for-the-badge&logo=google-chrome&logoColor=white&labelColor=101010)](https://plasysx.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Saúl_Ruiz_Plaza-0077B5?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=101010)](https://www.linkedin.com/in/saulruizplaza) [![Instagram](https://img.shields.io/badge/Instagram-@PlaSysX-E4405F?style=for-the-badge&logo=instagram&logoColor=white&labelColor=101010)](https://instagram.com/plasysx)
[![TikTok](https://img.shields.io/badge/TikTok-@plasysx_es-69C9D0?style=for-the-badge&logo=tiktok&logoColor=white&labelColor=101010)](https://tiktok.com/@plasysx_es)
[![YouTube](https://img.shields.io/badge/YouTube-Plasysx-FF0000?style=for-the-badge&logo=youtube&logoColor=white&labelColor=101010)](https://youtube.com/@Plasysx)
[![Twitter](https://img.shields.io/badge/Twitter-@plasysx-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white&labelColor=101010)](https://twitter.com/plasysx)
