# 🧪 Plantilla de Documentación eJPT / Laboratorios de Pentesting

> Plantilla creada para documentar prácticas, laboratorios, máquinas vulnerables y ejercicios de preparación para eJPT de forma ordenada, profesional y reutilizable.
>
> Úsala para estudiar, crear write-ups privados, reforzar metodología y construir evidencias técnicas sin depender de una máquina concreta.

---

## 📌 Información general

| Campo | Información |
|---|---|
| **Nombre del laboratorio** | `[Nombre del lab / máquina / escenario]` |
| **Plataforma** | `[INE / TryHackMe / Hack The Box / DockerLab / Local / Otra]` |
| **Dificultad estimada** | `[Fácil / Media / Alta]` |
| **Sistema objetivo** | `[Linux / Windows / Web / Red / Mixto]` |
| **IP objetivo** | `[X.X.X.X]` |
| **Fecha** | `[DD/MM/AAAA]` |
| **Autor** | `Saúl Ruiz` |
| **Estado** | `[En progreso / Completado / Revisado]` |

---

## 🎯 Objetivo del laboratorio

Describe brevemente qué se busca practicar en este laboratorio.

Ejemplo:

> El objetivo de este laboratorio es practicar una metodología básica de pentesting orientada a eJPT, incluyendo reconocimiento, enumeración de servicios, identificación de vulnerabilidades, explotación controlada, obtención de acceso inicial y escalada de privilegios.

### Habilidades trabajadas

- `[Reconocimiento de red]`
- `[Enumeración de servicios]`
- `[Análisis web]`
- `[Explotación de vulnerabilidades]`
- `[Fuerza bruta controlada]`
- `[Transferencia de archivos]`
- `[Escalada de privilegios Linux/Windows]`
- `[Pivoting / routing / port forwarding, si aplica]`
- `[Documentación técnica]`

---

## 🧭 Metodología utilizada

Durante el laboratorio se seguirá una metodología ordenada para evitar saltarse fases importantes:

1. **Preparación del entorno**
2. **Descubrimiento de hosts**
3. **Escaneo de puertos y servicios**
4. **Enumeración específica por servicio**
5. **Análisis de posibles vulnerabilidades**
6. **Explotación controlada**
7. **Acceso inicial**
8. **Enumeración interna**
9. **Escalada de privilegios**
10. **Post-explotación básica y evidencias**
11. **Conclusiones y aprendizaje**

---

## 🧱 Preparación del entorno

Antes de comenzar, se comprueba que el entorno de laboratorio esté correctamente configurado.

### Datos del entorno

| Elemento | Valor |
|---|---|
| **Máquina atacante** | `[Kali Linux / Parrot / Ubuntu]` |
| **Interfaz de red** | `[tun0 / eth0 / docker0 / otra]` |
| **Red del laboratorio** | `[X.X.X.0/24]` |
| **Herramientas principales** | `[nmap, gobuster, smbclient, hydra, metasploit, netcat, linpeas...]` |

### Comprobaciones iniciales

```bash
ip a
ip route
ping -c 1 <IP_OBJETIVO>
```

**Explicación:**

- `ip a` permite comprobar las interfaces de red disponibles.
- `ip route` ayuda a verificar las rutas activas.
- `ping` sirve para comprobar conectividad básica con el objetivo, aunque algunos sistemas pueden bloquear ICMP.

### Evidencia

```text
[Añadir aquí salida relevante o captura]
```


---

## 🔎 Fase 1: Descubrimiento de hosts

Esta fase sirve para identificar qué máquinas están activas dentro del rango de red.

### Comando utilizado

```bash
nmap -sn <RANGO_DE_RED>
```

### Explicación

El parámetro `-sn` realiza un descubrimiento de hosts sin escanear puertos. Es útil para detectar sistemas activos antes de realizar escaneos más detallados.

| Argumento | Significado |
|---|---|
| `-sn` | Realiza host discovery sin escaneo de puertos |
| `<RANGO_DE_RED>` | Rango objetivo, por ejemplo `192.168.1.0/24` |

