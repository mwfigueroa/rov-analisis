# Firmware Open-Source Alternativo para ROVs Submarinos

> Análisis de viabilidad de reemplazar el firmware propietario del QYSEA FIFISH V6 Expert
> por alternativas open-source. Fecha: 2026-07-31

---

## 1. Pregunta principal

> *"¿Existe firmware open-source que pueda reemplazar al original del ROV? ¿Se puede adaptar INAV, Betaflight o hay alguno similar para entorno de agua?"*

**Respuesta corta:** Sí, existe — **ArduSub**. No, Betaflight/INAV no son viables. Pero migrar el FIFISH V6 Expert a ArduSub es un proyecto de ingeniería inversa mayor, no un simple reflash.

---

## 2. ArduSub — El estándar open-source para ROVs

**ArduSub** es un proyecto open-source (GPLv3) dentro del ecosistema **ArduPilot**, derivado originalmente de ArduCopter. Es el firmware más maduro y completo para vehículos submarinos.

| Aspecto | Detalle |
|---|---|
| **Repositorio** | `ArduPilot/ardupilot` (carpeta `ArduSub/`) |
| **Licencia** | GPLv3 |
| **Origen** | Derivado de ArduCopter, adaptado a física submarina |
| **Patrocinador** | Blue Robotics (fabricante del BlueROV) |
| **Estado** | Activo — última release 4.7.0-beta6 (Junio 2026) |
| **Website** | https://ardupilot.org/sub/ |

### 2.1 Capacidades de ArduSub

| Funcionalidad | Detalle |
|---|---|
| **Estabilización de actitud** | AHRS + EKF (sensor fusion IMU + compás + presión) |
| **Depth Hold** | Precisión de pocos cm con sensor de presión externo |
| **Heading Hold** | Mantiene rumbo automáticamente |
| **Position Hold** | Con GPS (superficie) o DVL/acústico (sumergido) |
| **Misiones autónomas** | Waypoints, loiter, circle, RTL, terrain following |
| **Failsafes** | Fuga, temperatura interna, presión interna, pérdida de señal, batería |
| **Control de cámara/gimbal** | Estabilización + control desde joystick |
| **Luces** | Control de intensidad por PWM o MAVLink |
| **Gripper/brazo** | Soporte para servo de garra |
| **Companion computer** | BlueOS (Linux) para video, sonar, IA, OpenCV |
| **GCS** | QGroundControl, Cockpit, Mission Planner |
| **Scripting** | Lua scripting para extender funcionalidad |
| **Logging** | Dataflash logs completos para análisis post-vuelo |

### 2.2 Configuraciones de thrusters soportadas

ArduSub soporta **6 configuraciones predefinidas de frames**, incluyendo una casi idéntica al FIFISH V6:

| Frame | Thrusters | Ejes controlados | Equivalente |
|---|---|---|---|
| **Vectored** | 6 | R / Y / Z / F / L | ⭐ **Más cercano al V6 Expert** |
| BlueROV1 | 6 | R / P / Y / Z / F / L | 6-DOF completo |
| Vectored_6DOF | 8 | R / P / Y / Z / F / L | Heavy configuration |
| SimpleROV-3 | 3 | Y / Z / F | Mínimo |
| SimpleROV-4 | 4 | R / Y / Z / F | Básico |
| SimpleROV-5 | 5 | R / Y / Z / F / L | Intermedio |

> El frame **Vectored** (6 thrusters: 4 horizontales vectorizados + 2 verticales) es arquitectónicamente casi idéntico a la disposición del FIFISH V6 Expert. También admite **configuraciones personalizadas** vía parámetros.

---

## 3. ¿Betaflight / INAV para ROV submarino?

### ⛔ No viables. Razones técnicas:

| Problema | Betaflight | INAV |
|---|---|---|
| **Sensor de profundidad** | ❌ No soporta sensor de presión de agua | ❌ Solo barómetro de aire (inútil sumergido) |
| **Física** | Giroscopio + acelerómetro calibrados para vuelo (gravedad 1G hacia abajo, empuje hacia arriba) | Ídem — asume geometría de ala/hélice aérea |
| **Control de profundidad** | ❌ No existe | ❌ AltHold por barómetro/GPS, no presión hidrostática |
| **Flotabilidad neutra** | ❌ No contempla empuje hidrostático ni lastre variable | ❌ Ídem |
| **PID loop** | Optimizado para cambios rápidos de RPM (aire = baja inercia) | Optimizado para vuelo de ala fija / multirotor |
| **Failsafe de fuga** | ❌ No existe | ❌ No existe |
| **Control de luces/gripper** | Parcial (modos LED/modos servo) | Limitado |
| **Tether/umbilical** | ❌ Telemetría por radio, no por cable | ❌ Ídem |
| **Misiones subacuáticas** | ❌ Solo waypoints GPS aéreos | ❌ Ídem |

