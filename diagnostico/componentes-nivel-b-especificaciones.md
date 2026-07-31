# Componentes Nivel B con especificaciones: ROV correlativo ArduSub / ArduPilot

> Alcance: documentar componentes candidatos para una conversion Nivel B del FIFISH V6 Expert: reemplazar control, potencia, sensores, video y comunicaciones; conservar mecanica/casco solo si pasan inspeccion.
> Advertencia: las especificaciones comerciales cambian. Antes de comprar, verificar la hoja de datos del fabricante. Este documento define requisitos y candidatos, no una orden de compra final.

---

## 1. Supuestos de diseno

| Parametro | Objetivo inicial | Nota |
|---|---|---|
| Profundidad objetivo | 100 m; escalable a 200 m | Sensor, casco y penetradores deben tener margen |
| Propulsion | 6 thrusters | Geometria Vectored 6 o custom segun medicion del V6 |
| Control | ArduSub estable | Version congelada por proyecto |
| Companion | BlueOS | Video, MAVLink routing, tether |
| GCS | QGroundControl o Cockpit | Joystick USB |
| Bateria | Nueva | No reutilizar pack sospechoso de sal |
| Voltaje | 3S o 4S a definir | Debe coincidir con ESC/thrusters; no asumir por el cargador QYSEA |
| Video | Camara estandar | No depender de la camara 4K propietaria |
| Accesorios | PWM/relay/UART/CAN/Ethernet documentados | Q-Interface no se asume compatible |

---

## 2. Autopilot / controlador de vuelo

| Candidato | Rol | Especificaciones a exigir | Verificar antes de comprar |
|---|---|---|---|
| Blue Robotics Navigator | Autopilot orientado a BlueROV/ArduSub | Soporte ArduSub, salidas PWM suficientes para 6 thrusters + auxiliares, IMU, interfaces para sensores, integracion con BlueOS | Arquitectura exacta de la version: si incluye companion/CM4 o solo controlador; cantidad de PWM; sensores onboard; alimentacion |
| Pixhawk 6C / 6X | Autopilot generico ArduPilot | MCU soportado, IMU redundante, UART/I2C/CAN/PWM suficientes, buen soporte ArduSub | Necesita companion separado para BlueOS/video; cableado y alimentacion |
| Otra placa ArduPilot | Opcion economica | Soporte ArduSub confirmado, 6+ salidas de motor, buses para Bar30/leak/power | Documentacion de pines y bootloader |

Requisito minimo: 6 salidas de motor estables, al menos 1 I2C para sensor de presion, 1 entrada para fuga, telemetria al companion y logs completos.

---

## 3. Companion computer / comunicaciones

| Candidato | Rol | Especificaciones a exigir | Nota |
|---|---|---|---|
| Raspberry Pi 5 | Companion BlueOS | 4-8 GB RAM recomendado, Ethernet, USB, CSI si se usa camara Pi, buen disipador | Mas consumo; verificar alimentacion y termica |
| Raspberry Pi 4 | Companion BlueOS | 4 GB RAM minimo recomendado, Ethernet, USB, CSI | Opcion madura |
| Compute Module 4 / integrado | Compacto | Segun carrier/Navigator | Menos flexible, mas integrado |

Software:
- BlueOS como capa de companion.
- MAVLink hacia autopilot.
- Video hacia QGroundControl/Cockpit.
- Tether por Ethernet/UDP preferido; serial solo como fallback.

Prueba obligatoria: throughput real por el tether elegido durante al menos 10 minutos con video y telemetria simultaneos.

---

## 4. ESC para thrusters

| Candidato | Uso | Especificaciones a exigir | Riesgo si no se cumple |
|---|---|---|---|
| ESC recomendado por el fabricante del thruster | Opcion preferida | Mismo voltaje que bateria, corriente continua con margen, reversible/bidireccional, PWM o DShot soportado por ArduSub | Sobrecalentamiento, mala reversa, mezcla inestable |
| Blue Robotics Basic ESC u equivalente | Si se usan thrusters tipo T100/T200 | Compatibilidad explicita con el thruster elegido | Incompatibilidad de arranque o corriente |
| BLHeli_S / AM32 reversible | Opcion generica | Modo reversible/3D o bidireccional real, calibracion documentada, BEC aislado si aplica | La reversa puede ser asimetrica o lenta |