### Resultado

```text
Host descubierto: <IP_OBJETIVO>
```

### Notas

- Si no hay respuesta por ICMP, probar con técnicas alternativas.
- En laboratorios tipo eJPT puede ser necesario identificar varias máquinas dentro de una red.
- Guardar siempre las IP encontradas para fases posteriores.


---

## 🚪 Fase 2: Escaneo de puertos

Una vez identificado el objetivo, se realiza un escaneo de puertos para descubrir servicios expuestos.

### Escaneo inicial

```bash
nmap -p- --open -T4 <IP_OBJETIVO> -oN scans/all_ports.txt
```

### Explicación

Este escaneo busca todos los puertos TCP abiertos del objetivo. Guardar el resultado permite consultarlo más tarde sin repetir el escaneo.

| Argumento | Significado |
|---|---|
| `-p-` | Escanea todos los puertos TCP, del 1 al 65535 |
| `--open` | Muestra principalmente puertos abiertos |
| `-T4` | Aumenta la velocidad del escaneo en entornos controlados |
| `-oN` | Guarda la salida en formato normal |

### Puertos encontrados

| Puerto | Servicio | Estado | Observaciones |
|---|---|---|---|
| `[21]` | `[FTP]` | `[open]` | `[Anonymous login / versión / nota]` |
| `[22]` | `[SSH]` | `[open]` | `[OpenSSH versión]` |
| `[80]` | `[HTTP]` | `[open]` | `[Web detectada]` |
| `[445]` | `[SMB]` | `[open]` | `[Recursos compartidos]` |


---

## 🧬 Fase 3: Detección de versiones y scripts básicos

Con los puertos detectados, se realiza un escaneo más específico para obtener versiones, banners y resultados de scripts comunes.

### Comando utilizado

```bash
nmap -sC -sV -p <PUERTOS> <IP_OBJETIVO> -oN scans/services.txt
```

### Explicación

Este comando permite conocer mejor los servicios activos y puede revelar información útil como versiones vulnerables, configuraciones inseguras o acceso anónimo.

| Argumento | Significado |
|---|---|
| `-sC` | Ejecuta scripts NSE por defecto |
| `-sV` | Detecta versiones de servicios |
| `-p` | Especifica los puertos a analizar |
| `-oN` | Guarda la salida en un fichero |

### Resultado resumido

```text
[Añadir aquí los servicios, versiones y datos relevantes]
```

### Posibles hallazgos

- `[Servicio con acceso anónimo]`
- `[Versión vulnerable]`
- `[Banner con información sensible]`
- `[Servicio mal configurado]`
- `[Ruta o recurso expuesto]`


---

## 📚 Fase 4: Enumeración por servicio

En esta sección se documenta la enumeración específica de cada servicio encontrado.

---

### 🌐 Enumeración HTTP / HTTPS

Usar esta sección si existe un servicio web en puertos como `80`, `443`, `8080`, `8000`, etc.

#### Comprobación inicial

```bash
whatweb http://<IP_OBJETIVO>
curl -I http://<IP_OBJETIVO>
```

**Explicación:**

- `whatweb` identifica tecnologías web.
- `curl -I` muestra cabeceras HTTP.

#### Enumeración de directorios

```bash
gobuster dir -u http://<IP_OBJETIVO> -w /usr/share/wordlists/dirb/common.txt -x php,txt,html -o scans/gobuster.txt
```

| Argumento | Significado |
|---|---|
| `dir` | Modo de enumeración de directorios |
| `-u` | URL objetivo |
| `-w` | Wordlist utilizada |
| `-x` | Extensiones a probar |
| `-o` | Guarda la salida |

#### Resultados relevantes

| Ruta | Código | Observación |
|---|---|---|
| `/admin` | `[200/301/403]` | `[Panel / recurso restringido]` |
| `/uploads` | `[200]` | `[Posible subida de archivos]` |
| `/robots.txt` | `[200]` | `[Rutas interesantes]` |


---

### 📁 Enumeración FTP

Usar esta sección si el puerto `21` está abierto.

