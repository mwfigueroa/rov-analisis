# Adaptacion ArduSub / ArduPilot para un ROV correlativo al FIFISH V6 Expert

> Objetivo: definir una arquitectura ArduSub/ArduPilot que sea funcionalmente equivalente al QYSEA FIFISH V6 Expert, sin asumir que el firmware propietario pueda reemplazarse directamente.
> Regla principal: no abrir ni modificar el V6 Expert hasta resolver garantia/RMA. La adaptacion se plantea primero como banco de pruebas y prototipo paralelo.

---

## 1. Idea central

No conviene pensar en "flashear ArduSub dentro del FIFISH". Conviene pensar en construir una **version correlativa**: mismas funciones del V6 Expert, pero con componentes conocidos y soportados por ArduSub.

La equivalencia no es por marca, sino por funcion:

| Funcion del V6 Expert | Equivalente ArduSub/ArduPilot |
|---|---|
| Controlador propietario QYSEA | Autopilot con ArduSub: Navigator, Pixhawk 6C/6X u otra placa soportada |
| App FIFISH + mando QYSEA | QGroundControl, Cockpit o Mission Planner + joystick/gamepad |
| Tether propietario | Ethernet/UDP sobre tether usando companion computer con BlueOS |
| 6 thrusters Q-Motor | 6 thrusters/ESC estandar en frame Vectored o frame custom |
| Sensor de profundidad original | Sensor de presion MS5837/Bar30 o Keller, segun rating de profundidad |
| Depth lock / heading lock | ArduSub depth hold + attitude/heading hold |
| Bateria inteligente QYSEA | Bateria 3S/4S + power module MAVLink + BMS externo |
| Camara 4K propietaria | Camara USB/CSI/IP soportada por BlueOS/QGC, o payload independiente |
| LEDs 6000 lm | Driver de LEDs controlado por PWM/relay desde canaux |
| Q-Interface/accesorios | Servos/relays/MAVLink payloads estandar |

---

## 2. Arquitectura objetivo recomendada

### Version A: banco de pruebas correlativo, sin tocar el V6

Objetivo: aprender y validar ArduSub con la misma logica de propulsion del V6.

Componentes:
- Autopilot ArduSub: Navigator o Pixhawk soportado.
- Companion computer: Raspberry Pi 4/5 con BlueOS.
- ESC: 6 unidades PWM estandar, por ejemplo Basic ESC de Blue Robotics o equivalentes.
- Thrusters: 6 unidades conocidas, idealmente 4 vectorizados + 2 verticales.
- Sensor de presion: Bar30/MS5837 para validar depth hold.
- Power module: medicion de voltaje/corriente por MAVLink.
- Bateria de banco: 3S inicialmente, por correlacion con el cargador QYSEA de 12.6 V, sujeto a verificacion de ESC/thruster.
- GCS: QGroundControl o Cockpit.
- Joystick: gamepad USB estandar.

### Version B: ROV prototipo correlativo

Objetivo: reproducir capacidades del V6 Expert en un casco nuevo o frame de prueba.

Agregar:
- Casco estanco nuevo o frame abierto para pruebas en pileta/agua dulce.
- Penetradores estancos para tether, sensores y thrusters.
- Sensor de presion con rating acorde al objetivo: 100 m o 200 m.
- Sensor de fuga.
- Camara submarina estandar.
- Luces con driver adecuado.
- Si se necesita position hold real: DVL, USBL/acustica o odometria visual. GPS solo sirve en superficie.

### Version C: canibalizacion parcial del V6 Expert

Solo si el equipo queda fuera de garantia y se acepta perder funciones propietarias.

Reutilizables potenciales:
- Tether fisico, si se confirma pares para Ethernet/comunicacion y alimentacion.
- Domo/optica, como pieza mecanica.
- LEDs, si se confirma voltaje/corriente y se agrega driver propio.
- Thrusters Q-Motor, solo si se confirma que aceptan ESC estandar o si se reemplaza el motor completo manteniendo la vaina.
- Bateria, solo como pack de celdas; la parte inteligente/BMS probablemente no sera compatible.

