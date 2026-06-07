# 🧭 Plantilla de Documentación para Examen eJPT

> Plantilla pensada para organizar la información durante un examen/laboratorio eJPT donde pueden existir varias máquinas, redes internas, credenciales, rutas, servicios y topologías diferentes.
>
> Úsala como cuaderno de trabajo: documenta rápido, ordena evidencias y evita perder información importante durante la enumeración, explotación y pivoting.

---

## 📌 Información general

| Campo | Valor |
|---|---|
| Examen / Laboratorio | eJPT / Lab de práctica |
| Fecha | `DD/MM/YYYY` |
| VPN / Entorno | `Nombre de la VPN o entorno` |
| IP de atacante | `TU_IP` |
| Red inicial | `X.X.X.X/XX` |
| Objetivo principal | `Enumerar, explotar y documentar evidencias` |
| Estado | `En progreso / Finalizado` |

---

## 🧠 Reglas personales de documentación

Antes de empezar, define cómo vas a trabajar para no perderte durante el examen:

- Guardar todos los escaneos en `scans/`.
- Guardar capturas importantes en `assets/images/`.
- Guardar credenciales y tokens en la sección de credenciales, no dispersos por el documento.
- Documentar cada máquina por separado.
- Anotar siempre qué comandos funcionaron y cuáles no.
- Mantener una lista de próximos pasos para no repetir pruebas.

---

## 🗂️ Estructura recomendada de carpetas

```bash
exam-ejpt/
├── README.md
├── EXAM_TEMPLATE.md
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

# 1. 🗺️ Topología del entorno

## 1.1 Resumen de redes detectadas

| Red | Descripción | Acceso desde | Estado |
|---|---|---|---|
| `X.X.X.X/XX` | Red inicial | Atacante | ✅ Detectada |
| `X.X.X.X/XX` | Red interna | Host comprometido | ⏳ Pendiente |
| `X.X.X.X/XX` | Red adicional | Pivoting | ❌ No validada |

## 1.2 Diagrama de topología

> Inserta aquí una imagen o esquema simple de la red.

```text
[Atacante]
    |
    | VPN
    v
[Red inicial X.X.X.X/XX]
    |
    +--> [Host 1 - X.X.X.X]
    |
    +--> [Host 2 - X.X.X.X]
              |
              v
        [Red interna X.X.X.X/XX]
