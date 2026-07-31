# Reporte detallado: posible reutilizacion del QYSEA FIFISH V6 Expert en una plataforma ArduSub / ArduPilot

> Alcance: evaluar que partes del V6 Expert podrian reutilizarse en un ROV correlativo basado en ArduSub/ArduPilot, y que condiciones tecnicas, electricas, mecanicas y de software deberian cumplirse antes de reutilizarlas.
> Premisa de seguridad: no abrir ni modificar el V6 Expert hasta resolver garantia/RMA. La reutilizacion se evalua para una unidad donante, fuera de garantia, o despues de una reparacion autorizada.

---

## 1. Resumen ejecutivo

La reutilizacion del V6 Expert es **parcial y condicionada**. Lo mas valioso no suele ser la electronica propietaria, sino partes mecanicas, opticas, conectores, cableado y eventualmente actuadores si se verifica compatibilidad electrica.

| Categoria | Potencial de reutilizacion | Comentario corto |
|---|---:|---|
| Estructura, domo, alas, soportes | Alto | Reutilizables si pasan inspeccion mecanica y optica |
| Tether/cable y conectores | Medio-alto | Solo si se mapean conductores y se verifica integridad para 100 m |
| LEDs | Medio | Reutilizables con driver propio si se confirma voltaje/corriente |
| Thrusters Q-Motor | Medio | Solo si el motor es BLDC estandar y el ESC propietario se reemplaza |
| Sensor de presion | Medio-bajo | Reutilizable solo si es estandar tipo MS5837/Keller y accesible |
| Bateria | Bajo / riesgoso | No reutilizar tras agua salada salvo ensayo completo de celdas y BMS |
| Placa principal / firmware | Bajo | No asumir compatibilidad con ArduSub; requeriria ingenieria inversa |
| ESC/drivers | Bajo | Si el protocolo es propietario, conviene reemplazarlos |
| Camara 4K | Bajo | Probablemente integrada/propietaria; mejor como payload separado o reemplazo |
| Q-Interface/accesorios | Bajo | No es una interfaz ArduSub estandar |
| Mando RC/app FIFISH | Muy bajo | Reemplazar por QGroundControl/Cockpit + joystick |

Regla practica: **reutilizar mecanica y potencia solo tras medicion; reemplazar control, firmware, comunicaciones y cualquier cosa con protocolo desconocido.**

---

## 2. Limitaciones y riesgos antes de empezar

### 2.1 Garantia y postventa

QYSEA excluye postventa por modificacion, desarmado o apertura no autorizada. Por lo tanto:

- Si el V6 sigue en garantia o en proceso de RMA, no tocar.
- Si se abre, asumir que se pierde cobertura y que el sellado debe re-certificarse.
- Documentar todo antes de desconectar: fotos, videos, etiquetas, posicion de tornillos, orden de conectores.

### 2.2 Seguridad electrica

- Bateria Li-ion expuesta a sal: riesgo termico. No cargar, no perforar, no dejar conectada.
- ESCs y bancos de capacitores pueden retener carga. Descargar de forma controlada antes de manipular.
- Usar fuente de banco con limite de corriente para primeros encendidos.
- Trabajar con ESD, superficie limpia y control de sal/humedad.

### 2.3 Seguridad de estanqueidad

- Todo componente reutilizado que atraviese el casco debe tratarse como punto de fuga.
- O-rings: inspeccion a contraluz, limpieza de ranura, grasa de silicona compatible.
- Nunca usar grasa base petroleo.
- Tras cualquier reensamblado: prueba de vacio/presion antes de agua, y prueba en agua dulce antes de salada.

---

## 3. Metodo de evaluacion recomendado

Antes de decidir reutilizar algo, completar esta secuencia:

1. Inventario visual: fotos, part numbers, conectores, marcas de corrosion.
2. Mapa de continuidad: que pin va a que funcion.
3. Medicion electrica pasiva: resistencias, aislamiento, continuidad, diodos.
4. Identificacion de protocolos: PWM, DShot, UART, CAN, I2C, SPI, Ethernet, propietario.
5. Encendido controlado con fuente limitada: consumo, rieles, calentamiento.
6. Prueba funcional aislada: un solo subsistema a la vez.
7. Decision: reutilizar, adaptar, reemplazar o descartar.