### Conclusión sobre Betaflight/INAV

No tiene sentido técnico intentar adaptarlos. Habría que reescribir el 80% del firmware: cambiar el sensor primario de barómetro a presión hidrostática, reescribir el modelo físico (gravedad + empuje + arrastre hidrodinámico), agregar control de profundidad, failsafes de agua, y modificar el loop de control PID para la inercia del agua (~800× más densa que el aire). **Es más fácil empezar desde ArduSub**, que ya resuelve todo esto.

---

## 4. Comparativa: Firmwares open-source para ROV

| Firmware | Madurez | Thrusters | Navegación autónoma | Ecosistema | Hardware soportado |
|---|---|---|---|---|---|
| **ArduSub** | ⭐⭐⭐⭐⭐ Producción | 3–8 | ✅ Completa | QGC, BlueOS, Cockpit | 100+ Pixhawk/Pixhawk-like |
| **PX4** | ⭐⭐ Experimental | Limitado | ✅ Parcial | QGC | Pixhawk |
| **OpenROV** (Cockpit) | ⭐⭐ Legacy | 2–3 | ❌ Manual | Propio | Cape + BeagleBone |
| **BR2_ESP32** | ⭐ Experimental | 4–6 | ❌ Básico | Ninguno | ESP32 DIY |
| **Custom (ROS2)** | ⭐⭐ Variable | Cualquiera | ✅ Con paquete extra | ROS2, Gazebo | SBC Linux |

### ¿Por qué ArduSub es la única opción seria?

1. **Único con soporte activo y comunidad grande** (Blue Robotics + ArduPilot).
2. **Único con 6-thruster vectored frame** predefinido (calce casi directo con el V6).
3. **Único con failsafes acuáticos** (leak, presión interna, temperatura).
4. **Único con depth hold probado en producción** (Bar30 / Keller).
5. **Único con ecosistema completo**: GCS, companion computer, simulación SITL, logging.

---

## 5. ¿Se puede migrar el FIFISH V6 Expert a ArduSub?

### 📋 Lo que se necesitaría

#### A. Identificar el hardware interno del V6 Expert

| Componente | Qué buscar | Complejidad |
|---|---|---|
| **MCU principal** | Probable STM32F4/F7/H7 — abrir y leer el part number | 🔴 Requiere apertura del casco |
| **IMU** | Acelerómetro + giróscopo (ICM-20602, MPU-6000, etc.) | 🔴 Requiere apertura |
| **Compás** | Magnetómetro (HMC5883L, IST8310, etc.) | 🔴 Requiere apertura |
| **Sensor de presión** | Probable MS5837 o Keller (externo, para profundidad) | 🔴 Ver si es accesible sin abrir |
| **ESC/Drivers** | ¿PWM estándar, DShot, o protocolo propietario? | 🔴 Crítico — si es propietario, muy difícil |
| **Pinout** | Asignación de pines del MCU a cada thruster, cámara, luces | 🔴 Ingeniería inversa mayor |
| **Cámara** | Protocolo (MIPI, USB, compuesto) | 🔴 Probablemente propietario |

#### B. Escribir una definición de placa (hwdef) para ArduPilot

Si el MCU es un STM32 soportado por ArduPilot (muy probable), habría que:
- Crear una carpeta `libraries/AP_HAL_ChibiOS/hwdef/QYSEA-V6Expert/`
- Mapear cada pin del MCU a su función (UART, PWM, I2C, SPI)
- Configurar la IMU, compás y barómetro
- Definir los 6 canales PWM para los thrusters

#### C. Adaptar o reemplazar los ESCs

**Este es el punto más crítico.** Si QYSEA usa:
- **PWM estándar** (1100–1900 µs) → ✅ Compatible directo con ArduSub
- **DShot** (protocolo digital estándar) → ✅ Soporte en ArduPilot vía bidireccional
- **Protocolo propietario** (serial/UART/CAN personalizado) → ❌ Necesitarías reemplazar los ESCs

#### D. Periféricos propietarios

