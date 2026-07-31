# Informe practico: exploracion de conversion completa del ROV FIFISH V6 Expert a ArduSub / ArduPilot

> Objetivo: evaluar de forma practica si conviene y como se haria una conversion completa del QYSEA FIFISH V6 Expert a una arquitectura abierta basada en ArduSub/ArduPilot.
> Contexto: el ROV actual tiene una falla de hardware en un thruster frontal tras exposicion a agua salada. La conversion no debe confundirse con la reparacion. Si el equipo tiene garantia/RMA viable, la conversion completa no es la primera ruta recomendada.

---

## 1. Respuesta ejecutiva

La conversion completa es **tecnicamente posible**, pero en la practica equivale a **construir un ROV nuevo dentro o alrededor de la mecanica del FIFISH**. No es un cambio de firmware. Es un reemplazo de la pila de control, comunicaciones, potencia, video y probablemente actuadores.

Conviene solo si se cumple al menos una de estas condiciones:

- el equipo esta fuera de garantia y se acepta perder funciones propietarias;
- existe una unidad donante o casco para experimentar;
- el objetivo principal es aprender/investigar ArduSub, no recuperar el ROV rapido;
- se acepta un proyecto de semanas/meses con riesgo de no recuperar el valor del equipo.

No conviene si la prioridad es volver a operar el V6 Expert con camara 4K, app y accesorios originales. En ese caso, reparar por RMA es la ruta correcta.

---

## 2. Que significa "conversion completa"

Para este informe, conversion completa significa reemplazar el cerebro del ROV y sus interfaces criticas:

| Capa | Estado original | Estado convertido |
|---|---|---|
| Control de vuelo | Firmware QYSEA | ArduSub en autopilot soportado |
| Companion/video | Pipeline propietario | BlueOS + camara estandar |
| Estacion de control | App FIFISH + RC | QGroundControl/Cockpit + joystick |
| Comunicacion | Tether propietario | MAVLink sobre Ethernet/UDP o serial documentado |
| Potencia | BMS inteligente propietario | Bateria validada + power module + protecciones |
| Actuadores | ESC/thrusters QYSEA | ESC estandar; thrusters reutilizados solo si pasan ensayo |
| Sensores | Propietarios | Presion, fuga, temperatura y corriente conocidos |
| Accesorios | Q-Interface | PWM/relay/UART/CAN/Ethernet documentados |

La mecanica puede conservarse: casco, domo, alas, soportes, lastre y eventualmente tether. La electronica de control no se conserva salvo investigacion avanzada.

---

## 3. Tres niveles de conversion

| Nivel | Descripcion | Viabilidad | Comentario |
|---|---|---:|---|
| A. Control abierto con actuadores reutilizados | Reemplazar FC/companion/GCS; conservar thrusters si aceptan ESC estandar | Media | Depende de protocolo electrico de motores/ESC |
| B. Control y potencia abiertos | Reemplazar FC, ESC, power module, video y sensores; conservar casco/mecanica | Media-alta | Mas predecible que A |
| C. Referencia abierta completa | Conservar solo domo/casco/partes mecanicas; todo lo demas nuevo | Alta tecnicamente | Es casi un ROV nuevo; menor riesgo de protocolos propietarios |

Recomendacion practica: apuntar a **Nivel B** como objetivo realista. El Nivel A solo si se confirma que los thrusters Q-Motor son electricamente estandar. El Nivel C es el mas sano para aprender sin pelear con lo propietario.

---

## 4. Precondiciones go/no-go

No iniciar conversion completa sin:

- RMA/garantia cerrado o descartado por escrito.
- Bateria original retirada y descartada como fuente de energia de trabajo salvo ensayo completo.
- microSD retirada y datos personales respaldados/borrados.
- Fotos completas del exterior e interior si se abre.
- Presupuesto para reemplazar componentes si la reutilizacion falla.
- Lugar de prueba en agua dulce controlada.
- Plan de prueba de estanqueidad antes de cualquier inmersion.
- Aceptacion de que camara 4K, app, bateria inteligente y accesorios QYSEA probablemente se pierden.

Si falta alguno, el proyecto debe quedar en banco de pruebas paralelo, no en conversion del ROV principal.

---

## 5. Fase 0: exploracion no destructiva

Objetivo: decidir sin abrir si la conversion tiene sentido.

Trabajos:
- Reunir manuales, app, versiones, numero de serie, fotos externas y videos de funcionamiento.
- Registrar comportamiento del thruster fallado y de los sanos.
- Medir consumo visible en app si esta disponible: voltaje, corriente, alarmas.
- Confirmar largo de tether, tipo de conectores y accesorios instalados.
- Revisar si existe documentacion publica de teardown o fotos internas del mismo modelo.
- Definir que funciones originales son imprescindibles y cuales se pueden perder.

Salida: decision informada entre reparar, convertir nivel B/C o construir prototipo paralelo.

---

## 6. Fase 1: banco ArduSub independiente