Herramientas sugeridas:
- multimetro;
- fuente de banco con limitacion;
- osciloscopio;
- analizador logico;
- megometro o medicion de aislamiento si esta disponible;
- camara termica o termometro IR;
- balanza y banco de empuje para thrusters;
- laptop con QGroundControl/Cockpit, BlueOS y herramientas seriales.

---

## 4. Evaluacion por subsistema

## 4.1 Placa principal / controlador de vuelo

### Hardware a identificar
- MCU: STM32 u otra familia; part number exacto.
- IMU: acelerometro/giroscopo.
- Compas/magnetometro.
- Sensor de presion interno o externo.
- UARTs, I2C, SPI, PWM, CAN, ADC.
- Pines de debug: SWD/JTAG, UART de consola, boot0/reset.
- Memoria flash externa si existe.

### Software a considerar
- ArduPilot solo corre si el MCU y la placa tienen soporte o se crea un `hwdef` propio.
- Crear `hwdef` implica mapear cada pin, reloj, bus y periferico.
- Si el firmware tiene proteccion de lectura, no intentar saltar protecciones; trabajar desde documentacion o hardware nuevo.
- Aun si el MCU fuera compatible, puede no existir bootloader util o puede requerirse soldadura fina.

### Veredicto
No reutilizar como objetivo inicial. Si se quiere investigar, hacerlo sobre placa donante y sin expectativa de conservar funciones propietarias.

Riesgo principal: brick irreversible y perdida total del equipo.

---

## 4.2 ESC / drivers de thrusters

### Hardware a identificar
- Si los ESC estan en la placa principal, en una placa de potencia separada o dentro de cada thruster.
- Voltaje de entrada nominal y maximo.
- Corriente por canal y disipacion.
- Protocolo de control: PWM 1100-1900 us, DShot, OneShot, UART, CAN o propietario.
- Si hay sensado de corriente por canal.
- Si hay telemetria de ESC.

### Pruebas
- Con analizador logico/osciloscopio, observar la senal hacia un thruster sano.
- Medir resistencia fase-fase del motor y comparar contra otro thruster.
- Medir aislamiento fase-carcasa.
- Confirmar si el canal fallado del V6 es motor, cable, conector o ESC.

### Software
- ArduSub espera actuadores controlables por salidas estandar o por drivers soportados.
- Si el protocolo QYSEA es propietario, la integracion puede costar mas que reemplazar los ESC.
- Si se reemplazan ESC, hay que recablear y verificar penetraciones de potencia.

### Veredicto
Reutilizar solo si el protocolo es estandar y la electronica esta sana. En caso contrario, reemplazar ESC/drivers y conservar, como mucho, los motores si pasan ensayo.

---

## 4.3 Thrusters Q-Motor

### Hardware a evaluar
- Tipo de motor: BLDC sensorless probable.
- KV efectivo, corriente maxima, potencia.
- Estado de rodamientos y eje.
- Sellado del pod y corrosion por sal.
- Helice, tuerca, eje y desgaste.
- Resistencia fase-fase equilibrada.
- Aislamiento a carcasa.

### Pruebas minimas
| Prueba | Criterio de aceptacion |
|---|---|
| Giro a mano | Libre, sin ruido ni puntos duros anormales |
| Fase-fase | Tres combinaciones equilibradas y cercanas a un thruster sano |
| Aislamiento | Alto, sin continuidad anormal a carcasa |
| Banco de empuje | Empuje y corriente coherentes entre unidades del mismo tipo |
| Temperatura | Sin punto caliente anormal en banco corto con refrigeracion adecuada |

### Software
- En ArduSub hay que configurar frame, direccion de giro, limites y mezcla.
- Si se conservan motores con ESC nuevos, hay que medir curva empuje/consumo para estimar autonomia.

### Veredicto
Potencialmente reutilizables, pero no asumir. Un thruster expuesto a sal puede girar libre y aun asi tener corrosion interna o aislamiento degradado.

---

## 4.4 Sensor de presion / profundidad

### Hardware
- Buscar si el sensor es MS5837, Keller u otro.
- Verificar rating: 100 m o 200 m.
- Verificar si esta dentro del casco, en un penetrador o en modulo externo.

### Software
- ArduSub soporta sensores de presion de profundidad conocidos.
- Si el sensor QYSEA no tiene driver o no es accesible por bus estandar, reemplazar.