#### Acceso anónimo

```bash
ftp <IP_OBJETIVO>
# Usuario: anonymous
# Contraseña: anonymous
```

#### Comandos útiles dentro de FTP

```bash
ls
pwd
get <archivo>
put <archivo>
bye
```

### Explicación

FTP puede exponer archivos sensibles, credenciales, backups o notas internas. Si permite acceso anónimo, es un hallazgo importante que debe documentarse.

#### Evidencias encontradas

```text
[Archivos descargados]
[Contenido relevante]
[Credenciales o pistas encontradas]
```


---

### 🧩 Enumeración SMB

Usar esta sección si están abiertos los puertos `139` o `445`.

#### Listar recursos compartidos

```bash
smbclient -L //<IP_OBJETIVO>/ -N
smbclient -L //<IP_OBJETIVO>/ -U <USUARIO>
```

#### Acceder a un recurso compartido

```bash
smbclient //<IP_OBJETIVO>/<RECURSO> -U <USUARIO>
```

#### Comandos útiles dentro de SMB

```bash
ls
cd <directorio>
get <archivo>
put <archivo>
exit
```

### Explicación

SMB puede revelar carpetas compartidas, permisos inseguros, archivos internos o credenciales reutilizables. En eJPT es importante enumerar SMB con y sin credenciales.

#### Recursos encontrados

| Recurso | Permisos | Observaciones |
|---|---|---|
| `[share]` | `[READ / WRITE]` | `[Información relevante]` |
| `[users]` | `[READ]` | `[Posibles usuarios]` |


---

### 🔐 Enumeración SSH

Usar esta sección si el puerto `22` está abierto.

#### Comprobación de versión

```bash
nmap -sV -p 22 <IP_OBJETIVO>
```

#### Acceso SSH

```bash
ssh <USUARIO>@<IP_OBJETIVO>
ssh -i <CLAVE_PRIVADA> <USUARIO>@<IP_OBJETIVO>
```

### Explicación

SSH suele ser una vía de acceso cuando se obtienen credenciales válidas, claves privadas o usuarios reutilizados desde otros servicios.

#### Credenciales probadas

| Usuario | Contraseña / clave | Resultado |
|---|---|---|
| `[usuario]` | `[contraseña]` | `[válida/no válida]` |


---

### 🗄️ Enumeración de bases de datos

Usar esta sección si se detectan servicios como MySQL, PostgreSQL, MSSQL o Redis.

#### MySQL

```bash
mysql -h <IP_OBJETIVO> -u <USUARIO> -p
```

#### PostgreSQL

```bash
psql -h <IP_OBJETIVO> -U <USUARIO>
```

#### MSSQL

```bash
impacket-mssqlclient <DOMINIO>/<USUARIO>:<PASSWORD>@<IP_OBJETIVO>
```

### Explicación

Las bases de datos pueden contener credenciales, hashes, usuarios, rutas internas o información útil para escalar acceso.

#### Información encontrada

```text
[Bases de datos]
[Tablas relevantes]
[Credenciales]
[Hashes]
```


---

## 🔍 Fase 5: Análisis de vulnerabilidades

En esta fase se relaciona la información recopilada con posibles vectores de ataque.

### Hallazgos identificados

| ID | Hallazgo | Servicio | Riesgo | Evidencia |
|---|---|---|---|---|
| `H-01` | `[Acceso anónimo FTP]` | `FTP` | `[Medio]` | `[Captura / salida]` |
| `H-02` | `[Credenciales expuestas]` | `[HTTP/SMB]` | `[Alto]` | `[Archivo encontrado]` |
| `H-03` | `[Versión vulnerable]` | `[Servicio]` | `[Alto]` | `[nmap / exploit-db]` |

### Búsqueda de exploits o referencias

```bash
searchsploit <SERVICIO> <VERSION>
```

### Explicación

No todo hallazgo es explotable directamente. En esta fase se prioriza lo que tiene más probabilidad de dar acceso o aportar información útil.

### Vector elegido

```text
[Explicar por qué se elige este vector y no otro]
```


