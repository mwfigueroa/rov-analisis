# Imagen para Raspberry Pi 4, placas de interfaz, microcontroladores y firmware para ArduSub

> Contexto: BOM Nivel B con Blue Robotics Navigator + Raspberry Pi 4 + BlueOS.
> Respuesta corta: para la arquitectura Navigator se usa **BlueOS para Raspberry Pi 4**. No se usa Raspberry Pi OS desktop como base. El stack de control se completa con ArduSub gestionado por BlueOS y Cockpit/QGroundControl en superficie.

---

## 1. Que imagen se usa en la Raspberry Pi 4

Usar **BlueOS** para Raspberry Pi 4, orientada al ecosistema Blue Robotics.

### Link de imagen y alcance real

- Descarga/instalacion BlueOS: https://blueos.cloud/docs/stable/usage/installation/
- Releases e imagenes adicionales: https://github.com/bluerobotics/BlueOS/releases
- Guia Navigator Software Setup: https://bluerobotics.com/learn/navigator-software-setup/
- Getting started BlueOS: https://blueos.cloud/docs/stable/usage/getting-started/

Para Raspberry Pi 4B, la documentacion de BlueOS indica imagen **ARMv7 32-bit Bullseye** como estandar en vehiculos Blue Robotics. Para Raspberry Pi 5 lista ARMv8 64-bit Bookworm con pruebas limitadas; para este proyecto se eligio Raspberry Pi 4 por compatibilidad declarada con Navigator.

Importante: existe imagen lista de **BlueOS**, pero no una imagen completa ya configurada para tu ROV/ArduSub. El flujo correcto es: flashear BlueOS, entrar a la interfaz web, actualizar, ir a **Vehicle > Firmware**, seleccionar **ArduSub** y luego configurar sensores, power, frame y failsafes en QGroundControl/Cockpit. La guia de Navigator indica minimo **ArduSub 4.5.1** para Navigator y recomienda usar latest stable si no se sabe que version elegir.

Notas importantes:

- El kit de Navigator declara que incluye una microSD con BlueOS. Igual conviene saber reinstalarla.
- BlueOS no es solo un sistema operativo: es la capa companion para gestionar autopilot, video, extensiones, red y actualizaciones.
- No instalar primero Raspberry Pi OS desktop y despues intentar convertirlo en el stack final salvo que se acepte una integracion manual mas compleja.

Flujo recomendado:

1. Grabar BlueOS en microSD para Raspberry Pi 4.
2. Montar Navigator sobre la Raspberry Pi 4 con su heatsink.
3. Alimentar con fuente 5 V adecuada.
4. Actualizar BlueOS.
5. Instalar/seleccionar firmware ArduSub desde BlueOS.
6. Conectar Cockpit o QGroundControl.
7. Registrar versiones de BlueOS, ArduSub, Cockpit/QGC y parametros.

---

## 2. Arquitectura A: Navigator como placa de interfaz

En esta arquitectura, la Raspberry Pi 4 actua como cerebro Linux y la Navigator como placa de sensores/PWM.

### Placas requeridas

| Placa / interfaz               | Rol                                                        |                   Obligatoria para MVP |
| ------------------------------ | ---------------------------------------------------------- | -------------------------------------: |
| Navigator Flight Controller    | Sensores onboard, PWM, ADC, leak probes, puertos serie/I2C |                                     Si |
| Raspberry Pi 4                 | Ejecuta BlueOS y el stack de control                       |                                     Si |
| Fuente 5V 6A                   | Alimentacion estable de Pi/Navigator/perifericos           |                                     Si |
| Power Sense Module             | Medicion de voltaje/corriente                              |                            Recomendado |
| Bar30 / MS5837                 | Profundidad para depth hold                                |                     Si para depth hold |
| 6 x Basic ESC                  | Control de los 6 thrusters                                 |                                     Si |
| sta                           | Ethernet por tether de largo alcance                       | Si si el tether no es Ethernet directo |
| Camara Pi/USB/IP               | FPV/video                                                  |                            Si para FPV |
| Sondas SOS leak                | Deteccion de fuga                                          |                            Recomendado |
| Driver de luces / MOSFET       | Control seguro de LEDs                                     |                       Si se usan luces |
| JST GH cables/adaptadores      | Conexion de sensores y expansions                          |                      Segun componentes |
| I2C level converter / splitter | Solo si algun dispositivo I2C lo requiere                  |                               Opcional |
| DVL/USBL/GPS                   | Navegacion avanzada                                        |                               Opcional |

### Microcontroladores y firmware en arquitectura Navigator