### Veredicto
Reutilizable solo si es estandar y esta electricamente accesible. Para un prototipo, usar Bar30/MS5837 nuevo simplifica el desarrollo.

---

## 4.5 Bateria y BMS

### Riesgo principal
Es el componente mas sensible. Hubo exposicion a agua salada y calentamiento. Una celda danada puede entrar en fuga termica despues, no necesariamente de inmediato.

### Datos conocidos
- Manual: bateria de 156 Wh, celdas Panasonic 21700 Li-ion.
- Cargador del ROV: salida 12.6 V, 10 A. Esto sugiere un sistema tipo 3S Li-ion, pero debe confirmarse.

### Pruebas si se insiste en evaluar
- Inspeccion de corrosion, hinchamiento, olor, decoloracion y humedad.
- Voltaje por celda, no solo pack completo.
- Resistencia interna por celda.
- Capacidad real con descarga controlada.
- Autodescarga durante dias.
- Comportamiento termico bajo carga moderada.
- Estado del BMS: balanceo, cortes, termistores, comunicacion.

### Criterio recomendado
Si hay cualquier signo de ingreso salino, corrosion, hinchamiento, caida anormal de voltaje o diferencia entre celdas, no reutilizar. Reciclar o enviar a servicio.

### Veredicto
No reutilizar para vuelo/inmersion salvo ensayo completo y criterio conservador. Para ArduSub, usar pack nuevo con BMS conocido y power module calibrado.

---

## 4.6 Distribucion de potencia

### Hardware
- Confirmar rieles: bateria directa, 12 V, 9-12 V de Q-Interface, 5 V, 3.3 V.
- Medir consumo de LEDs, camara, electronica y thrusters.
- Evaluar fusibles, proteccion contra inversion, inrush y caidas de tension.
- Separar potencia de motores y electronica sensible si hace falta.

### Software
- Calibrar battery monitor en ArduSub: voltaje y corriente.
- Configurar failsafes por bajo voltaje y capacidad restante.
- Registrar consumo para comparar contra la autonomia original.

### Veredicto
La distribucion propietaria probablemente no se reutilice completa. Si se reutilizan cargas como LEDs o accesorios, conviene alimentarlas desde una etapa propia medida y protegida.

---

## 4.7 Tether, carrete y conectores

### Potencial
Es uno de los elementos mas interesantes para reutilizar, pero tambien uno de los mas faciles de subestimar.

### Hardware a verificar
- Cantidad de conductores y pares trenzados reales.
- Si hay alimentacion por tether o solo datos.
- Impedancia, capacitancia y atenuacion en 100 m.
- Continuidad punto a punto y aislamiento entre conductores.
- Estado del conector ROV, conector de superficie y carrete.
- Compatibilidad real con Ethernet de 100 Mbps si se pretende usar BlueOS/QGC.

### Consideracion importante
El manual muestra conectores y puertos propietarios. No asumir que el tether es Ethernet estandar solo porque el sistema tiene funciones de red. Hay que mapear y probar.

### Veredicto
Reutilizar si el mapeo confirma pares aptos para datos y si los conectores pueden adaptarse sin comprometer estanqueidad. Si hay dudas, usar tether nuevo conocido.

---

## 4.8 Camara 4K

### Hardware
- Determinar si la camara es un modulo independiente con salida estandar o si esta integrada al procesador principal.
- Buscar interfaces posibles: MIPI CSI, USB, HDMI, compuesto, Ethernet/RTSP.
- Verificar alimentacion y consumo.

### Software
- BlueOS/QGC funcionan mejor con camaras USB, CSI de Raspberry Pi o IP/RTSP conocidas.
- Si la camara QYSEA no expone una interfaz estandar, puede quedar como payload grabador independiente, sin integracion MAVLink.

### Veredicto
No contar con reutilizarla para FPV integrado. Si tiene valor optico, evaluarla como modulo separado con su propia alimentacion y grabacion.

---

## 4.9 LEDs

### Hardware
- Medir voltaje de trabajo, corriente y potencia.
- Confirmar si el dimming original es PWM, analogico o por bus.
- Verificar disipacion termica y estado de lentes.