```


## 1.3 Tabla general de hosts

| Host | IP | Sistema aproximado | Puertos principales | Estado | Prioridad |
|---|---|---|---|---|---|
| Host 1 | `X.X.X.X` | Linux/Windows | `21,22,80` | Enumerado | Alta |
| Host 2 | `X.X.X.X` | Linux/Windows | `445,3389` | Pendiente | Media |
| Host 3 | `X.X.X.X` | Desconocido | `-` | No escaneado | Baja |

---

# 2. 🔎 Reconocimiento inicial

## 2.1 Identificación de interfaz e IP propia

```bash
ip a
ip route
```

**Notas:**

- IP atacante: `TU_IP`
- Interfaz VPN: `tun0 / tap0 / otra`
- Red objetivo inicial: `X.X.X.X/XX`

## 2.2 Descubrimiento de hosts activos

```bash
sudo nmap -sn X.X.X.X/XX -oN scans/network/host_discovery.txt
```

| Argumento | Explicación |
|---|---|
| `-sn` | Realiza descubrimiento de hosts sin escanear puertos. |
| `X.X.X.X/XX` | Rango de red objetivo. |
| `-oN` | Guarda la salida en formato normal. |

### Hosts encontrados

| IP | Nombre/Hostname | Observaciones |
|---|---|---|
| `X.X.X.X` | `host.local` | Responde a ping / puertos abiertos |
| `X.X.X.X` | `desconocido` | Requiere enumeración |

---

# 3. 📡 Escaneo por máquina

> Repite esta sección por cada host encontrado.

---

## 🖥️ Host 1 - `X.X.X.X`

### 3.1 Información general

| Campo | Valor |
|---|---|
| IP | `X.X.X.X` |
| Hostname | `HOSTNAME` |
| Sistema operativo | `Linux / Windows / Desconocido` |
| Rol posible | `Servidor web / SMB / FTP / AD / DB / Otro` |
| Estado | `Pendiente / Enumerado / Comprometido` |

### 3.2 Escaneo rápido de puertos

```bash
sudo nmap -p- --min-rate 5000 -T4 X.X.X.X -oN scans/host_1/all_ports.txt
```

**Objetivo:** detectar todos los puertos TCP abiertos para no depender solo del top 1000 de Nmap.

### 3.3 Escaneo detallado

```bash
sudo nmap -sC -sV -p PUERTOS X.X.X.X -oN scans/host_1/service_scan.txt
```

| Argumento | Explicación |
|---|---|
| `-sC` | Ejecuta scripts básicos de enumeración segura. |
| `-sV` | Detecta versiones de servicios. |
| `-p` | Indica los puertos concretos encontrados. |
| `-oN` | Guarda el resultado para revisarlo después. |

### 3.4 Servicios detectados

| Puerto | Servicio | Versión | Posible vector | Estado |
|---|---|---|---|---|
| `21` | FTP | `vsftpd / ProFTPD / otro` | Anonymous / credenciales | Pendiente |
| `22` | SSH | `OpenSSH` | Reutilización de credenciales | Pendiente |
| `80` | HTTP | `Apache / Nginx / IIS` | Web enum | Pendiente |
| `445` | SMB | `Samba / Windows SMB` | Shares / usuarios | Pendiente |

### 3.5 Evidencias importantes

```text
Pegar aquí banners, versiones, rutas interesantes o hallazgos relevantes.
```

---

# 4. 🧩 Enumeración por servicios

## 4.1 FTP

```bash
ftp X.X.X.X
```

### Pruebas realizadas

| Prueba | Comando | Resultado |
|---|---|---|
| Acceso anónimo | `anonymous:anonymous` | `Funciona / No funciona` |
| Listado de archivos | `ls -la` | `Archivos encontrados` |
| Descarga | `get archivo.txt` | `Descargado / Error` |

### Archivos interesantes

| Archivo | Ruta | Contenido relevante |
|---|---|---|
| `nota.txt` | `/` | Posibles usuarios / pistas |
| `backup.zip` | `/backups` | Puede contener credenciales |

---

## 4.2 SMB

```bash
smbclient -L //X.X.X.X -N
smbclient -L //X.X.X.X -U usuario
```

### Recursos compartidos

| Share | Acceso | Usuario usado | Observaciones |
|---|---|---|---|
| `public` | Lectura | Invitado | Contiene archivos útiles |
| `users` | Lectura/Escritura | `usuario` | Revisar documentos |

### Enumeración adicional

```bash
enum4linux-ng X.X.X.X
crackmapexec smb X.X.X.X
crackmapexec smb X.X.X.X -u usuario -p contraseña --shares
```

### Hallazgos SMB

```text
- Usuarios encontrados:
- Shares accesibles:
- Archivos descargados:
- Posibles credenciales:
```

---

## 4.3 HTTP / HTTPS

### Información básica

| Campo | Valor |
|---|---|
| URL | `http://X.X.X.X` |
| Tecnología | `Apache / Nginx / IIS / PHP / WordPress / Joomla / Otra` |
| Código principal | `200 / 301 / 403 / 500` |
| Virtual hosts | `Sí / No / Pendiente` |

### Enumeración web

```bash
whatweb http://X.X.X.X
nikto -h http://X.X.X.X -o scans/host_1/nikto.html -Format htm
gobuster dir -u http://X.X.X.X -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x php,txt,html -o scans/host_1/gobuster.txt
```

### Rutas encontradas

| Ruta | Código | Tamaño | Interés |
|---|---|---|---|
| `/admin` | `200` | `-` | Panel de login |
| `/backup` | `403` | `-` | Revisar bypass |
| `/uploads` | `200` | `-` | Posible subida de archivos |

### Formularios / paneles

| URL | Tipo | Usuarios probados | Resultado |
|---|---|---|---|
| `/login` | Login | `admin`, `user` | Pendiente |

---

## 4.4 SSH

SSH normalmente no se explota directamente en eJPT salvo que existan credenciales válidas, claves privadas reutilizadas o una mala configuración.

```bash
ssh usuario@X.X.X.X
ssh -i id_rsa usuario@X.X.X.X
```

| Usuario | Método | Resultado |
|---|---|---|
| `usuario` | Contraseña | `Acceso / Fallo` |
| `usuario` | Clave privada | `Acceso / Fallo` |

---

## 4.5 Bases de datos

### MySQL / MariaDB

```bash
mysql -h X.X.X.X -u usuario -p
```

### MSSQL

```bash
impacket-mssqlclient dominio/usuario:contraseña@X.X.X.X
```

### PostgreSQL

```bash
psql -h X.X.X.X -U usuario
```

| Servicio | Credenciales | Acceso | Datos interesantes |
|---|---|---|---|
| MySQL | `user:pass` | Sí/No | Usuarios / hashes |
| MSSQL | `domain/user:pass` | Sí/No | xp_cmdshell / tablas |

