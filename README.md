# 📊 VerVisitas - Analizador de Logs Apache

![Hacking y Seguridad](http://hackingyseguridad.com/banner.png)

> **Herramienta moderna para monitorizar y analizar logs de peticiones web en Apache Server en tiempo real.**

![Logo VerVisitas](./vervisitas.png)

## 📋 Tabla de Contenidos

- [Introducción](#introducción)
- [¿Qué es VerVisitas?](#qué-es-vervisitas)
- [Características](#características)
- [Requisitos](#requisitos)
- [Instalación](#instalación)
- [Scripts Disponibles](#scripts-disponibles)
- [Uso Detallado](#uso-detallado)
- [Configuración de Apache](#configuración-de-apache)
- [Análisis de Logs](#análisis-de-logs)
- [Formatos de Log](#formatos-de-log)
- [Geolocalización de IPs](#geolocalización-de-ips)
- [Interfaz Web](#interfaz-web)
- [Seguridad](#seguridad)
- [Análisis Avanzado](#análisis-avanzado)
- [Monitoreo en Tiempo Real](#monitoreo-en-tiempo-real)
- [Casos de Uso](#casos-de-uso)
- [Troubleshooting](#troubleshooting)
- [Referencias](#referencias)

---

## Introducción

Los servidores web generan miles de peticiones diarias. Analizar estos logs es fundamental para:

✅ **Detectar ataques** - Identificar patrones maliciosos  
✅ **Monitorear tráfico** - Ver cuántas visitas recibe tu sitio  
✅ **Auditar accesos** - Quién accedió a qué y cuándo  
✅ **Investigar errores** - Diagnosticar problemas de conexión  
✅ **Optimizar rendimiento** - Identificar recursos lentos  
✅ **Cumplimiento legal** - Registro de accesos para auditoría  

### 🎯 Objetivo del Proyecto

VerVisitas es una **herramienta minimalista pero potente** que:

- Visualiza logs en tiempo real
- Proporciona interfaz web moderna
- Permite análisis rápido de patrones
- Facilita investigación forense
- Oferece geolocalización de IPs

---

## ¿Qué es VerVisitas?

VerVisitas es un conjunto de **scripts bash + interfaz web HTML** que permite:

1. **Ver logs en vivo** - Monitoreo en tiempo real de peticiones HTTP
2. **Analizar conexiones** - Ver conexiones activas por puerto
3. **Interfaz gráfica** - Dashboard web para visualización
4. **Filtrar datos** - Búsquedas rápidas por IP, usuario-agente, etc.
5. **Geolocalizar** - Obtener ubicación geográfica de visitantes
6. **Exportar datos** - Guardar logs para análisis posterior

### 🏗️ Arquitectura

```
┌─────────────────────────────────────────┐
│     SERVIDOR APACHE                     │
├─────────────────────────────────────────┤
│  /var/log/apache2/access.log            │
│  /var/log/apache2/error.log             │
│  /var/log/apache2/other_vhosts_access.log
└────────────────┬────────────────────────┘
                 │
       ┌─────────┴─────────┐
       │                   │
    ┌──▼──┐           ┌───▼────┐
    │ CLI │           │  WEB   │
    │Scripts          │Interface
    └──┬──┘           └───┬────┘
       │                  │
   vervisitas        index.html
   vervisitas2       index2.html
   verconexiones     favicon.ico
   verconxiones.sh   gatito.* (assets)
```

---

## Características

### ✨ Funcionalidades Principales

| Característica | Descripción | Uso |
|----------------|-------------|-----|
| **Monitoreo Real-Time** | Ver logs mientras suceden | `tail -f` automático |
| **Interfaz Web** | Dashboard HTML/CSS moderno | Acceso por navegador |
| **CLI Scripts** | Herramientas línea de comando | Automatización y scripting |
| **Geolocalización** | Obtener país/ciudad de IPs | Análisis geográfico |
| **Análisis Rápido** | Filtros y búsquedas | `grep` + awk optimizado |
| **Conexiones Activas** | Ver conexiones por puerto | Monitoreo de red |
| **Múltiples Formatos** | Apache, Nginx, syslog | Flexibilidad |
| **Estadísticas** | Resúmenes por IP, hora, etc. | Reportes |

### 🔧 Herramientas Incluidas

| Herramienta | Tipo | Función | Versión |
|------------|------|---------|---------|
| **vervisitas** | Script Bash | Ver logs de acceso | 1.0 |
| **vervisitas2** | Script Bash | Versión mejorada | 2.0 |
| **verconexiones** | Script Bash | Ver conexiones activas | 1.0 |
| **verconxiones.sh** | Script Bash | Versión mejorada | 2.0 |
| **index.html** | Web | Dashboard principal | 1.0 |
| **index2.html** | Web | Dashboard alternativo | 2.0 |
| **instalar.sh** | Script Setup | Instalación automática | 1.0 |

---

## Requisitos

### 🖥️ Requisitos del Sistema

| Requisito | Mínimo | Recomendado |
|-----------|--------|------------|
| **SO** | Linux (Ubuntu/Debian/CentOS) | Ubuntu 20.04+ |
| **Apache** | 2.2+ | 2.4+ (Con módulos mod_ssl, mod_rewrite) |
| **Bash** | 4.0+ | 5.0+ |
| **Coreutils** | Estándar | Con GNU awk, sed, grep |
| **Permisos** | Root o sudo | `sudo` sin contraseña (opcional) |
| **Espacio Disco** | 100 MB | 1 GB+ (para logs históricos) |
| **RAM** | 512 MB | 2 GB+ (para análisis grandes) |
| **Navegador** | HTML5 | Chrome, Firefox, Safari, Edge |

### 📦 Paquetes Necesarios

```bash
# Ubuntu/Debian
sudo apt-get install \
  apache2 \
  apache2-utils \
  curl \
  wget \
  gawk \
  grep \
  sed \
  coreutils \
  net-tools

# CentOS/RHEL
sudo yum install \
  httpd \
  httpd-tools \
  curl \
  wget \
  gawk \
  grep \
  sed \
  coreutils \
  net-tools

# macOS (Homebrew)
brew install apache2 curl wget gawk grep gnu-sed coreutils
```

### 🔐 Permisos Necesarios

```bash
# Ver logs de Apache requiere permisos root o grupo apache
sudo usermod -a -G adm $USER          # Grupo adm (Debian/Ubuntu)
sudo usermod -a -G apache $USER       # Grupo apache (CentOS)
newgrp adm                             # Aplicar nuevo grupo

# O ejecutar scripts con sudo
sudo /path/to/vervisitas
```

---

## Instalación

### 📥 Opción 1: Instalación Automática

```bash
# Clonar repositorio
git clone https://github.com/hackingyseguridad/vervisitas.git
cd vervisitas

# Hacer instalador ejecutable
chmod +x instalar.sh

# Ejecutar instalación
sudo ./instalar.sh
```

**¿Qué hace instalar.sh?**:
- ✅ Instala dependencias necesarias
- ✅ Copia scripts a `/usr/local/bin/`
- ✅ Copia archivos web a `/var/www/html/`
- ✅ Configura permisos
- ✅ Crea directorios necesarios

### 📥 Opción 2: Instalación Manual

#### Paso 1: Clonar repositorio

```bash
git clone https://github.com/hackingyseguridad/vervisitas.git
cd vervisitas
```

#### Paso 2: Instalar dependencias

**Ubuntu/Debian**:
```bash
sudo apt-get update
sudo apt-get install apache2 curl wget net-tools gawk
```

**CentOS/RHEL**:
```bash
sudo yum update
sudo yum install httpd curl wget net-tools gawk
```

#### Paso 3: Copiar scripts

```bash
# Copiar scripts a /usr/local/bin/ (opcional)
sudo cp vervisitas /usr/local/bin/
sudo cp vervisitas2 /usr/local/bin/
sudo cp verconexiones /usr/local/bin/
sudo cp verconxiones.sh /usr/local/bin/

# O copiar a la raíz web
sudo cp vervisitas /var/www/html/cgi-bin/
```

#### Paso 4: Hacer scripts ejecutables

```bash
sudo chmod +x /usr/local/bin/vervisitas*
sudo chmod +x /usr/local/bin/verconexiones*
```

#### Paso 5: Copiar archivos web

```bash
sudo cp index.html /var/www/html/
sudo cp index2.html /var/www/html/
sudo cp favicon.ico /var/www/html/
sudo cp gatito.* /var/www/html/
sudo chown -R www-data:www-data /var/www/html/
sudo chmod 755 /var/www/html/
```

#### Paso 6: Configurar permisos de logs

```bash
# Usuario actual puede leer logs de Apache
sudo usermod -a -G adm $USER

# O permitir lectura de logs
sudo chmod 644 /var/log/apache2/access.log
```

### ✅ Verificar Instalación

```bash
# Verificar que los scripts están disponibles
which vervisitas
which vervisitas2
which verconexiones

# Verificar que Apache está corriendo
sudo systemctl status apache2

# Verificar acceso a logs
sudo tail -5 /var/log/apache2/access.log

# Probar que funciona
sudo vervisitas
```

---

## Scripts Disponibles

### 🔧 Script: vervisitas

**Descripción**: Ver logs de acceso HTTP de Apache en tiempo real.

**Uso**:
```bash
sudo vervisitas
# o
sudo tail -f /var/log/apache2/access.log
```

**Salida**:
```
192.168.1.100 - - [14/Jul/2026:12:34:56 +0000] "GET / HTTP/1.1" 200 1234 "-" "Mozilla/5.0"
203.0.113.45 - - [14/Jul/2026:12:35:01 +0000] "POST /api/users HTTP/1.1" 201 567 "-" "curl/7.68"
192.0.2.50 - - [14/Jul/2026:12:35:10 +0000] "GET /admin HTTP/1.1" 403 0 "-" "Mozilla/5.0"
```

**Campos mostrados**:
- IP del cliente
- Usuario (si autenticado)
- Timestamp
- Método HTTP (GET, POST, etc.)
- Ruta accedida
- Código HTTP
- Bytes enviados
- Referrer
- User-Agent

**Opciones útiles**:
```bash
# Ver últimas 20 líneas
sudo tail -20 /var/log/apache2/access.log

# Ver con timestamps en tiempo real
sudo tail -f /var/log/apache2/access.log | awk '{print strftime("%H:%M:%S"), $0}'

# Ver solo errores 4xx y 5xx
sudo grep " 4[0-9][0-9] \| 5[0-9][0-9] " /var/log/apache2/access.log

# Ver desde una hora específica
sudo tail -500 /var/log/apache2/access.log | grep "14/Jul/2026:15"
```

### 🔧 Script: vervisitas2

**Descripción**: Versión mejorada de vervisitas con análisis adicional.

**Uso**:
```bash
sudo vervisitas2
```

**Mejoras respecto a v1**:
- ✅ Filtra por rango de códigos HTTP
- ✅ Estadísticas de acceso por IP
- ✅ Resumen de métodos HTTP
- ✅ Top 10 de páginas accedidas
- ✅ Alertas de ataques potenciales

**Ejemplo de salida**:
```
═══════════════════════════════════════
  ESTADÍSTICAS DE ACCESO (últimas 1000 líneas)
═══════════════════════════════════════

TOP 10 IPs:
  1. 192.168.1.100 (234 accesos)
  2. 203.0.113.45   (89 accesos)
  3. 192.0.2.50     (45 accesos)

CÓDIGOS HTTP:
  200 OK:                  340
  404 Not Found:            28
  403 Forbidden:             5
  500 Server Error:          3

TOP 10 PÁGINAS:
  1. / (234 accesos)
  2. /api/users (89 accesos)
  3. /login (45 accesos)

MÉTODOS HTTP:
  GET:                     350
  POST:                     20
  PUT:                       3
  DELETE:                     1
```

### 🔧 Script: verconexiones

**Descripción**: Ver conexiones activas por puerto en tiempo real.

**Uso**:
```bash
sudo verconexiones
# o
sudo netstat -tapn | grep ESTABLISHED
```

**Salida**:
```
Proto Recv-Q Send-Q Local Address     Foreign Address     State   PID/Program
tcp        0      0 0.0.0.0:80        0.0.0.0:*          LISTEN  1234/apache2
tcp        0      0 0.0.0.0:443       0.0.0.0:*          LISTEN  1234/apache2
tcp        0    128 192.168.1.1:80    203.0.113.45:54321 ESTABLISHED 5678/apache2
tcp        0      0 192.168.1.1:443   192.0.2.50:8765    ESTABLISHED 6789/apache2
```

**Columnas**:
- **Proto** - Protocolo (tcp/udp)
- **Local Address** - IP y puerto local
- **Foreign Address** - IP y puerto remoto
- **State** - Estado de conexión (LISTEN, ESTABLISHED, etc.)
- **PID/Program** - Proceso que usa la conexión

**Opciones útiles**:
```bash
# Ver solo HTTP
sudo netstat -tapn | grep :80

# Ver solo HTTPS
sudo netstat -tapn | grep :443

# Ver conexiones establecidas
sudo netstat -tapn | grep ESTABLISHED

# Contar conexiones activas
sudo netstat -an | grep ESTABLISHED | wc -l

# Ver conexiones por segundo (en tiempo real)
watch -n 1 'sudo netstat -an | grep ESTABLISHED | wc -l'
```

### 🔧 Script: verconxiones.sh

**Descripción**: Versión mejorada de verconexiones con análisis.

**Uso**:
```bash
sudo verconxiones.sh
```

**Características**:
- ✅ Resumen por puerto
- ✅ Conexiones por estado
- ✅ Top IPs conectadas
- ✅ Alertas de congestión
- ✅ Estadísticas en tiempo real

---

## Uso Detallado

### 🔍 Casos de Uso Común

#### Monitorear en Tiempo Real

```bash
# Ver logs en vivo (últimas 10 líneas + nuevas)
sudo tail -f /var/log/apache2/access.log

# Con mejor formato
sudo tail -f /var/log/apache2/access.log | \
  awk '{print $4, $1, $6, $7, $9}'
```

**Salida**:
```
[14/Jul/2026:12:34:56 192.168.1.100 GET / 200
[14/Jul/2026:12:35:01 203.0.113.45 POST /api 201
```

#### Ver Últimas Visitas

```bash
# Últimas 50 visitas
sudo tail -50 /var/log/apache2/access.log

# Últimas 10 minutos
sudo awk -v s=$(date -d '10 min ago' +%s) \
  'BEGIN{OFS=" "} {print $4,$1,$7,$9}' \
  /var/log/apache2/access.log | \
  grep "14/Jul/2026:1[2-4]"
```

#### Filtrar por IP

```bash
# Ver accesos desde IP específica
sudo grep "192.168.1.100" /var/log/apache2/access.log

# Ver todas las IPs únicas
sudo awk '{print $1}' /var/log/apache2/access.log | sort -u

# Top 10 IPs
sudo awk '{print $1}' /var/log/apache2/access.log | sort | uniq -c | sort -rn | head -10
```

#### Detectar Ataques

```bash
# Ver intentos de acceso a /admin
sudo grep "/admin" /var/log/apache2/access.log | wc -l

# Ver intentos 404 (no encontrado)
sudo grep " 404 " /var/log/apache2/access.log | wc -l

# Ver intentos de inyección SQL
sudo grep -i "union\|select\|insert\|delete\|drop" /var/log/apache2/access.log

# Ver User-Agents sospechosos
sudo awk -F'" '{print $(NF-1)}' /var/log/apache2/access.log | sort | uniq -c | sort -rn
```

#### Analizar Errores

```bash
# Ver errores 5xx
sudo grep " 5[0-9][0-9] " /var/log/apache2/access.log

# Ver errores 4xx
sudo grep " 4[0-9][0-9] " /var/log/apache2/access.log

# Estadística de códigos HTTP
sudo awk '{print $9}' /var/log/apache2/access.log | sort | uniq -c | sort -rn
```

---

## Configuración de Apache

### ⚙️ Habilitar Módulos Necesarios

```bash
# Habilitar módulos
sudo a2enmod ssl              # HTTPS
sudo a2enmod rewrite          # URL rewriting
sudo a2enmod headers          # Headers
sudo a2enmod status           # Status page
sudo a2enmod access_compat    # Compatibilidad

# Reiniciar Apache
sudo systemctl restart apache2
```

### 📝 Formato de Logs Recomendado

#### Archivo: `/etc/apache2/apache2.conf`

```apache
# Formato de log personalizado (Common Log Format)
LogFormat "%h %l %u %t \"%r\" %>s %b" common

# Formato extendido (Combined Log Format) - RECOMENDADO
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined

# Formato JSON (para análisis avanzado)
LogFormat "{ \"time_local\": \"%{%Y-%m-%d}t %{%H:%M:%S}t\", \"client_ip\": \"%a\", \"request\": \"%r\", \"response_code\": %s, \"bytes_out\": %b, \"user_agent\": \"%{User-Agent}i\", \"referer\": \"%{Referer}i\" }" json

# Usar formato combinado
CustomLog ${APACHE_LOG_DIR}/access.log combined
CustomLog ${APACHE_LOG_DIR}/access.json json
```

#### Archivo: `/etc/apache2/sites-available/000-default.conf`

```apache
<VirtualHost *:80>
  ServerName ejemplo.com
  DocumentRoot /var/www/html
  
  # Logs específicos del sitio
  CustomLog ${APACHE_LOG_DIR}/ejemplo-access.log combined
  ErrorLog ${APACHE_LOG_DIR}/ejemplo-error.log
  
  # Nivel de log
  LogLevel warn
  
  # Logs con timestamps
  CustomLog ${APACHE_LOG_DIR}/ejemplo-access.log "%t %h %l %u \"%r\" %>s %b"
</VirtualHost>

<VirtualHost *:443>
  ServerName ejemplo.com
  DocumentRoot /var/www/html
  
  SSLEngine on
  SSLCertificateFile /etc/ssl/certs/cert.pem
  SSLCertificateKeyFile /etc/ssl/private/key.pem
  
  CustomLog ${APACHE_LOG_DIR}/ejemplo-ssl-access.log combined
  ErrorLog ${APACHE_LOG_DIR}/ejemplo-ssl-error.log
</VirtualHost>
```

#### Aplicar cambios

```bash
# Verificar sintaxis
sudo apache2ctl configtest
# Debería mostrar: Syntax OK

# Reiniciar Apache
sudo systemctl restart apache2
```

### 📊 Ubicación de Logs

| Distribución | Ubicación |
|-------------|-----------|
| **Ubuntu/Debian** | `/var/log/apache2/` |
| **CentOS/RHEL** | `/var/log/httpd/` |
| **macOS** | `/var/log/apache2/` |

**Archivos de log típicos**:
- `access.log` - Todas las peticiones HTTP
- `error.log` - Errores de Apache
- `other_vhosts_access.log` - Accesos de otros VirtualHosts
- `ssl_access.log` - Accesos HTTPS
- `ssl_error.log` - Errores HTTPS

---

## Análisis de Logs

### 📋 Campos del Log Apache (Combined Format)

```
192.168.1.100 - username [14/Jul/2026:12:34:56 +0000] "GET / HTTP/1.1" 200 1234 "-" "Mozilla/5.0"
│             │ │        │                              │ │   │   │    │  │     │
│             │ │        │                              │ │   │   │    │  │     └─ User-Agent
│             │ │        │                              │ │   │   │    │  └─ Referrer
│             │ │        │                              │ │   │   │    └─ Bytes enviados
│             │ │        │                              │ │   │   └─ Bytes recibidos (-)
│             │ │        │                              │ │   └─ HTTP Status Code
│             │ │        │                              │ └─ HTTP Request
│             │ │        └─ Timestamp
│             │ └─ User (si autenticado)
│             └─ Remote Logname (casi siempre -)
└─ Cliente IP

```

### 📊 Tabla de Códigos HTTP

| Código | Significado | Acción |
|--------|-------------|--------|
| **2xx** | **Éxito** | Petición procesada |
| 200 | OK | Petición correcta |
| 201 | Created | Recurso creado |
| 204 | No Content | Sin contenido (válido) |
| **3xx** | **Redirección** | Requiere acción adicional |
| 301 | Moved Permanently | Redirección permanente |
| 302 | Found | Redirección temporal |
| 304 | Not Modified | Cache válido |
| **4xx** | **Error Cliente** | Petición incorrecta |
| 400 | Bad Request | Sintaxis inválida |
| 401 | Unauthorized | Autenticación requerida |
| 403 | Forbidden | Acceso denegado |
| 404 | Not Found | No existe |
| 429 | Too Many Requests | Rate limit |
| **5xx** | **Error Servidor** | Fallo en servidor |
| 500 | Internal Error | Error genérico |
| 502 | Bad Gateway | Gateway inválido |
| 503 | Service Unavailable | Servicio no disponible |

### 🔍 Análisis Detallado

#### Ver resumen de accesos

```bash
# Total de peticiones
sudo wc -l /var/log/apache2/access.log

# Peticiones hoy
sudo awk -v d=$(date +%d) -v m=$(date +%b) '$4 ~ "/"d"/"m' /var/log/apache2/access.log | wc -l

# Bytes transferidos
sudo awk '{sum+=$10} END {print sum " bytes (" int(sum/1024/1024) " MB)"}' /var/log/apache2/access.log
```

#### Análisis por hora

```bash
# Peticiones por hora (hoy)
sudo awk '{print $4}' /var/log/apache2/access.log | \
  grep $(date +%d/%b/%Y) | \
  awk -F: '{print $2":00"}' | \
  sort | uniq -c | sort -rn
```

**Salida**:
```
120 12:00
115 13:00
98  14:00
87  15:00
```

#### Análisis de métodos HTTP

```bash
# Contar métodos
sudo awk '{print $6}' /var/log/apache2/access.log | sort | uniq -c
```

**Salida**:
```
850 GET
125 POST
12  PUT
5   DELETE
2   HEAD
```

#### Análisis de User-Agents

```bash
# Top 10 navegadores
sudo awk -F'"' '{print $(NF-1)}' /var/log/apache2/access.log | \
  sort | uniq -c | sort -rn | head -10
```

**Salida**:
```
234 Mozilla/5.0 (X11; Linux x86_64)...
89  Mozilla/5.0 (Windows NT 10.0)...
45  curl/7.68.0
23  wget/1.20.3
12  Python-requests/2.25.1
```

---

## Formatos de Log

### 📄 Common Log Format (CLF)

```apache
LogFormat "%h %l %u %t \"%r\" %>s %b" common
```

**Ejemplo**:
```
192.168.1.100 - - [14/Jul/2026:12:34:56 +0000] "GET / HTTP/1.1" 200 1234
```

**Campos**:
- `%h` - Remote host (IP)
- `%l` - Remote logname (casi siempre -)
- `%u` - Remote user
- `%t` - Timestamp
- `%r` - Request line
- `%s` - HTTP status
- `%b` - Bytes sent

### 📄 Combined Log Format (Recomendado)

```apache
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
```

**Ejemplo**:
```
192.168.1.100 - - [14/Jul/2026:12:34:56 +0000] "GET / HTTP/1.1" 200 1234 "http://google.com" "Mozilla/5.0"
```

### 📄 Formato Personalizado

```apache
# Con información adicional
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %T %D" detailed

# Códigos de formato útiles
# %h - IP remota
# %l - Remote logname
# %u - Remote user
# %t - Timestamp
# %r - Línea de petición
# %s - Status code
# %b - Bytes enviados
# %i - Header de entrada
# %o - Header de salida
# %T - Tiempo en segundos
# %D - Tiempo en microsegundos
# %{variable}e - Variable de entorno
# %>s - Status code (último)
```

### 📄 Formato JSON (Para análisis avanzado)

```apache
LogFormat "{ \"timestamp\": \"%t\", \"client_ip\": \"%h\", \"method\": \"%m\", \"uri\": \"%U\", \"status\": %s, \"bytes_sent\": %b, \"response_time_us\": %D, \"user_agent\": \"%{User-Agent}i\", \"referer\": \"%{Referer}i\" }" json
```

**Ejemplo**:
```json
{ "timestamp": "14/Jul/2026:12:34:56", "client_ip": "192.168.1.100", "method": "GET", "uri": "/", "status": 200, "bytes_sent": 1234, "response_time_us": 45000, "user_agent": "Mozilla/5.0", "referer": "-" }
```

---

## Geolocalización de IPs

### 🌍 Servicios de Geolocalización Gratis

| Servicio | Límite | Acceso | Ejemplo |
|----------|--------|--------|---------|
| **ip-api.com** | 45/min | REST API | `http://ip-api.com/json/192.168.1.1` |
| **ipapi.co** | 30000/mes | REST API | `https://ipapi.co/192.168.1.1/json` |
| **geoip.json.io** | Ilimitado | REST API | `https://geoip.json.io/192.168.1.1` |
| **GeoIP2** | Pagado | MaxMind | Más precisión |

### 📍 Script: Geolocalizar IPs

```bash
#!/bin/bash
# Script para geolocalizar IPs del log

IP=$1
API="http://ip-api.com/json/${IP}?fields=status,country,city,lat,lon,isp"

# Obtener datos
curl -s "$API" | jq .

# O con formato simple
curl -s "$API" | jq -r ".country, .city, .isp"
```

**Uso**:
```bash
chmod +x geoip.sh
./geoip.sh 203.0.113.45
```

**Salida**:
```
{
  "status": "success",
  "country": "United States",
  "city": "New York",
  "lat": 40.7128,
  "lon": -74.0060,
  "isp": "Verizon Communications"
}
```

### 🔍 Geolocalizar desde logs

```bash
# Extraer IPs únicas y geolocalizarlas
sudo awk '{print $1}' /var/log/apache2/access.log | \
  sort -u | \
  while read ip; do
    echo -n "$ip "
    curl -s "http://ip-api.com/json/$ip?fields=country,city" | jq -r '.country + ", " + .city'
  done
```

**Salida**:
```
192.168.1.100 United States, New York
203.0.113.45   Spain, Madrid
192.0.2.50    United Kingdom, London
```

### 📊 Mapa de accesos

Crear mapa HTML con accesos geolocalizados:

```html
<script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"></script>

<div id="map" style="height: 500px;"></div>

<script>
  const map = L.map('map').setView([20, 0], 2);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
  
  // Datos de accesos
  const visits = [
    {lat: 40.7128, lng: -74.0060, city: "New York", count: 234},
    {lat: 40.4168, lng: -3.7038, city: "Madrid", count: 89},
    {lat: 51.5074, lng: -0.1278, city: "London", count: 45}
  ];
  
  visits.forEach(v => {
    L.circleMarker([v.lat, v.lng], {
      radius: Math.sqrt(v.count),
      color: 'red'
    }).bindPopup(`${v.city}: ${v.count} visits`).addTo(map);
  });
</script>
```

---

## Interfaz Web

### 🌐 Archivos HTML Disponibles

| Archivo | Descripción | Características |
|---------|-------------|-----------------|
| **index.html** | Dashboard v1 | UI básico, estadísticas |
| **index2.html** | Dashboard v2 | UI mejorado, gráficos |
| **favicon.ico** | Icono | Pequeño favicon |
| **gatito.png** | Imagen | Asset decorativo |
| **gatito.gif** | Animación | GIF animado |
| **gatito.mp4** | Video | Video decorativo |

### 📊 Características de index.html

```html
<!DOCTYPE html>
<html>
<head>
  <title>VerVisitas - Monitor Apache</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; }
    .container { max-width: 1200px; margin: 0 auto; }
    .stat { background: #f0f0f0; padding: 10px; margin: 10px 0; border-radius: 5px; }
  </style>
</head>
<body>
  <h1>VerVisitas - Monitor de Logs Apache</h1>
  <div class="container">
    <div class="stat">
      <h2>Estadísticas en Tiempo Real</h2>
      <p>Total de visitas: <span id="total"></span></p>
      <p>Últimas 24h: <span id="last24"></span></p>
    </div>
  </div>
</body>
</html>
```

### 🔄 Auto-actualización

```html
<script>
  // Recargar cada 5 segundos
  setTimeout(() => location.reload(), 5000);
  
  // O con fetch
  setInterval(() => {
    fetch('/api/stats.json')
      .then(r => r.json())
      .then(data => {
        document.getElementById('total').textContent = data.total;
        document.getElementById('last24').textContent = data.last24h;
      });
  }, 5000);
</script>
```

---

## Seguridad

### 🔒 Proteger Acceso a Logs

#### Proteger directorio web

```apache
# En /etc/apache2/apache2.conf o sitio

<Directory /var/www/html/logs>
  Require ip 192.168.1.0/24
  # o
  Require user admin
</Directory>
```

#### Requiere autenticación

```apache
<Location /logs>
  AuthType Basic
  AuthName "Acceso Restringido"
  AuthUserFile /etc/apache2/.htpasswd
  Require user admin
</Location>
```

**Crear usuario**:
```bash
sudo htpasswd -c /etc/apache2/.htpasswd admin
```

#### Limitar acceso SSH

```bash
# Solo root puede ejecutar scripts
sudo chmod 700 /usr/local/bin/vervisitas*

# O con sudo
sudo visudo
# Agregar:
# usuario ALL=NOPASSWD: /usr/local/bin/vervisitas
```

### 🛡️ Proteger Datos Sensibles

```bash
# Rotar logs regularmente
sudo logrotate /etc/logrotate.d/apache2

# Comprimir logs antiguos
sudo gzip /var/log/apache2/access.log.1

# Archivar en almacenamiento seguro
sudo tar czf /backup/apache-logs-$(date +%Y%m%d).tar.gz /var/log/apache2/
```

### 📊 Permisos Recomendados

```bash
# Logs solo legibles por root/apache
sudo chmod 640 /var/log/apache2/access.log
sudo chmod 640 /var/log/apache2/error.log

# Scripts solo ejecutables por autorizado
sudo chmod 750 /usr/local/bin/vervisitas
sudo chmod 750 /usr/local/bin/verconexiones

# Archivos web públicos
sudo chmod 644 /var/www/html/index.html
sudo chmod 755 /var/www/html/
```

---

## Análisis Avanzado

### 🔬 Detectar Ataques

#### Fuerza Bruta SSH (en logs)

```bash
# Ver intentos fallidos de autenticación
sudo grep "authentication failure" /var/log/auth.log | wc -l

# Por IP
sudo grep "authentication failure" /var/log/auth.log | awk '{print $15}' | sort | uniq -c | sort -rn
```

#### SQLi (SQL Injection)

```bash
# Buscar patrones SQL en logs
sudo grep -iE "union|select|insert|delete|drop|exec|script" /var/log/apache2/access.log

# Con detalles
sudo grep -iE "union|select|insert" /var/log/apache2/access.log | awk '{print $1, $6, $7}'
```

#### XSS (Cross Site Scripting)

```bash
# Buscar scripts sospechosos
sudo grep -iE "<script|javascript:|onerror|onclick" /var/log/apache2/access.log

# Con análisis
sudo grep -iE "script|javascript" /var/log/apache2/access.log | wc -l
```

#### Path Traversal

```bash
# Buscar accesos a directorios padre
sudo grep -E "\.\./|\.\.%2f" /var/log/apache2/access.log
```

#### Rate Limiting

```bash
# IPs con más de 100 accesos en últimas 2 horas
sudo awk '{print $1}' /var/log/apache2/access.log | sort | uniq -c | awk '$1 > 100 {print $2}'
```

### 📈 Gráficos y Reportes

#### Accesos por hora (ASCII)

```bash
#!/bin/bash
# Script para graficar accesos por hora

echo "Accesos por Hora - Últimas 24h"
sudo awk -F: '{print $2}' /var/log/apache2/access.log | \
  grep $(date +%d/%b/%Y) | \
  cut -d: -f2 | \
  sort | uniq -c | \
  sort -n | \
  awk '{for(i=1;i<=$1;i++) printf "#"; printf " %s (%d)\n", $2, $1}'
```

**Salida**:
```
Accesos por Hora - Últimas 24h
################ 12 (16)
#################### 13 (20)
########################## 14 (26)
###################### 15 (22)
```

#### Reporte diario

```bash
#!/bin/bash
# Script de reporte diario

LOG=/var/log/apache2/access.log
DATE=$(date +%d/%b/%Y)

echo "═══════════════════════════════════════"
echo "  REPORTE DE ACCESOS - $DATE"
echo "═══════════════════════════════════════"
echo ""

echo "Total de peticiones:"
sudo grep "$DATE" $LOG | wc -l

echo ""
echo "Peticiones por IP (Top 10):"
sudo grep "$DATE" $LOG | awk '{print $1}' | sort | uniq -c | sort -rn | head -10

echo ""
echo "Códigos HTTP:"
sudo grep "$DATE" $LOG | awk '{print $9}' | sort | uniq -c | sort -rn
```

---

## Monitoreo en Tiempo Real

### ⏱️ Monitoreo Continuo

```bash
# Ver logs en vivo (actualización cada segundo)
sudo watch -n 1 'tail -20 /var/log/apache2/access.log'

# Con grep en tiempo real
sudo tail -f /var/log/apache2/access.log | grep "404\|500"

# Con timestamps
sudo tail -f /var/log/apache2/access.log | while read line; do
  echo "[$(date '+%H:%M:%S')] $line"
done
```

### 🚨 Alertas Automáticas

#### Script de monitoreo con alertas

```bash
#!/bin/bash
# Monitorear errores 5xx y enviar alerta

LOG=/var/log/apache2/access.log
THRESHOLD=5  # Alertar si hay más de 5 errores 5xx por minuto

while true; do
  ERRORS=$(tail -100 $LOG | grep " 5[0-9][0-9] " | wc -l)
  
  if [ $ERRORS -gt $THRESHOLD ]; then
    echo "⚠️ ALERTA: $ERRORS errores detectados" | \
      mail -s "Apache Error Alert" admin@ejemplo.com
    
    # O notificación local
    notify-send "Apache Alert" "$ERRORS errors detected"
  fi
  
  sleep 60
done
```

#### Script con systemd timer

```bash
# /etc/systemd/system/apache-monitor.service
[Unit]
Description=Apache Log Monitor
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/apache-monitor.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

### 📊 Prometheus Exporter

Exponer métricas para Prometheus/Grafana:

```bash
#!/bin/bash
# Metrics en formato Prometheus

PORT=9100

http_server() {
  while true; do
    read -r line
    
    if [[ "$line" == "" ]]; then
      total=$(wc -l < /var/log/apache2/access.log)
      status_200=$(grep " 200 " /var/log/apache2/access.log | wc -l)
      status_404=$(grep " 404 " /var/log/apache2/access.log | wc -l)
      
      cat <<EOF
HTTP/1.1 200 OK
Content-Type: text/plain

# HELP apache_total_requests Solicitudes totales
# TYPE apache_total_requests counter
apache_total_requests $total

# HELP apache_status_200 Respuestas 200
# TYPE apache_status_200 counter
apache_status_200 $status_200

# HELP apache_status_404 Respuestas 404
# TYPE apache_status_404 counter
apache_status_404 $status_404
EOF
      break
    fi
  done
}

nc -l -p $PORT -e "bash -c 'http_server'"
```

---

## Casos de Uso

### 📌 Caso 1: Monitorear un Sitio Web

```bash
# 1. Ver estadísticas en vivo
sudo vervisitas

# 2. Analizar accesos por hora
sudo tail -1000 /var/log/apache2/access.log | \
  awk -F: '{print $2}' | sort | uniq -c

# 3. Ver top IPs
sudo awk '{print $1}' /var/log/apache2/access.log | sort | uniq -c | sort -rn | head -20

# 4. Generar reporte
sudo bash -c 'echo "=== Accesos Últimas 24h ===" && \
  grep "$(date +%d/%b/%Y)" /var/log/apache2/access.log | wc -l && \
  echo "=== Errores ===" && \
  grep " 4[0-9][0-9] \| 5[0-9][0-9] " /var/log/apache2/access.log | wc -l'
```

### 📌 Caso 2: Investigar Ataque

```bash
# 1. Ver IP atacante
sudo tail -1000 /var/log/apache2/access.log | grep " 404 \| 403 " | awk '{print $1}' | sort | uniq -c | sort -rn

# 2. Ver qué intenta acceder
sudo grep "203.0.113.45" /var/log/apache2/access.log | awk '{print $7}' | sort | uniq

# 3. Bloquear IP
sudo iptables -I INPUT -s 203.0.113.45 -j DROP
sudo iptables-save > /etc/iptables/rules.v4

# 4. Verificar
sudo iptables -L -n | grep 203.0.113.45
```

### 📌 Caso 3: Diagnosticar Problema de Rendimiento

```bash
# 1. Ver URLs más lentas (tiempo en microsegundos)
sudo tail -1000 /var/log/apache2/access.log | \
  awk '{print $7, $(NF-2)}' | sort -k2 -rn | head -20

# 2. Ver correlación con errores
sudo grep " 503 \| 504 " /var/log/apache2/access.log | wc -l

# 3. Verificar carga del servidor en ese momento
sudo journalctl -u apache2 --since "14:00" --until "15:00" | grep -i error
```

### 📌 Caso 4: Auditoría de Accesos

```bash
# 1. Ver quién accedió a /admin
sudo grep "/admin" /var/log/apache2/access.log

# 2. Crear informe
sudo awk -F\" '$2 ~ /\/admin/ {print $2}' /var/log/apache2/access.log | \
  awk '{print $1}' | sort | uniq -c > /tmp/admin-access-report.txt

# 3. Archivar para auditoría
sudo tar czf /backup/audit-$(date +%Y%m%d).tar.gz /var/log/apache2/access.log
```

---

## Troubleshooting

### ❌ Problemas Comunes

| Problema | Causa | Solución |
|----------|-------|----------|
| **"Permission denied"** | Sin permisos de lectura | `sudo` o añadir grupo |
| **Log vacío** | Apache no corriendo | `sudo systemctl start apache2` |
| **No ve nueva entradas** | Buffer sin flush | `sudo tail -f` en vez de `tail` |
| **Scripts no funcionan** | Bash no encontrado | `which bash` |
| **Geolocalización lenta** | API rate limiting | Usar caché o API pagado |

### 🔧 Diagnosticar Problemas

```bash
# Verificar que Apache está corriendo
sudo systemctl status apache2

# Ver logs de Apache
sudo journalctl -u apache2 -n 50 -f

# Verificar permisos
ls -la /var/log/apache2/access.log

# Verificar configuración Apache
sudo apache2ctl configtest

# Ver accesos a script
sudo tail -5 /var/log/apache2/access.log

# Verificar que comando existe
which vervisitas
which verconexiones
```

### 📊 Test de Conectividad

```bash
# Generar tráfico de prueba
curl http://localhost/
curl -H "User-Agent: Test Bot" http://localhost/
ab -n 100 -c 10 http://localhost/  # Apache Bench

# Ver en logs
sudo tail -3 /var/log/apache2/access.log
```

---

## Referencias

### 📚 Documentación Oficial

- 🔗 [Apache Logging](https://httpd.apache.org/docs/current/logs.html) - Documentación oficial
- 🔗 [Apache LogFormat](https://httpd.apache.org/docs/current/mod/mod_log_config.html) - Formatos de log
- 🔗 [RFC 3986 - URI](https://tools.ietf.org/html/rfc3986) - Estándar URI

### 🛠️ Herramientas Relacionadas

| Herramienta | Descripción |
|-------------|-------------|
| **tail/head** | Ver primeras/últimas líneas |
| **grep** | Buscar patrones |
| **awk** | Procesar datos |
| **sed** | Editar streams |
| **cut** | Extraer campos |
| **sort** | Ordenar líneas |
| **uniq** | Eliminar duplicados |
| **wc** | Contar líneas |
| **netstat** | Ver conexiones |
| **curl** | Hacer peticiones HTTP |
| **jq** | Procesar JSON |
| **watch** | Ejecutar periódicamente |
| **logrotate** | Rotar logs |

### 📖 Lectura Recomendada

- 📖 "Linux System Administration" - Evi Nemeth
- 📖 "Apache: The Definitive Guide" - Laurie, Laurie, Dubois
- 📖 "Web Security Testing Cookbook" - Stuttard, Pinto

### 🌐 Sitios Web Útiles

- 🔗 [Apache HTTP Server](https://httpd.apache.org/)
- 🔗 [HTTP Status Codes](https://http.cat/)
- 🔗 [ip-api.com](http://ip-api.com/) - Geolocalización
- 🔗 [Hacking y Seguridad](http://www.hackingyseguridad.com/)

---

## 📁 Estructura del Repositorio

```
vervisitas/
├── README.md                    # Este archivo (mejorado)
├── LICENSE                      # GPL-3.0
├── instalar.sh                  # Script de instalación
├── vervisitas                   # Script principal v1
├── vervisitas2                  # Script mejorado v2
├── verconexiones                # Ver conexiones v1
├── verconxiones.sh              # Ver conexiones v2
├── index.html                   # Dashboard web v1
├── index2.html                  # Dashboard web v2
├── favicon.ico                  # Icono
├── vervisitas.png               # Logo principal
├── gatito.png                   # Imagen decorativa
├── gatito.gif                   # Animación
└── gatito.mp4                   # Video
```

---

## 🚀 Quick Start

### Instalación Rápida (3 pasos)

```bash
# 1. Clonar
git clone https://github.com/hackingyseguridad/vervisitas.git
cd vervisitas

# 2. Instalar
sudo bash instalar.sh

# 3. Usar
sudo vervisitas
```

### Primeros Comandos

```bash
# Ver logs en vivo
sudo tail -f /var/log/apache2/access.log

# Ver estadísticas
sudo vervisitas2

# Ver conexiones
sudo verconexiones

# Abrir dashboard
# Acceder a http://localhost/index.html en navegador
```

---

## ⚖️ Licencia

Este proyecto está bajo licencia **GPL-3.0**. Ver [LICENSE](LICENSE).

---

## 📞 Contacto

- 🌐 **Sitio Web**: [www.hackingyseguridad.com](http://www.hackingyseguridad.com/)
- 📧 **GitHub**: [hackingyseguridad](https://github.com/hackingyseguridad)
- 🐛 **Issues**: [Reportar problema](https://github.com/hackingyseguridad/vervisitas/issues)

---

## 🤝 Contribuciones

Las contribuciones son bienvenidas:

1. Fork el repositorio
2. Crea una rama (`git checkout -b feature/mejora`)
3. Commit cambios (`git commit -am 'Añade mejora'`)
4. Push a rama (`git push origin feature/mejora`)
5. Abre Pull Request

---

## 📈 Estado del Proyecto

| Aspecto | Estado |
|--------|--------|
| Última Actualización | Julio 2026 |
| Versión | 2.1 |
| Mantenimiento | 🟢 Activo |
| Estabilidad | 🟢 Estable |
| Compatibilidad | 🟢 Ubuntu/Debian/CentOS |
| Documentación | 🟢 Completa |

---

## ⭐ Características Futuras

- [ ] Dashboard React/Vue en tiempo real
- [ ] Análisis ML de anomalías
- [ ] Integración con Elasticsearch
- [ ] Alertas por Telegram/Slack
- [ ] Gráficos interactivos con Chart.js
- [ ] API REST
- [ ] Docker containerización
- [ ] Soporte para Nginx
- [ ] Base de datos histórica
- [ ] Reportes automatizados

---

**🎯 Usa VerVisitas para mantener tu servidor web seguro y monitoreado.**

**Hecho con ❤️ por Hacking y Seguridad**