Objetivo: dominar la pila abierta antes de tocar el V6.

Componentes minimos:
- autopilot soportado;
- Raspberry Pi con BlueOS;
- un ESC estandar;
- un thruster de prueba;
- sensor de presion Bar30/MS5837;
- power module;
- bateria de banco;
- QGroundControl o Cockpit;
- joystick.

Hitos:
- telemetria MAVLink funcionando;
- joystick controlando un motor con limites bajos;
- depth hold en balde/pileta;
- failsafe de bateria y telemetria probados;
- logs guardados y revisados.

Criterio de salida: poder operar un lazo completo ArduSub sin improvisacion.

---

## 7. Fase 2: evaluacion de donante o apertura controlada

Solo si se decide convertir.

Trabajos:
- Abrir en entorno limpio, seco y ESD.
- Fotografiar cada capa antes de desconectar.
- Etiquetar conectores, tornillos y arneses.
- Identificar MCU, IMU, compas, sensor de presion, reguladores y drivers.
- Mapear continuidad de tether, thrusters, LEDs y sensores.
- Medir resistencias fase-fase y aislamiento de los 6 thrusters.
- Determinar protocolo de ESC con osciloscopio/analizador logico si es posible.
- Separar componentes en cuatro bolsas: reutilizar, adaptar, reemplazar, descartar.

Regla: si un subsistema no se entiende electricamente, no va al lazo de control.

---

## 8. Fase 3: arquitectura convertida recomendada

### 8.1 Pila de control
- Autopilot con ArduSub.
- Companion Raspberry Pi con BlueOS.
- GCS QGroundControl o Cockpit.
- Joystick USB.
- Logs habilitados desde el primer encendido.

### 8.2 Propulsion
- Seis ESC estandar.
- Seis thrusters: reutilizados solo si pasan ensayo; si no, nuevos.
- Frame inicial: Vectored 6 o custom segun geometria real.
- Direccion de giro verificada sin helices y luego con helices en condicion segura.

### 8.3 Potencia
- Bateria nueva o validada, con BMS conocido.
- Power module calibrado.
- Fusible principal y proteccion contra inversion.
- Separacion entre potencia de motores y electronica sensible.
- Corte por fuga o failsafe segun diseno.

### 8.4 Sensores
- Presion de profundidad con rating correcto.
- Sensor de fuga.
- Temperatura interna si el hardware lo permite.
- Corriente/voltaje por power module.
- Opcional: DVL/USBL solo si se necesita position hold real.

### 8.5 Video y luces
- Camara compatible BlueOS/QGC.
- Luces con driver PWM/relay, no directas al autopilot.
- Verificar termica de LEDs antes de cerrar el casco.

---

## 9. Configuracion practica de software

Sin fijar valores finales hasta tener hardware:

- Usar una version estable de ArduSub y congelarla para el proyecto.
- Registrar version de ArduSub, BlueOS, QGC/Cockpit y parametros.
- Configurar frame y matriz de motores segun geometria real.
- Limitar salida maxima de motores en las primeras pruebas.
- Configurar battery monitor y failsafes antes de agua.
- Configurar profundidad maxima permitida muy por debajo del objetivo final.
- Probar perdida de telemetria en banco: que haga algo seguro y predecible.
- Probar fuga simulada antes de depender de ella.
- Guardar un archivo de parametros por cada hito superado.

No usar modos agresivos al inicio. La prioridad es estabilidad, no velocidad.

---

## 10. Plan de pruebas

| Etapa | Prueba | Criterio de aceptacion |
|---|---|---|
| Banco 1 | Alimentacion y rieles | Sin calentamiento anormal, consumo esperado |
| Banco 2 | Un motor | Direccion correcta, sin tirones, corriente controlada |
| Banco 3 | Seis motores | Mezcla correcta, sin canal cruzado |
| Banco 4 | Sensores | Profundidad, fuga, bateria y temperatura coherentes |
| Estanqueidad | Vacio/presion | Sin caida anormal antes de agua |
| Pileta 1 | Flotacion y trim | ROV neutro o levemente positivo, sin cabeceo incontrolable |
| Pileta 2 | Manual estabilizado | Control predecible en los 6 ejes segun frame |
| Pileta 3 | Depth hold | Mantiene profundidad sin oscilaciones grandes |
| Pileta 4 | Failsafes | Bateria baja, telemetria y fuga actuan como se configuro |
| Agua abierta | Sesion corta | Sin fugas, logs limpios, recuperacion simple |

Definicion de conversion completa exitosa: el ROV convertido supera todas las etapas hasta pileta 4 y una primera sesion corta en agua abierta, con logs y sin alarmas no explicadas.

---

## 11. BOM orientativo para Nivel B