---

# 5. 🔐 Credenciales y reutilización

> Esta sección es crítica en eJPT. Muchas veces una credencial encontrada en un servicio sirve para otro.

## 5.1 Credenciales encontradas

| Usuario | Contraseña / Hash | Origen | Probado en | Estado |
|---|---|---|---|---|
| `usuario1` | `password123` | FTP / archivo | SSH, SMB, Web | Válida en SMB |
| `admin` | `hash` | Base de datos | Crackeo pendiente | Pendiente |

## 5.2 Comprobación de credenciales

```bash
crackmapexec smb X.X.X.X -u users.txt -p passwords.txt
hydra -L users.txt -P passwords.txt ssh://X.X.X.X
```

## 5.3 Notas de reutilización

```text
- La contraseña X funciona para el usuario Y en SMB.
- La misma contraseña no funciona en SSH.
- Probar credenciales contra panel web.
```

---

# 6. 🚪 Acceso inicial

## 6.1 Vector de entrada

| Campo | Valor |
|---|---|
| Host comprometido | `X.X.X.X` |
| Servicio usado | `HTTP / SMB / FTP / SSH / Otro` |
| Técnica | `Credenciales válidas / subida webshell / explotación CVE / mala config` |
| Usuario obtenido | `usuario` |
| Tipo de shell | `reverse shell / bind shell / SSH / web shell` |

## 6.2 Comandos usados

```bash
# Pegar aquí los comandos que dieron acceso
```