No asumir compatibilidad de:
- Camara 4K.
- App FIFISH.
- BMS inteligente.
- Q-Interface.
- U-QPS/accesorios propietarios.

---

## 3. Mapa correlativo de componentes

| Subsistema FIFISH V6 Expert | Dato conocido del manual/proyecto | Componente ArduSub correlativo | Reusar del V6? | Riesgo/nota |
|---|---|---|---|---|
| Control principal | Firmware/app propietarios | Autopilot ArduSub + companion BlueOS | No | Requiere red completa MAVLink |
| Propulsion | 6 thrusters Q-Motor Tech; manual indica 4 x Vector + 2 x Horizontal | Frame ArduSub Vectored 6 o custom | Tal vez, con pruebas | Confirmar si el motor es BLDC estandar y donde esta el ESC |
| ESC/drivers | Dentro del casco, protocolo desconocido | 6 ESC PWM estandar | No inicialmente | Si QYSEA usa protocolo propietario, no reutilizar ESC |
| Profundidad | Versiones M100/M200 | MS5837/Bar30 o Keller con rating correcto | No | El sensor debe soportar 100/200 m y estar bien penetrado |
| Estabilizacion | Depth lock, heading lock, IMU | ArduSub AHRS/EKF + depth hold | No | Requiere calibracion de IMU/compas y tuning en agua |
| Bateria | 156 Wh, Panasonic 21700; cargador ROV 12.6 V 10 A | Pack 3S/4S + power module + BMS | Parcial | La informacion inteligente de bateria probablemente se pierde |
| Carga | Cargador dedicado 12.6 V | Cargador acorde a quimica y BMS elegido | Tal vez | No cargar pack modificado sin BMS conocido |
| Camara | 4K UHD, 12 MP, EIS | Camara USB/CSI/IP en BlueOS | Probablemente no | Si es MIPI/propietaria, tratarla como payload cerrado |
| LEDs | 6000 lm, 5500 K | Driver LED PWM/relay | Tal vez | Confirmar voltaje, corriente y disipacion |
| Mando | RC QYSEA con sticks/wheels | Joystick USB + QGC/Cockpit | No | Se pierde integracion original |
| Tether | 100 m, con carrete | Ethernet sobre tether + BlueOS topside/ROV | Tal vez | Confirmar pares, blindaje y conectores |
| Acustica/posicion | U-QPS opcional | DVL, USBL, GPS solo superficie | No | Position hold submarino exige sensor adicional |
| Gripper/accesorios | Q-Interface 9-12 V, 5 A max | Servo/relay/aux PWM o payload MAVLink | No | Q-Interface no es estandar ArduSub |
| Failsafes | App/estado propietario | ArduSub FS leak/temp/pressure/battery/telemetry | No | Hay que implementar y probar cada failsafe |

---

## 4. Que conviene adaptar primero

Orden recomendado de validacion:

1. Autopilot + BlueOS en banco.
2. Joystick + QGroundControl/Cockpit.
3. Un ESC + un thruster: direccion, aceleracion, consumo, temperatura.
4. Sensor de presion: depth hold en balde/pileta.
5. Power module: voltaje/corriente y failsafe de bateria.
6. Seis thrusters en frame Vectored: mezcla de motores y ejes.
7. Luces por canal auxiliar.
8. Camara por BlueOS.
9. Failsafe de fuga y corte por perdida de telemetria.
10. Solo despues: evaluar reutilizar piezas del V6.

---

## 5. Configuracion ArduSub esperada

Sin fijar valores numericos hasta tener hardware real:

- Seleccionar frame tipo Vectored de 6 thrusters si la geometria coincide con el V6.
- Si la geometria real difiere, usar frame custom y definir la matriz de mezcla.
- Mapear canales de motores y verificar direccion de giro antes de poner helices.
- Configurar bateria: voltaje, corriente, capacidad, failsafe.
- Configurar sensor de presion externo y profundidad maxima permitida.
- Configurar failsafes: perdida de telemetria, bateria baja, fuga, temperatura interna.
- Limitar salida maxima de motores en las primeras pruebas.
- Registrar logs para comparar consumo, temperatura y estabilidad contra el comportamiento del V6.

