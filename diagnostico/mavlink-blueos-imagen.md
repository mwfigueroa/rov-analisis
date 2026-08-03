# Configuración MAVLink según la imagen BlueOS Core dc34c974

> Fuente: exploración local de la imagen `bluerobotics/blueos-core:dc34c974` cargada desde `P:\Rov\docker\BlueOs-core-docker-image-dc34c974-amd64.tar`.
> Nota: el contenedor de exploración se usó en modo solo lectura con bash dormido; los servicios no quedaron corriendo.

---

## 1. Arranque de la imagen

El comando por defecto de la imagen es:

```bash
/usr/bin/start-blueos-core && sleep infinity
```

Variables MAVLink clave definidas en `start-blueos-core`:

```bash
MAV_SYSTEM_ID=${MAV_SYSTEM_ID:-1}
MAV_COMPONENT_ID_ONBOARD_COMPUTER4=194
```

Interpretación:

- `MAV_SYSTEM_ID`: system id MAVLink del vehículo, por defecto **1**.
- `MAV_COMPONENT_ID_ONBOARD_COMPUTER4`: component id usado por mavlink2rest/onboard computer, **194**.

---

## 2. Servicios prioritarios relacionados con MAVLink

En `start-blueos-core` aparecen como servicios prioritarios:

| Servicio | Proceso | Rol |
|---|---|---|
| `autopilot` | `/home/pi/services/ardupilot_manager/main.py` | Levanta ArduPilot/ArduSub y el router MAVLink |
| `cable_guy` | `/home/pi/services/cable_guy/main.py` | Configuración de red |
| `video` | `mavlink-camera-manager` | Video y control MAVLink de cámara |
| `mavlink2rest` | `mavlink2rest` | Puente MAVLink hacia HTTP/WebSocket para Cockpit |

Servicios no prioritarios pero relevantes: `kraken`, `wifi`, `zenohd`, `beacon`, `bridget`, `commander`, `nmea_injector`, `helper`, `linux2rest`.

---

## 3. ArduPilot / ArduSub y router MAVLink

`ardupilot_manager` tiene dos responsabilidades:

1. Ejecutar el firmware ArduPilot/ArduSub.
2. Ejecutar un router MAVLink para distribuir mensajes a GCS, video, mavlink2rest, Zenoh y otros endpoints.

Para Navigator/Linux, ArduPilot arranca con master UDP interno:

```text
-A udp:127.0.0.1:8852
```

Flujo:

```text
ArduSub/ArduPilot -> udp:127.0.0.1:8852 -> MAVLink router
```

Para SITL, el master cambia a TCP:

```text
SITL -> tcp:127.0.0.1:5760 -> MAVLink router
```

Para placas seriales, el master es el puerto serial correspondiente.

---

## 4. Endpoints MAVLink por defecto

`ardupilot_manager` define endpoints con tipos: `udpin`, `udpout`, `tcpin`, `tcpout`, `serial`, `zenoh`.

| Endpoint | Tipo | Destino | Estado | Función |
|---|---|---|---|---|
| GCS Client Link | `udpout` | `192.168.2.1:14550` | enabled | Telemetría/comando hacia QGC/Cockpit en superficie |
| GCS Server Link | `udpin` | `0.0.0.0:14550` | disabled | Escuchar GCS entrante; opcional |
| MAVLink2RestServer | `udpin` | `127.0.0.1:14001` | protected | Loopback interno hacia mavlink2rest |
| MAVLink2Rest | `udpout` | `127.0.0.1:14000` | protected | Loopback interno hacia mavlink2rest |
| Internal Link | `tcpin` | `127.0.0.1:5777` | protected | Enlace interno para mavlink-camera-manager |
| Zenoh Daemon | `zenoh` | `0.0.0.0:7117` | protected | Bus interno BlueOS |

Red esperada:

```text
ROV/Raspberry/BlueOS: 192.168.2.2
GCS/laptop:           192.168.2.1
MAVLink GCS UDP:      14550
```

---

## 5. mavlink2rest

Comando observado en `start-blueos-core`:

```bash
mavlink2rest --connect=udpout:127.0.0.1:14001 --server [::]:6040 --system-id 1 --component-id 194
```

Flujo práctico:

```text
MAVLink router <-> loopback 14000/14001 <-> mavlink2rest <-> HTTP/WebSocket [::]:6040 <-> Cockpit
```

Puerto importante para Cockpit/mavlink2rest: **6040**.

---

## 6. Video y MAVLink

Comando observado para `mavlink-camera-manager`:

```bash
mavlink-camera-manager --default-settings BlueROVUDP --mavlink tcpout:127.0.0.1:5777 --mavlink-system-id 1 --mavlink-camera-component-id-range=100-105 --gst-feature-rank omxh264enc=0,v4l2h264enc=250,x264enc=260 --log-path /var/logs/blueos/services/mavlink-camera-manager --stun-server stun://stun.l.google.com:19302 --enable-realtime-threads --verbose
```

Interpretación:

- Video hacia GCS: WebRTC/UDP según `BlueROVUDP`.
- Control MAVLink de cámara: TCP loopback `127.0.0.1:5777`.
- Componentes MAVLink de cámara: rango **100–105**.

---

## 7. Routers MAVLink soportados

`ardupilot_manager` soporta varios routers:

- `MAVLinkRouter` / `mavlink-routerd`
- `MAVP2P`
- `MAVProxy`
- `MAVLinkServer`

Elige el router preferido guardado en settings o el primero disponible. Algunos endpoints son persistentes y otros están protegidos para evitar borrado accidental desde la UI.

---

## 8. Flujo completo simplificado

```text
ArduSub/ArduPilot
   |
   | master interno
   | Navigator/Linux: udp:127.0.0.1:8852
   | SITL:           tcp:127.0.0.1:5760
   v
MAVLink router
   |-- udpout 192.168.2.1:14550  -> QGroundControl/Cockpit
   |-- tcpin 127.0.0.1:5777      -> mavlink-camera-manager
   |-- loopback 14000/14001      -> mavlink2rest -> [::]:6040 -> Cockpit
   |-- zenoh 7117                -> bus interno BlueOS
   `-- udpin 0.0.0.0:14550       -> opcional, GCS entrante, disabled por defecto
```

---

## 9. Valores importantes para el proyecto

```text
ROV IP:              192.168.2.2
GCS IP:              192.168.2.1
MAVLink system id:   1
GCS UDP:             14550
mavlink2rest:        6040
camera manager MAV:  127.0.0.1:5777
camera components:   100-105
Navigator master:    udp:127.0.0.1:8852
SITL master:         tcp:127.0.0.1:5760
Zenoh:               7117
```

---

## 10. Referencias internas

- `diagnostico/diagrama-ardusub-bloques.md`
- `diagnostico/rpi4-imagen-interfaces-firmware.md`
- `diagnostico/bom-final-nivel-b.md`
- `diagnostico/ardusub-adaptacion-fifish-v6.md`
