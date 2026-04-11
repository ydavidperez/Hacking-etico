# 🎭 Módulo 4: Ataques de Ingeniería Social

> **Fase:** Explotación humana | **Vector:** Psicología | **Efectividad:** Alta (>90% de brechas involucran este factor)

---

## 📌 ¿De qué trata este módulo?

La ingeniería social es la **explotación del factor humano**: convencer a personas para que realicen acciones o revelen información que comprometa la seguridad. No importa cuán robusto sea el firewall si un empleado entrega sus credenciales voluntariamente.

> *"El eslabón más débil en la cadena de seguridad es el ser humano."* — Kevin Mitnick

---

## 🧠 Principios Psicológicos Explotados

Los atacantes aprovechan sesgos cognitivos profundamente arraigados:

| Principio | Descripción | Ejemplo de ataque |
|-----------|-------------|-------------------|
| **Autoridad** | Obedecemos a figuras de autoridad | "Soy el CTO, necesito tu contraseña ahora" |
| **Urgencia** | La presión temporal nubla el juicio | "Tu cuenta será bloqueada en 10 minutos" |
| **Reciprocidad** | Sentimos obligación de devolver favores | Dar algo gratis antes de pedir algo |
| **Escasez** | Lo limitado parece más valioso | "Solo quedan 2 accesos disponibles" |
| **Prueba social** | Seguimos lo que otros hacen | "Todos en tu empresa ya actualizaron" |
| **Simpatía** | Confiamos más en quien nos agrada | Construir relación antes de atacar |

---

## 🎯 Tipos de Ataques

### 1. 📧 Phishing

El más prevalente. Emails falsos que imitan entidades legítimas:

```
Tipos:
├── Phishing masivo     → millones de correos genéricos
├── Spear Phishing      → dirigido a persona específica
├── Whaling             → dirigido a ejecutivos (CEO, CFO)
├── Vishing             → por voz/teléfono
└── Smishing            → por SMS
```

**Indicadores de un email de phishing:**
- Dominio similar pero diferente: `paypa1.com`, `arnazon.com`
- Urgencia artificial en el asunto
- Links con redirecciones ocultas
- Adjuntos `.exe`, `.doc` con macros

---

### 2. 🛠️ Social Engineering Toolkit (SET)

Herramienta de Kali Linux para automatizar ataques de ingeniería social:

```bash
# Lanzar SET
sudo setoolkit

# Opciones principales:
# 1) Social-Engineering Attacks
#   > 2) Website Attack Vectors
#     > 3) Credential Harvester Attack
#       > 2) Site Cloner → clonar sitio web legítimo
```

**Flujo del ataque con SET:**
```
Atacante clona web legítima → Víctima ingresa credenciales
→ SET captura datos → Redirige a sitio real (víctima no nota nada)
```

---

### 3. 🪤 Pretexting

Crear un escenario falso creíble para obtener información:

**Ejemplo de script de pretexting:**
```
Atacante llama al helpdesk haciéndose pasar por empleado:
"Hola, soy Juan Pérez del área de ventas, estoy viajando
y no puedo acceder al sistema. ¿Puede resetear mi contraseña?
Mi jefe necesita el reporte urgente..."
```

---

### 4. 💿 Baiting (Cebo)

Dejar dispositivos infectados en lugares estratégicos:
- USB con malware en el estacionamiento de la empresa
- CD con etiqueta "Nómina Q4 - Confidencial"
- La curiosidad humana hace el resto

---

### 5. 🚪 Tailgating / Piggybacking

Acceso físico no autorizado siguiendo a alguien con acceso legítimo:
- Cargar cajas grandes para que alguien abra la puerta
- Vestimenta de técnico o proveedor

---

## 🛡️ Contramedidas y Concienciación

```
DEFENSA TÉCNICA          DEFENSA HUMANA
─────────────────        ──────────────────────
Filtros antispam         Capacitación continua
MFA habilitado           Cultura de verificación
DMARC/SPF/DKIM           Política "verificar siempre"
Sandboxing adjuntos      Simulacros de phishing
```

**Señales de alerta que todo empleado debe conocer:**
- [ ] ¿Me están pidiendo algo inusual?
- [ ] ¿Hay presión de tiempo artificial?
- [ ] ¿Puedo verificar la identidad por otro canal?
- [ ] ¿El email o dominio es exactamente el correcto?

---

## 💡 Análisis Personal

> Este módulo fue el más impactante desde el punto de vista humano. La tecnología puede parchearse; los sesgos cognitivos humanos son prácticamente inmutables. Lo más preocupante es que **los ataques de ingeniería social no requieren conocimiento técnico avanzado** — cualquier persona con habilidades de comunicación puede ejecutarlos.
>
> La defensa real no es técnica: es **cultura organizacional**. Una empresa donde "está bien preguntar y verificar" es mucho más segura que una donde la jerarquía impide cuestionar órdenes de "superiores".

---

[← Módulo 3](./modulo3-recopilacion-vulnerabilidades.md) | [Módulo 5 →](./modulo5-explotacion-redes.md)