---

## 6. Correlacion de modos de vuelo/operacion

| Funcion QYSEA | Equivalente ArduSub | Nota |
|---|---|---|
| Attitude mode | Stabilize / Alt Hold segun configuracion | Attitude/heading estabilizado |
| Depth Holding | Depth Hold | Necesita sensor de presion correcto |
| Sport mode | Manual/Acro segun exposicion de controles | Usar con cuidado en agua |
| Combination/VR | No equivalente directo | Se reemplaza por GCS/joystick o desarrollo propio |
| Auto Pilot/Assist Driving | Guided/Auto/Scripting Lua | Requiere navegacion y sensores adicionales |
| Posture Lock | Attitude/heading hold | Ajustar ganancias en agua |
| Depth Lock | Depth Hold | Validar deriva y compensacion de flotabilidad |

---

## 7. BOM orientativo para prototipo correlativo

| Bloque | Componente sugerido | Funcion |
|---|---|---|
| Autopilot | Navigator o Pixhawk soportado por ArduSub | Ejecutar ArduSub |
| Companion | Raspberry Pi 4/5 + BlueOS | Video, MAVLink routing, tether Ethernet |
| ESC | 6 ESC PWM estandar | Controlar thrusters |
| Propulsion | 6 thrusters tipo T100/T200 o equivalentes | Misma arquitectura 4 vector + 2 vertical |
| Profundidad | Bar30/MS5837 o Keller | Depth hold y limite de profundidad |
| Potencia | Power module + BMS + bateria 3S/4S | Medicion y failsafe electrico |
| Fuga | Sensor de fuga | Failsafe acuatico |
| Camara | USB/CSI/IP compatible BlueOS | FPV y grabacion basica |
| Luces | LED submarino + driver PWM | Iluminacion controlada |
| Mecanica | Frame, casco estanco, penetradores | Integracion y sellado |
| GCS | Laptop + QGroundControl/Cockpit + joystick | Operacion |

---

## 8. Criterios go/no-go para tocar el V6 Expert

No avanzar con modificacion del V6 si falta cualquiera de estos puntos:

- Garantia/RMA resuelta o explicitamente descartada.
- Fotos y part numbers de placa principal, IMU, compas, sensor de presion y drivers.
- Confirmacion de protocolo de ESC: PWM, DShot o propietario.
- Esquema de alimentacion y voltajes de cada subsistema.
- Plan de re-sellado y prueba de estanqueidad.
- Backup de configuracion original y posibilidad de volver atras.
- Repuesto o placa donante para no inutilizar el ROV.

Si falta mas de uno, seguir con prototipo paralelo.

---

## 9. Riesgos principales

- Perdida de garantia/postventa por apertura no autorizada.
- Brick del controlador original.
- Incompatibilidad de camara, bateria inteligente y accesorios.
- Dano por sal si se abre sin control de corrosion.
- Fuga a profundidad si se altera el sellado sin re-certificacion.
- Riesgo termico si se reutiliza bateria Li-ion expuesta a sal.

---

## 10. Recomendacion practica

Para este proyecto, la ruta mas sensata es doble:

1. Reparar el V6 Expert por RMA y mantener firmware original.
2. En paralelo, construir un banco/prototipo ArduSub correlativo para aprender la arquitectura: autopilot, BlueOS, ESC/thrusters, depth hold, power module y failsafes.

Solo despues de dominar el prototipo se evalua si alguna pieza del V6 puede reutilizarse sin comprometer seguridad ni estanqueidad.

---

## 11. Referencias

- ArduSub: https://ardupilot.org/sub/
- ArduPilot GitHub: https://github.com/ArduPilot/ardupilot
- BlueOS: https://blueos.cloud/
- QGroundControl: https://qgroundcontrol.com/
- Cockpit: https://github.com/bluerobotics/cockpit
- Documento relacionado: `diagnostico/firmware-open-source-rov.md`
- Diagnostico principal: `diagnostico/fifish-v6-expert.md`