### Software
- ArduSub puede controlar luces por canales auxiliares o MAVLink segun configuracion.
- Se necesita driver externo si la corriente supera lo que puede conmutar una salida de autopilot.

### Veredicto
Buen candidato a reutilizacion si pasan medicion electrica y termica. No conectar directo a una salida de autopilot sin driver.

---

## 4.10 Q-Interface y accesorios

### Datos conocidos
- Q-Interface: acero inoxidable 316, 9.0-12.0 V, 5 A max, red 100 Mbps.

### Problema
Aunque electricamente pueda alimentar cargas, el protocolo de accesorios probablemente sea propietario. ArduSub no va a reconocer un brazo, sonar o sampler QYSEA sin documentacion o ingenieria inversa.

### Veredicto
Usar la idea electrica como referencia, pero reemplazar por interfaces estandar: servo PWM, relay, RS485, UART, CAN o Ethernet documentada.

---

## 4.11 Sensores auxiliares: fuga, temperatura, humedad

### Hardware
- Verificar si son contactos simples, analogicos o bus digital.
- Medir salida en seco y en humedad controlada, sin exponer el ROV completo.

### Software
- ArduSub admite failsafes y entradas para leak/temp segun hardware y configuracion.
- Definir accion de failsafe: superficie, cortar motores, alerta, o solo log segun severidad.

### Veredicto
Reutilizables si son electricamente simples. Si son parte de un bus propietario, reemplazar por sensores conocidos.

---

## 4.12 Mecanica, casco, domo y flotabilidad

### Reutilizables probables
- Domo, si no tiene rayas, niebla permanente o corrosion en el aro.
- Alas traseras, manijas, soportes, tornilleria si no esta corroida.
- Casco, solo si se inspeccionan superficies de sello y penetradores.

### Cuidados
- Cambiar electronica cambia masa y centro de gravedad/flotabilidad.
- Hay que rehacer trim de flotabilidad y lastre.
- Un casco reutilizado exige prueba de estanqueidad igual o mas estricta que uno nuevo.

### Veredicto
Muy reutilizable como mecanica, pero no como sistema certificado sin prueba. Para un primer prototipo ArduSub, un frame/casco nuevo reduce variables.

---

## 5. Matriz de decision por componente

| Componente | Reutilizar | Condicion necesaria | Si no se cumple |
|---|---|---|---|
| Domo/mecanica | Si | Sin danos opticos ni corrosion en sello | Reemplazar pieza mecanica |
| Tether | Tal vez | Mapa de conductores + prueba de datos 100 m | Tether nuevo |
| LEDs | Tal vez | Voltaje/corriente/driver confirmados | LEDs nuevos |
| Thrusters | Tal vez | Resistencias equilibradas, aislamiento OK, rodamientos OK | Thrusters/ESC nuevos |
| Sensor presion | Tal vez | MS5837/Keller estandar accesible | Bar30/Keller nuevo |
| Bateria | No recomendado | Ensayo completo de celdas/BMS sin anomalies | Pack nuevo |
| Placa principal | No inicialmente | MCU soportado + hwdef + debug | Autopilot nuevo |
| ESC | No inicialmente | Protocolo estandar y canales sanos | ESC nuevos |
| Camara | No integrada | Salida estandar documentada | Camara BlueOS/QGC |
| Q-Interface | No | Protocolo documentado | Servo/relay/MAVLink |
| RC/app | No | No aplica | QGC/Cockpit + joystick |

---

## 6. Software y firmware: puntos criticos

1. ArduSub debe correr en hardware soportado o en una placa con `hwdef` validado.
2. La comunicacion recomendada es MAVLink sobre Ethernet/UDP con BlueOS como companion.
3. QGroundControl o Cockpit reemplazan la app FIFISH.
4. El video debe resolverse con camara compatible, no con el pipeline propietario QYSEA.
5. El frame de motores debe validarse contra la geometria real del V6 antes de usar helices.
6. Las direcciones de giro se prueban sin helices primero y con corriente limitada despues.
7. El battery monitor debe calibrarse con carga real y fuente de referencia.
8. Los failsafes deben probarse de forma controlada: perdida de telemetria, bateria baja, fuga simulada, temperatura.
9. Todo cambio de parametros debe registrarse en el repositorio.
10. Si se distribuye una adaptacion derivada de ArduPilot, revisar obligaciones de licencia GPLv3.