Requisitos electricos:
- Corriente continua del ESC mayor que la corriente maxima esperada del thruster con margen. Objetivo: margen de al menos 1.5x si no hay dato de banco.
- Voltaje compatible con la bateria elegida.
- Entrada de control compatible con ArduSub sin logica propietaria.
- Si el ESC esta fuera del pod del thruster, la penetracion debe soportar corriente y sellado.

No reutilizar ESC QYSEA salvo protocolo confirmado.

---

## 5. Thrusters

| Opcion | Especificaciones a exigir | Cuando usarla |
|---|---|---|
| Thrusters nuevos tipo T100/T200 o equivalentes | Empuje conocido, curva consumo/empuje publicada, ESC recomendado, repuestos disponibles | Opcion recomendada para Nivel B predecible |
| Reutilizar Q-Motor del V6 | Resistencias fase-fase equilibradas, aislamiento correcto, rodamientos sanos, ESC nuevo compatible | Solo si pasan ensayo completo |
| Otros thrusters submarinos | Rating de profundidad, conector/penetrador, corriente y empuje conocidos | Si hay disponibilidad local |

Datos que hay que medir o conseguir:
- empuje maximo y empuje util a voltaje elegido;
- corriente maxima y corriente tipica;
- sentido de giro y helice correcta;
- resistencia fase-fase;
- aislamiento a carcasa;
- temperatura tras banco con refrigeracion adecuada.

Criterio: no mezclar thrusters de origen distinto en el mismo lazo critico si no se conocen curvas equivalentes.

---

## 6. Sensor de presion / profundidad

| Candidato | Especificaciones a exigir | Nota |
|---|---|---|
| Bar30 / MS5837-30BA | Rango suficiente para 100/200 m con margen, interfaz I2C soportada por ArduSub, capsula/penetracion correcta | Opcion habitual en ROVs |
| Keller u otro industrial | Rango y precision superiores, salida soportada o adaptador | Para mayor exigencia o profundidad |

Requisito: el rating del sensor debe superar la profundidad objetivo con margen de ingenieria. Para 200 m, no usar un sensor justo al limite.

---

## 7. Sensor de fuga y temperatura

| Sensor | Especificaciones a exigir | Integracion |
|---|---|---|
| Leak/conductividad | Salida digital o analogica clara, inmune a falsos positivos por condensacion | Entrada de autopilot o companion; failsafe probado |
| Temperatura interna | Rango adecuado, lectura estable | Log y alarma; no confiar solo en temperatura de bateria |

Failsafe minimo: alarma + log. Failsafe fuerte: recortar motores y subir, solo si la mecanica y la mision lo permiten.

---

## 8. Bateria, BMS y power module

| Elemento | Especificaciones a exigir | Decision pendiente |
|---|---|---|
| Bateria | Nueva, quimica conocida, BMS conocido, capacidad suficiente, descarga adecuada para 6 thrusters + electronica | 3S correlativo vs 4S de mayor rendimiento |
| Power module | Voltaje y corriente compatibles, medicion calibrable en ArduSub, conectores seguros | Analogico/I2C/CAN segun autopilot |
| Proteccion | Fusible principal, proteccion contra inversion, cableado dimensionado | Obligatorio |

Regla: si se cambia el voltaje, hay que revalidar ESC, thrusters, LEDs, reguladores y camara.

No reutilizar la bateria del V6 para la conversion salvo informe de ensayo completo sin anomalias.

---

## 9. Camara y video

| Candidato | Especificaciones a exigir | Ventaja | Limitacion |
|---|---|---|---|
| Raspberry Pi Camera Module 3 / HQ | Compatible con Raspberry Pi, baja latencia configurable, buena integracion BlueOS | Integracion limpia | Requiere iluminacion y optica sumergible correcta |
| Camara USB low-light | UVC estandar, 1080p minimo, baja latencia | Facil de reemplazar | Calidad variable segun sensor |
| Camara IP/RTSP submarina | RTSP estable, latencia conocida | Buena para tether Ethernet | Puede agregar latencia y consumo |

No asumir reutilizacion de la camara 4K QYSEA para FPV integrado.

---

## 10. Luces

| Componente | Especificaciones a exigir | Nota |
|---|---|---|
| LED submarino | Voltaje/corriente conocidos, optica adecuada, cable y penetrador estancos | Puede reutilizar LED del V6 solo si se mide |
| Driver | Corriente constante o MOSFET PWM dimensionado, proteccion termica | No alimentar LED potente desde salida de autopilot |
| Control | Canal auxiliar PWM/relay | Verificar disipacion dentro del casco |

---

## 11. Tether y conectores