| Componente | Probabilidad de compatibilidad |
|---|---|
| Luces LED | Media — suelen ser PWM simple |
| Cámara 4K | Baja — procesamiento de video propietario |
| Joystick/mando | Media — si emite USB HID estándar |
| App móvil | ❌ No compatible — reemplazar por QGC/Cockpit |
| Batería inteligente | ❌ BMS propietario, necesitarías bypass |

### 5.1 Veredicto de viabilidad

| Factor | Evaluación |
|---|---|
| **Viabilidad técnica** | Teóricamente posible, pero requiere ingeniería inversa significativa |
| **Riesgo de brick** | ALTO — flashear firmware incorrecto puede inutilizar la placa |
| **Pérdida de funcionalidad** | Cámara 4K, app, batería inteligente probablemente inutilizables |
| **Tiempo estimado** | 3–6 meses de ingeniería inversa + desarrollo (1 persona dedicada) |
| **¿Vale la pena?** | ❌ NO para este caso — el firmware original funciona; el problema es un thruster dañado, no el firmware |

---

## 6. Alternativa práctica: Usar ArduSub con hardware nuevo

En vez de migrar el V6 Expert, la ruta pragmática es:

### Opción A — Reparar el V6 Expert con firmware original
- ✅ Más rápido, más barato, conserva todas las funcionalidades
- ✅ La falla es de hardware (thruster), no de firmware
- ✅ El firmware original QYSEA es maduro y probado
- ➡️ **Ruta recomendada para este caso**

### Opción B — Construir un ROV con ArduSub desde cero
Si en el futuro se quiere un ROV con firmware abierto:
- **Autopilot:** Pixhawk 6C / Navigator (Blue Robotics) — ~$150–300 USD
- **ESCs:** Basic ESC (Blue Robotics) o BLHeli_S/AM32 con PWM — ~$30–50 c/u
- **Thrusters:** T200 (Blue Robotics) — ~$200 c/u
- **Sensor de profundidad:** Bar30 o Keller — ~$70 USD
- **Companion computer:** Raspberry Pi 4/5 + BlueOS — ~$80 USD
- **GCS:** QGroundControl (gratis, open-source)
- **Total aproximado:** $1,500–2,500 USD en partes (6 thrusters)

---

## 7. Otros proyectos open-source de interés

| Proyecto | Descripción | URL |
|---|---|---|
| **ArduPilot Sub** | Firmware principal para ROVs | https://ardupilot.org/sub/ |
| **BlueOS** | Sistema operativo companion para ROVs | https://blueos.cloud/ |
| **Cockpit** | GCS web moderna (alternativa a QGC) | https://github.com/bluerobotics/cockpit |
| **Ping Sonar** | Firmware open-source para sonar de altitud | https://github.com/bluerobotics/ping-firmware-oss |
| **MAVLink** | Protocolo de comunicación drone-ROV | https://mavlink.io/ |
| **QGroundControl** | Estación de control en tierra | https://qgroundcontrol.com/ |

---

## 8. Resumen y recomendación final

| Pregunta | Respuesta |
|---|---|
| ¿Existe firmware open-source para ROVs? | ✅ **Sí — ArduSub** (maduro, producción, activo) |
| ¿Se puede usar Betaflight/INAV? | ❌ **No** — diseñados para vuelo aéreo, imposible adaptarlos |
| ¿Se puede migrar el V6 Expert a ArduSub? | ⚠️ **Técnicamente posible pero impractico** — requiere ingeniería inversa mayor, riesgo de brick, pérdida de funcionalidades propietarias |
| ¿Qué conviene hacer? | **Reparar el thruster dañado y mantener el firmware original.** El firmware NO es el problema — el hardware (thruster/ESC) sí lo es. |

> **Conclusión:** La mejor inversión de tiempo y dinero es completar el diagnóstico del thruster dañado y enviar el ROV a un centro autorizado QYSEA para reparación. Si en el futuro se desea un ROV con firmware 100% abierto, construir uno basado en ArduSub + Blue Robotics desde cero es más viable que hacer ingeniería inversa del V6 Expert.

---

## 9. Referencias

- [ArduSub — ArduPilot Wiki](https://ardupilot.org/sub/)
- [ArduSub Frame Configurations](https://ardupilot.org/sub/docs/sub-frames.html)
- [ArduPilot GitHub — ArduSub](https://github.com/ArduPilot/ardupilot/tree/master/ArduSub)
- [Blue Robotics — BlueROV2](https://bluerobotics.com/store/rov/bluerov2/)
- [BlueOS Documentation](https://blueos.cloud/docs)
- [QGroundControl](https://docs.qgroundcontrol.com/)