| Bloque | Componente | Nota |
|---|---|---|
| Autopilot | Navigator o Pixhawk soportado | Elegir por disponibilidad y sensores |
| Companion | Raspberry Pi 4/5 + BlueOS | Video y MAVLink |
| ESC | 6 ESC PWM estandar | Dimensionar por corriente de thruster |
| Thrusters | Reutilizados si pasan ensayo o nuevos | No mezclar estados desconocidos |
| Profundidad | Bar30/MS5837 o Keller | Rating acorde a 100/200 m |
| Potencia | Power module + BMS + bateria | Bateria nueva recomendada |
| Fuga | Sensor de fuga | Failsafe critico |
| Camara | USB/CSI/IP compatible | Reemplaza camara propietaria |
| Luces | LED + driver | Control por canal auxiliar |
| Mecanica | Penetradores, conectores, O-rings, grasa silicona | No escatimar |
| GCS | Laptop + joystick | QGroundControl/Cockpit |

Estimacion bruta: si se reutilizan casco y thrusters, varios cientos a alrededor de mil o mas USD segun calidad de partes. Si se reemplazan thrusters y electronica completa, acercarse al costo de un ROV nuevo de gama media. El factor dominante suele ser mano de obra, penetraciones y pruebas, no el autopilot.

---

## 12. Tiempo estimado

| Escenario | Tiempo practico |
|---|---:|
| Banco ArduSub inicial | 1-3 semanas |
| Conversion Nivel B con donante ordenado | 6-12 semanas |
| Conversion con ingenieria inversa de ESC/camara | 3-6 meses o mas |
| Conversion confiable para operar en mar | agregar pruebas y re-certificacion de sellado |

La variable que mas incertidumbre agrega es el protocolo de actuadores/camara. La segunda es el sellado despues de modificar el casco.

---

## 13. Riesgos principales

| Riesgo | Efecto | Mitigacion |
|---|---|---|
| Perder el ROV como equipo operativo | Alto costo | Usar donante o aceptar conversion total |
| Protocolo propietario de ESC | No controlar thrusters | Reemplazar ESC desde el inicio |
| Camara incompatible | Perder FPV/grabacion | Usar camara estandar |
| Bateria insegura | Riesgo termico | No reutilizar bateria sospechosa |
| Fuga tras modificar casco | Dano total | Penetradores correctos + prueba de vacio/presion |
| Inestabilidad por mala mezcla | Control pobre | Frame correcto, limites, pruebas progresivas |
| Corrosion residual | Fallas tardias | Limpieza, secado, medicion de aislamiento |
| Proyecto infinito | No terminar | Definir MVP y criterios de salida |

---

## 14. Decision practica

| Opcion | Cuando elegirla | Resultado esperado |
|---|---|---|
| Reparar por RMA | Se quiere recuperar el V6 con funciones originales | Camino mas corto y menor riesgo |
| Prototipo paralelo ArduSub | Se quiere aprender sin arriesgar el V6 | Conocimiento util y reusable |
| Conversion Nivel B | Hay donante o se acepta perder lo propietario | ROV abierto funcional si se respetan pruebas |
| Conversion Nivel C | Se quiere maxima previsibilidad | Casi un ROV nuevo con mecanica aprovechada |
| Conversion Nivel A | Solo si los actuadores son claramente estandar | Ahorro parcial, pero alta incertidumbre |

Recomendacion concreta: **no convertir la unidad principal mientras exista valor de reparacion/RMA.** Si se desea explorar conversion completa, hacerlo como proyecto paralelo o sobre donante, con objetivo Nivel B y MVP medible.

---

## 15. MVP recomendado

El primer producto minimo viable no es "el FIFISH convertido". Es:

1. ArduSub controlando un thruster en banco.
2. BlueOS/QGC con telemetria y video basicos.
3. Depth hold en pileta.
4. Failsafes probados.
5. Seis thrusters en frame correcto.
6. Estanqueidad validada.
7. Sesion corta en agua dulce.

Recien despues de eso se decide si la mecanica del V6 entra al proyecto.

---

## 16. Checklist ejecutivo

- [ ] Confirmado que no hay RMA/garantia activa.
- [ ] Bateria original fuera de servicio.
- [ ] Datos/microSD gestionados.
- [ ] Banco ArduSub funcionando.
- [ ] BOM Nivel B aprobado.
- [ ] Casco/domo inspeccionados.
- [ ] Tether mapeado o reemplazado.
- [ ] ESC estandar seleccionados.
- [ ] Thrusters ensayados o reemplazados.
- [ ] Sensores de profundidad/fuga/potencia funcionando.
- [ ] Parametros y versiones registrados.
- [ ] Prueba de estanqueidad superada.
- [ ] Pruebas de pileta superadas.
- [ ] Plan de recuperacion y abortado definido.

---

## 17. Referencias internas

- `diagnostico/fifish-v6-expert.md`
- `diagnostico/firmware-open-source-rov.md`
- `diagnostico/ardusub-adaptacion-fifish-v6.md`
- `diagnostico/reporte-reutilizacion-v6.md`
- `docs/QYSEA_V6Expert_QuickStart_Manual_V2.2_EN_extracted-text.txt`