| Elemento | Especificaciones a exigir | Verificacion |
|---|---|---|
| Tether datos | Pares trenzados aptos para 100 Mbps en 100 m, baja atenuacion, chaqueta adecuada | Prueba de throughput real |
| Tether con potencia | Conductores dimensionados, caida de tension calculada, proteccion en superficie | Solo si se quiere alimentacion desde superficie |
| Conectores sumergibles | Rating de profundidad, pines suficientes, alivio de traccion | No usar conectores de audio para lazo critico |
| Penetradores | Compatibles con casco, O-rings y presion objetivo | Prueba de vacio/presion |

El tether original del V6 solo entra si pasa mapeo, continuidad, aislamiento y prueba de datos.

---

## 12. Casco, penetradores y estanqueidad

| Elemento | Especificaciones a exigir | Nota |
|---|---|---|
| Casco | Superficies de sello sin dano, espacio para nueva electronica, disipacion termica | Reutilizar solo tras inspeccion |
| Penetradores | Cantidad y diametro para tether, 6 thrusters, sensor de presion, luces y futuros | Mejor sobrar capacidad |
| O-rings | Material compatible, ranuras limpias, grasa de silicona | Nunca base petroleo |
| Prueba | Vacio/presion antes de agua | Obligatoria despues de cualquier apertura |

---

## 13. Posicionamiento opcional

| Sensor | Cuando agregarlo | Nota |
|---|---|---|
| GPS | Solo superficie | No sirve sumergido |
| DVL | Position hold y navegacion submarina | Costo alto; integracion avanzada |
| USBL/acustica | Posicion relativa desde superficie | Sistema adicional complejo |
| Odometria visual | Investigacion | Requiere camara, luz y computo |

Para el primer Nivel B, depth hold + heading hold es suficiente. Position hold submarino queda como fase posterior.

---

## 14. GCS y control

| Elemento | Especificaciones a exigir |
|---|---|
| Laptop/PC | Corre QGroundControl o Cockpit, puerto Ethernet/USB, bateria o UPS |
| Joystick | USB, ejes suficientes, botones para modos/failsafe |
| Red | Configuracion estatica/documentada para tether |
| Logs | Activar y guardar desde la primera prueba |

---

## 15. Tabla de reutilizacion del V6 en Nivel B

| Parte del V6 | Estado en Nivel B | Condicion |
|---|---|---|
| Casco/domo/mecanica | Candidata | Inspeccion y prueba de estanqueidad |
| Tether | Candidato condicionado | Mapeo + prueba de datos |
| LEDs | Candidatos condicionados | Medicion electrica/termica + driver |
| Thrusters Q-Motor | Condicionados | Ensayo completo + ESC nuevo |
| Sensor presion | Solo si estandar | Debe ser MS5837/Keller accesible |
| Placa principal | No | Reemplazar por autopilot |
| ESC propietarios | No inicialmente | Reemplazar |
| Camara 4K | No integrada | Reemplazar o payload separado |
| Bateria | No recomendado | Pack nuevo |
| Q-Interface | No | Reemplazar por interfaces documentadas |
| RC/app | No | QGC/Cockpit + joystick |

---

## 16. Criterios de aceptacion de compra

Antes de comprar, cada componente debe tener:

- hoja de datos o documentacion suficiente;
- soporte confirmado en ArduSub/BlueOS/QGC si aplica;
- voltaje y corriente compatibles con el resto del sistema;
- rating de profundidad si atraviesa el casco;
- repuestos o disponibilidad razonable;
- plan de prueba individual;
- costo y tiempo de entrega registrados.

Si un componente no tiene documentacion suficiente, pasa a la categoria "experimental" y no al lazo critico.

---

## 17. Datos que todavia faltan del V6

Para cerrar el BOM Nivel B respecto al V6 falta confirmar:

- voltaje real del sistema y de la bateria;
- corriente maxima por thruster;
- protocolo de control de los ESC;
- ubicacion de los ESC;
- tipo de sensor de presion;
- mapa del tether y conectores;
- interfaz electrica de LEDs;
- consumo total en reposo y en movimiento;
- geometria exacta de los 6 thrusters.

Hasta no tener esos datos, el BOM seguro es con thrusters/ESC nuevos o con donante medido.

---

## 18. Referencias internas

- `diagnostico/informe-conversion-completa-rov.md`
- `diagnostico/ardusub-adaptacion-fifish-v6.md`
- `diagnostico/reporte-reutilizacion-v6.md`
- `diagnostico/firmware-open-source-rov.md`
- `diagnostico/fifish-v6-expert.md`
