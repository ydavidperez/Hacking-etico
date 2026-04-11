# 🔍 Módulo 3: Recopilación de Información y Análisis de Vulnerabilidades

> **Fase:** Reconocimiento | **Tipo:** Pasivo y Activo | **Riesgo:** Bajo-Medio

---

## 📌 ¿De qué trata este módulo?

Este módulo cubre la **primera fase crítica de cualquier prueba de penetración**: conocer el objetivo antes de atacarlo. Un hacker ético que omite esta fase está actuando a ciegas. Aquí se aprende a construir un mapa completo del objetivo usando técnicas pasivas (sin contacto directo) y activas (con interacción con los sistemas).

---

## 🗂️ Conceptos Clave

### 1. 🌐 Reconocimiento Pasivo (OSINT)

No se interactúa directamente con el objetivo. Se recopila información desde fuentes públicas:

| Fuente | Qué revela |
|--------|-----------|
| WHOIS | Propietario del dominio, fechas, contactos |
| Google Dorks | Archivos expuestos, paneles de login, errores |
| LinkedIn / Redes sociales | Empleados, tecnologías usadas, estructura |
| Shodan | Dispositivos expuestos a Internet |
| theHarvester | Emails, subdominios, IPs |
| Maltego | Relaciones entre entidades (grafo) |

**Ejemplo de Google Dork:**
```
site:empresa.com filetype:pdf "confidencial"
intitle:"index of" /backup
```

---

### 2. 🎯 Reconocimiento Activo

Se interactúa directamente con los sistemas del objetivo. Implica mayor riesgo de detección:

#### Escaneo con Nmap
```bash
# Descubrir hosts activos en la red
nmap -sn 192.168.1.0/24

# Escaneo de puertos + versiones de servicios
nmap -sV -sC -O 192.168.1.100

# Escaneo agresivo (detección OS + scripts)
nmap -A -T4 192.168.1.100

# Escaneo sigiloso (SYN scan)
nmap -sS 192.168.1.100
```

#### Enumeración de servicios
- **Puerto 21 (FTP):** ¿Acceso anónimo habilitado?
- **Puerto 22 (SSH):** ¿Versión vulnerable?
- **Puerto 80/443 (HTTP/S):** ¿Qué servidor web? ¿CMS?
- **Puerto 445 (SMB):** ¿Versión de Windows? ¿Shares visibles?

---

### 3. 🕵️ Análisis de Vulnerabilidades

Una vez mapeada la superficie de ataque, se identifican debilidades:

#### Scanners de vulnerabilidades
```bash
# Nikto - análisis web
nikto -h http://192.168.1.100

# Nmap con scripts de vulnerabilidades
nmap --script vuln 192.168.1.100
```

#### Bases de datos de referencia
| Recurso | URL | Uso |
|---------|-----|-----|
| CVE | cve.mitre.org | ID único por vulnerabilidad |
| NVD | nvd.nist.gov | Severidad CVSS |
| Exploit-DB | exploit-db.com | PoC y exploits públicos |
| Searchsploit | Local (Kali) | Búsqueda offline de exploits |

```bash
# Buscar exploits por servicio
searchsploit apache 2.4
searchsploit --cve 2021-44228
```

---

## 🧩 Metodología Completa

```
┌─────────────────────────────────────────────┐
│           PROCESO DE RECONOCIMIENTO         │
├──────────────┬──────────────────────────────┤
│   PASIVO     │  OSINT, WHOIS, Shodan        │
│              │  Google Dorks, Redes Soc.    │
├──────────────┼──────────────────────────────┤
│   ACTIVO     │  Nmap, Banner Grabbing       │
│              │  Enumeración de servicios    │
├──────────────┼──────────────────────────────┤
│  ANÁLISIS    │  Nikto, Nessus, OpenVAS      │
│  VULN.       │  CVE lookup, Searchsploit    │
└──────────────┴──────────────────────────────┘
```

---

## 💡 Análisis Personal

> Lo más valioso de este módulo es entender que **la información pública ya es una vulnerabilidad**. Muchas organizaciones exponen sin querer credenciales en repositorios GitHub, estructuras internas en LinkedIn, y servicios no parcheados visibles desde Shodan.
>
> La regla de oro: **cuanta más información recopiles antes de atacar, más efectivo y silencioso será el ataque**. Un reconocimiento pobre lleva a ataques ruidosos y detección temprana.

---

## 📋 Checklist de Reconocimiento

- [ ] Registros DNS y WHOIS revisados
- [ ] Google Dorks ejecutados
- [ ] Shodan consultado
- [ ] Emails y empleados enumerados con theHarvester
- [ ] Escaneo Nmap completado
- [ ] Servicios identificados y versionados
- [ ] Vulnerabilidades mapeadas en CVE/NVD

---

[← README](./README.md) | [Módulo 4 →](./modulo4-ingenieria-social.md)