---

## ⚔️ Fase 6: Explotación controlada

En esta fase se documenta el vector usado para obtener acceso inicial o extraer información sensible.

> [!IMPORTANT]
> Todo el proceso debe realizarse únicamente en entornos autorizados, laboratorios propios o plataformas de práctica.

### Vector de explotación

```text
[Servicio o vulnerabilidad utilizada]
```

### Comando / procedimiento

```bash
[comando utilizado]
```

### Explicación

```text
[Explicar qué hace el comando, por qué funciona y qué resultado se espera]
```

### Resultado obtenido

```text
[Credenciales / shell / archivo sensible / token / hash / acceso]
```


---

## 🖥️ Fase 7: Acceso inicial

Una vez obtenido un vector válido, se documenta cómo se consigue acceso al sistema.

### Tipo de acceso conseguido

- `[Shell reversa]`
- `[Acceso SSH]`
- `[Panel web]`
- `[Sesión Meterpreter]`
- `[Credenciales válidas]`
- `[Otro]`

### Comando utilizado

```bash
[comando de acceso]
```

### Estabilización de shell, si aplica

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
stty rows 40 columns 120
```

### Explicación

Una shell estable permite trabajar mejor dentro del sistema, usar comandos interactivos y evitar problemas con editores, `sudo`, autocompletado o señales de teclado.

### Usuario comprometido

```bash
whoami
hostname
id
```

```text
Usuario: [usuario]
Host: [hostname]
Grupos: [grupos]
```


---

## 🧾 Fase 8: Enumeración interna

Después del acceso inicial, se recopila información del sistema para buscar vías de escalada de privilegios.

### Información básica del sistema

```bash
whoami
id
hostname
uname -a
cat /etc/os-release
```

### Usuarios y directorios

```bash
cat /etc/passwd
ls -la /home
find /home -type f -name "*.txt" 2>/dev/null
```

### Permisos sudo

```bash
sudo -l
```

### Binarios SUID

```bash
find / -perm -4000 -type f 2>/dev/null
```

### Capacidades

```bash
getcap -r / 2>/dev/null
```

### Tareas programadas

```bash
cat /etc/crontab
ls -la /etc/cron.*
```

### Procesos y puertos internos

```bash
ps aux
ss -tulnp
```

### Archivos interesantes

```bash
find / -name "*.bak" 2>/dev/null
find / -name "*.conf" 2>/dev/null
find / -name "*password*" 2>/dev/null
```

### Explicación

La enumeración interna es una de las fases más importantes. El objetivo es encontrar configuraciones incorrectas, credenciales, permisos inseguros, binarios abusables o servicios internos no visibles desde fuera.


---

## 🧗 Fase 9: Escalada de privilegios

En esta fase se documenta cómo se pasa de un usuario limitado a un usuario con mayores privilegios.

### Vector de escalada identificado

```text
[Permiso sudo mal configurado / SUID / credenciales reutilizadas / servicio vulnerable / tarea cron / PATH hijacking / otro]
```

### Evidencia del vector

```bash
[comando que demuestra el vector]
```

### Explotación del vector

```bash
[comando utilizado para escalar privilegios]
```

### Explicación

```text
[Explicar por qué este vector permite escalar privilegios]
```

### Confirmación de privilegios

```bash
whoami
id
```

```text
Usuario final: [root / Administrator / usuario privilegiado]
```


---

## 🏁 Fase 10: Evidencias finales

En esta sección se documentan las pruebas principales de compromiso del laboratorio.

### Flags / archivos relevantes

| Archivo | Ruta | Contenido / evidencia |
|---|---|---|
| `user.txt` | `[ruta]` | `[valor o captura]` |
| `root.txt` | `[ruta]` | `[valor o captura]` |
| `[otro]` | `[ruta]` | `[valor o captura]` |

### Capturas finales



---

## 🔁 Pivoting / redes internas, si aplica

Usar esta sección solo si el laboratorio incluye varias redes o máquinas internas.

### Información de red encontrada

```bash
ip a
ip route
arp -a
```

### Hosts internos detectados

| IP interna | Servicio | Observación |
|---|---|---|
| `[IP]` | `[Servicio]` | `[Descripción]` |

### Técnicas utilizadas

- `[Port forwarding]`
- `[Proxychains]`
- `[Chisel]`
- `[SSH tunneling]`
- `[Metasploit autoroute]`

### Comandos

```bash
[comandos utilizados]
```

### Explicación

El pivoting permite alcanzar redes o servicios que no son accesibles directamente desde la máquina atacante, utilizando una máquina comprometida como punto intermedio.


---

## 🧹 Limpieza del laboratorio

Al finalizar, se documentan las acciones realizadas para cerrar el entorno de forma ordenada.

```bash
[comando para detener contenedor / cerrar VPN / eliminar archivos temporales]
```

### Notas

- Detener máquinas o contenedores que ya no se usen.
- Guardar capturas y resultados importantes.
- Eliminar credenciales temporales o claves generadas si no se necesitan.
- Organizar los ficheros de evidencia.


---

## 🧠 Resumen técnico

### Cadena de ataque

```text
[Descubrimiento] → [Enumeración] → [Hallazgo] → [Explotación] → [Acceso inicial] → [Enumeración interna] → [Escalada de privilegios]
```

### Resumen en una frase

> En este laboratorio se consiguió comprometer el sistema mediante `[vector inicial]`, obteniendo acceso como `[usuario inicial]` y escalando privilegios a `[usuario final]` mediante `[vector de escalada]`.

### Tabla resumen

| Fase | Resultado |
|---|---|
| Descubrimiento | `[IP objetivo identificada]` |
| Puertos abiertos | `[21, 22, 80, 445...]` |
| Servicio vulnerable | `[Servicio]` |
| Acceso inicial | `[Usuario / método]` |
| Escalada | `[Vector]` |
| Privilegios finales | `[root / Administrator]` |

---

## 📖 Lecciones aprendidas

Documenta aquí lo más importante que has aprendido.

- `[Lección 1]`
- `[Lección 2]`
- `[Lección 3]`

### Errores cometidos

- `[Error o bloqueo encontrado]`
- `[Cómo se solucionó]`

### Cosas a repasar

- `[Servicio o técnica que debo estudiar más]`
- `[Comando que debo memorizar]`
- `[Concepto que debo practicar]`

---

## 🧰 Comandos útiles del laboratorio

```bash
# Descubrimiento
nmap -sn <RANGO>