## 6.3 Estabilización de shell

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
stty rows 40 columns 120
```

## 6.4 Evidencia de acceso

```bash
whoami
hostname
ip a
```

```text
Usuario actual:
Hostname:
IP interna:
```

---

# 7. 🧱 Enumeración interna del host

## 7.1 Sistema

```bash
whoami
id
hostname
uname -a
cat /etc/os-release
```

## 7.2 Usuarios y permisos

```bash
cat /etc/passwd
sudo -l
find / -perm -4000 2>/dev/null
```

## 7.3 Red interna

```bash
ip a
ip route
ss -tulnp
netstat -tulnp
```

## 7.4 Archivos interesantes

```bash
find / -name "*.txt" 2>/dev/null
find / -name "*.conf" 2>/dev/null
find / -name "*.bak" 2>/dev/null
```

### Hallazgos internos

| Hallazgo | Ruta / Comando | Importancia |
|---|---|---|
| Credencial | `/var/www/html/config.php` | Puede servir para DB |
| Binario SUID | `/usr/bin/find` | Posible escalada |
| Red interna | `10.10.10.0/24` | Requiere pivoting |

---

# 8. 🔺 Escalada de privilegios

## 8.1 Resumen

| Campo | Valor |
|---|---|
| Usuario inicial | `www-data / user / otro` |
| Usuario final | `root / administrator / otro` |
| Técnica | `sudo -l / SUID / credenciales / kernel / servicio mal configurado` |
| Evidencia | `whoami`, `id`, captura |

## 8.2 Comprobaciones realizadas

| Técnica | Comando | Resultado |
|---|---|---|
| Sudo | `sudo -l` | `Permite ejecutar X` |
| SUID | `find / -perm -4000 2>/dev/null` | `Binario interesante` |
| Credenciales | `grep -Ri "password" /var/www` | `Credenciales encontradas` |
| Procesos | `ps aux` | `Servicio vulnerable` |

## 8.3 Explotación de la escalada

```bash
# Comandos utilizados para escalar privilegios
```

## 8.4 Evidencia final

```bash
whoami
id
hostname
```

```text
Resultado:
```

---

# 9. 🔁 Pivoting y redes internas

> Úsalo solo si descubres nuevas redes desde una máquina comprometida.

## 9.1 Redes descubiertas desde host comprometido

| Red | Interfaz | Cómo se descubrió | Accesible |
|---|---|---|---|
| `10.10.10.0/24` | `eth1` | `ip route` | Sí/No |
| `172.16.0.0/24` | `tunel` | `arp -a` | Pendiente |

## 9.2 Hosts internos encontrados


| IP interna | Hostname | Puertos | Notas |
|---|---|---|---|
| `10.10.10.5` | `internal-host` | `80,445` | Nuevo objetivo |

## 9.3 proxy / Port-forwarding


### Proxychains

```bash
proxychains nmap -sT -Pn -p 80,445 10.10.10.5
```

### Redirección de puertos

```bash
portfwd add -l 8080 -p 80 -r 192.168.0.50
```
## 9.4 Notas de pivoting

```text
- Ruta interna detectada:
- Host usado como pivote:
- Método de túnel:
- Servicios alcanzables:
```

---

# 10. 🧾 Evidencias y respuestas

> En eJPT es importante tener las respuestas claras y localizables.

## 10.1 Evidencias por host

| Host | Evidencia | Comando / Ruta | Valor |
|---|---|---|---|
| `X.X.X.X` | Usuario actual | `whoami` | `user` |
| `X.X.X.X` | Hostname | `hostname` | `machine01` |
| `X.X.X.X` | Credencial | `config.php` | `user:pass` |
| `X.X.X.X` | Privilegios | `sudo -l` | `file como sudo` |

## 10.2 Preguntas / respuestas del examen

| Nº | Pregunta | Respuesta | Evidencia |
|---|---|---|---|
| 1 | `Pregunta del examen` | `Respuesta` | `Comando / captura` |
| 2 | `Pregunta del examen` | `Respuesta` | `Comando / captura` |
| 3 | `Pregunta del examen` | `Respuesta` | `Comando / captura` |

---

# 11. ✅ Checklist de control

## Reconocimiento

- [ ] Identificada IP atacante.
- [ ] Identificada red inicial.
- [ ] Descubiertos hosts activos.
- [ ] Escaneo completo de puertos por host.
- [ ] Escaneo de versiones y scripts básicos.

## Enumeración

- [ ] FTP enumerado.
- [ ] SMB enumerado.
- [ ] HTTP/HTTPS enumerado.
- [ ] SSH probado con credenciales válidas.
- [ ] Bases de datos revisadas si existen.
- [ ] Archivos descargados revisados.

## Credenciales

- [ ] Usuarios recopilados.
- [ ] Contraseñas recopiladas.
- [ ] Hashes guardados.
- [ ] Reutilización de credenciales probada.
- [ ] Credenciales documentadas con origen.

## Explotación

- [ ] Vector de acceso inicial identificado.
- [ ] Shell obtenida.
- [ ] Shell estabilizada.
- [ ] Evidencia de acceso guardada.

## Escalada

- [ ] Enumeración local realizada.
- [ ] `sudo -l` comprobado.
- [ ] SUID revisado.
- [ ] Archivos de configuración revisados.
- [ ] Privilegios elevados si aplica.

## Pivoting

- [ ] Rutas internas revisadas.
- [ ] Interfaces internas identificadas.
- [ ] Hosts internos descubiertos.
- [ ] Túnel configurado si aplica.
- [ ] Servicios internos enumerados.

## Cierre

- [ ] Respuestas del examen rellenadas.
- [ ] Evidencias organizadas.
- [ ] Comandos importantes guardados.
- [ ] Notas finales completadas.

---

# 12. 🧠 Notas rápidas durante el examen

```text
[ ] Revisar puertos raros.
[ ] Probar credenciales en todos los servicios.
[ ] Mirar archivos de configuración web.
[ ] Comprobar si hay otra interfaz de red.
[ ] Revisar rutas internas.
[ ] No quedarse atascado demasiado tiempo en una sola máquina.
[ ] Documentar cada hallazgo al momento.
```

---

# 13. 📝 Resumen final del entorno

## Máquinas comprometidas

| Host | IP | Usuario inicial | Usuario final | Técnica principal |
|---|---|---|---|---|
| Host 1 | `X.X.X.X` | `user` | `root` | `Credenciales + sudo` |
| Host 2 | `X.X.X.X` | `www-data` | `root` | `Webshell + SUID` |

## Credenciales finales útiles

| Usuario | Contraseña | Servicio | Host |
|---|---|---|---|
| `usuario` | `contraseña` | SSH/SMB/Web | `X.X.X.X` |

## Rutas internas confirmadas

| Red | Acceso mediante | Hosts relevantes |
|---|---|---|
| `X.X.X.X/XX` | `Host 1` | `Host 2, Host 3` |

---

# 14. 📚 Lecciones aprendidas

```text
- Qué técnica fue más importante:
- Qué servicio dio más información:
- Qué error me hizo perder tiempo:
- Qué debería revisar antes la próxima vez:
- Comandos que quiero recordar:
```

---

## ⚠️ Nota de uso

Esta plantilla está pensada para documentación personal, estudio y organización durante laboratorios o exámenes prácticos autorizados. No incluye respuestas del examen ni sustituye la metodología: sirve para mantener ordenada la información y evitar perder evidencias importantes.

---

## <img src="./assets/images/logoactual.jpg" width=25 style="; border-radius:50%;">  ¡Hola! Me llamo Saúl Ruiz
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