| Elemento                       | Que es                                   | Firmware                                                     | Como se carga                                                             |
| ------------------------------ | ---------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------- |
| Raspberry Pi 4                 | Computadora Linux, no un MCU tradicional | BlueOS + servicios + ArduPilot/ArduSub gestionado por BlueOS | Imagen en microSD; actualizacion por BlueOS                               |
| Navigator                      | Placa de interfaz con sensores e ICs     | No se flashea como autopilot por el usuario                  | Sus ICs trabajan como perifericos de la Pi                                |
| PCA9685 en Navigator           | Generador PWM                            | Firmware interno del IC                                      | No se flashea; lo usa el stack                                            |
| ADS1115 en Navigator           | ADC                                      | Firmware interno del IC                                      | No se flashea                                                             |
| IMU/compas/barometro Navigator | Sensores                                 | Firmware interno del sensor                                  | No se flashea                                                             |
| Basic ESC                      | Controlador de motor BLDC                | BLHeli_S                                                     | Normalmente no se toca; si se cambia, hacerlo con cuidado y documentacion |
| Fathom-X/HomePlug              | Interfaz Ethernet por par                | Firmware del modulo                                          | No se flashea para uso normal                                             |
| Cockpit/QGroundControl         | GCS en superficie                        | App de escritorio/web                                        | Instalacion en laptop/tablet                                              |

Punto clave: con Navigator no hay un Pixhawk/STM32 separado ejecutando ArduPilot. La integracion depende de BlueOS/ArduPilot corriendo en la Raspberry Pi y de la Navigator como capa de E/S.

---

## 3. Arquitectura B: Pixhawk separado como microcontrolador

Si se quiere un microcontrolador clasico de autopilot, la alternativa es usar Pixhawk.

### Placas requeridas

| Placa / interfaz                     | Rol                                     |
| ------------------------------------ | --------------------------------------- |
| Pixhawk 6C/6X u otra placa ArduPilot | MCU principal que ejecuta ArduSub       |
| Raspberry Pi 4 + BlueOS              | Companion para video, red y extensiones |
| Power module compatible              | Medicion y a veces alimentacion         |
| Bar30                                | Profundidad                             |
| Basic ESC / ESC compatibles          | Motores                                 |
| Fathom-X                             | Tether                                  |
| Camara                               | Video                                   |
| Sensores opcionales                  | DVL, GPS superficie, USBL               |

### Firmware en arquitectura Pixhawk

| Elemento                   | Firmware                        | Como se carga                                        |
| -------------------------- | ------------------------------- | ---------------------------------------------------- |
| Pixhawk STM32H7 u otro MCU | ArduSub/ArduPilot sobre ChibiOS | QGroundControl, Mission Planner o BlueOS segun setup |
| Raspberry Pi 4             | BlueOS companion                | Imagen microSD                                       |
| ESC                        | BLHeli_S/AM32 segun modelo      | Solo si se necesita configurar; documentar           |
| GCS                        | Cockpit/QGroundControl          | App de escritorio                                    |

Esta arquitectura es mas parecida a drones clasicos: MCU de tiempo real para control y companion Linux para video/red.

---

## 4. Conexionado logico recomendado

```text
Bateria 4S -> fusible -> Power Sense Module -> distribucion
Distribucion -> 6 x Basic ESC -> 6 x T200
Distribucion -> 5V 6A BEC -> Navigator + Raspberry Pi 4
Raspberry Pi 4 -> Navigator HAT
Navigator PWM -> senal de 6 x Basic ESC
Navigator I2C -> Bar30
Navigator leak -> sondas SOS
Camara CSI/USB -> Raspberry Pi / BlueOS
Fathom-X ROV <-> Ethernet Raspberry Pi
Fathom-X superficie <-> laptop GCS
Cockpit/QGroundControl <-> MAVLink -> ArduSub
```

---

## 5. Errores comunes a evitar

- Usar Raspberry Pi OS desktop como base en vez de BlueOS para el stack Blue Robotics.
- Alimentar Navigator/Raspberry Pi desde una fuente 5 V debil.
- Asumir que Navigator tiene un STM32/Pixhawk interno.
- Flashear ESC BLHeli_S sin necesidad ni backup.
- Mezclar voltajes de senal sin verificar: PWM 3.3-5 V segun ESC/autopilot.
- Instalar Bar30 sin entender que la cara trasera no es estanca por si sola.
- No configurar failsafes antes de agua.
- No registrar versiones de BlueOS/ArduSub/Cockpit/QGC.
- Comprar Raspberry Pi 5 asumiendo compatibilidad total con Navigator sin revisar revision/notas del fabricante. Para el BOM se uso Raspberry Pi 4 porque la pagina de Navigator declara compatibilidad con Raspberry Pi 4.

---

## 6. Checklist de imagen y firmware

- [ ] microSD grabada con BlueOS para Raspberry Pi 4.
- [ ] Navigator montada y alimentada correctamente.
- [ ] BlueOS actualizado.
- [ ] ArduSub instalado/seleccionado.
- [ ] Frame configurado: Vectored 6 o custom.
- [ ] 6 salidas PWM mapeadas a ESC.
- [ ] Bar30 detectado por I2C.
- [ ] Power Sense Module detectado y calibrado.
- [ ] Leak probes detectadas.
- [ ] Camara visible en BlueOS/Cockpit/QGC.
- [ ] Fathom-X enlace estable.
- [ ] Failsafes configurados y probados.
- [ ] Versiones y parametros guardados en el repo.

---

## 7. Referencias internas

- `diagnostico/bom-final-nivel-b.md`
- `diagnostico/componentes-nivel-b-especificaciones.md`
- `diagnostico/diagrama-ardusub-bloques.md`
- `diagnostico/ardusub-adaptacion-fifish-v6.md`