---

## 7. Integracion recomendada si se reutilizan partes

### Arquitectura de datos
- Autopilot ArduSub como nucleo.
- Companion BlueOS para video y routing MAVLink.
- Sensores de profundidad/fuga por buses conocidos.
- Accesorios por PWM/relay/UART/CAN documentados.
- Nada propietario en el lazo critico de control.

### Arquitectura de potencia
- Bateria nueva o validada.
- Power module calibrado.
- Etapa de motores separada de electronica sensible.
- LEDs y accesorios con fusible/driver propio.
- Proteccion contra inversion y corte por fuga.

### Arquitectura mecanica
- Rehacer flotabilidad y trim.
- Mantener centro de empuje y centro de gravedad controlados.
- Evitar masas altas que generen roll/pitch no deseados.
- Verificar que el tether no introduzca par de giro en el conector.

---

## 8. Registro de riesgos

| Riesgo | Probabilidad | Impacto | Mitigacion |
|---|---:|---:|---|
| Perder garantia por apertura | Alta | Alto | No abrir hasta resolver RMA |
| Brick de placa original | Media | Alto | No reflashear placa principal |
| Fuga termica de bateria | Media | Muy alto | No reutilizar bateria sospechosa |
| Falsa compatibilidad de ESC | Alta | Alto | Medir protocolo antes de conectar |
| Fuga por penetrador/O-ring | Media | Muy alto | Re-certificar sellado |
| Corrosion oculta | Alta | Alto | Inspeccion, limpieza, medicion de aislamiento |
| Inestabilidad por cambio de masa | Media | Medio | Rebalanceo y pruebas en agua dulce |
| Perdida de camara/app | Alta | Medio | Asumir reemplazo desde el inicio |
| Datos personales en microSD | Baja | Medio | Retirar y respaldar/borrar tarjeta antes de manipular |

---

## 9. Checklist de reutilizacion

### Antes de abrir
- [ ] RMA/garantia cerrado o abandonado explicitamente.
- [ ] Bateria retirada y aislada.
- [ ] microSD retirada y datos gestionados.
- [ ] Fotos externas completas.
- [ ] Zona limpia, seca y ESD preparada.

### Al abrir, si corresponde
- [ ] Fotografiar cada capa antes de desconectar.
- [ ] Etiquetar conectores y tornillos.
- [ ] Medir y registrar rieles antes de desenergizar definitivamente.
- [ ] Identificar part numbers de MCU, IMU, compas, presion, ESC, reguladores.
- [ ] Mapear continuidad de tether, thrusters, LEDs y sensores.

### Antes de reutilizar cualquier parte
- [ ] Sin corrosion activa.
- [ ] Aislamiento electrico correcto.
- [ ] Protocolo conocido o adaptador definido.
- [ ] Consumo y temperatura dentro de rango.
- [ ] Integracion mecanica no compromete sello.
- [ ] Prueba funcional aislada superada.
- [ ] Prueba de estanqueidad superada.

---

## 10. Recomendacion final

Para el proyecto ROV Analisis, la reutilizacion del V6 deberia manejarse como **recuperacion selectiva de componentes**, no como conversion del ROV completo.

Prioridad de reutilizacion:
1. Mecanica y domo, tras inspeccion.
2. Tether, solo si el mapeo confirma capacidad de datos.
3. LEDs, con driver propio.
4. Thrusters, solo con ensayo electrico/mecanico completo.
5. Sensor de presion, solo si es estandar.

No priorizar:
- placa principal;
- firmware;
- ESC propietarios;
- camara integrada;
- bateria;
- Q-Interface;
- RC/app.

La via mas segura sigue siendo: reparar el V6 por RMA y construir un prototipo ArduSub paralelo. Si luego se decide reutilizar partes, hacerlo con criterio de componente aislado, prueba y trazabilidad.

---

## 11. Referencias internas

- `diagnostico/fifish-v6-expert.md`
- `diagnostico/firmware-open-source-rov.md`
- `diagnostico/ardusub-adaptacion-fifish-v6.md`
- `docs/QYSEA_V6Expert_QuickStart_Manual_V2.2_EN_extracted-text.txt`
- `docs/QYSEA_V6Expert_Motors_Battery_Maintenance_Guide.pdf`