# Escaneo completo
nmap -p- --open -T4 <IP> -oN scans/all_ports.txt

# Versiones y scripts
nmap -sC -sV -p <PUERTOS> <IP> -oN scans/services.txt

# Web
whatweb http://<IP>
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirb/common.txt -x php,txt,html

# FTP
ftp <IP>

# SMB
smbclient -L //<IP>/ -N
smbclient //<IP>/<SHARE> -U <USUARIO>

# SSH
ssh <USUARIO>@<IP>
ssh -i id_rsa <USUARIO>@<IP>

# Linux privesc
sudo -l
find / -perm -4000 -type f 2>/dev/null
getcap -r / 2>/dev/null
```

---

## 🗂️ Estructura recomendada del repositorio

```text
nombre-del-laboratorio/
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
        ├── 01-preparacion-entorno.png
        ├── 02-descubrimiento-hosts.png
        ├── 03-escaneo-puertos.png
        └── ...
```

---

## ✅ Checklist eJPT para documentar laboratorios

- [ ] He identificado la IP objetivo.
- [ ] He escaneado todos los puertos TCP.
- [ ] He detectado versiones de servicios.
- [ ] He enumerado cada servicio relevante.
- [ ] He guardado los resultados importantes.
- [ ] He documentado usuarios, credenciales o pistas encontradas.
- [ ] He explicado el vector de explotación.
- [ ] He documentado el acceso inicial.
- [ ] He realizado enumeración interna.
- [ ] He explicado la escalada de privilegios.
- [ ] He añadido evidencias finales.
- [ ] He escrito lecciones aprendidas.

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
