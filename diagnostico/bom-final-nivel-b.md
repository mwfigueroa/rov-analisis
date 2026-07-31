# BOM final Nivel B: ROV correlativo ArduSub / ArduPilot

> Fecha de evidencia: 2026-07-31.
> Moneda: USD, sin impuestos, envio, aduana ni mano de obra. Los precios son estimativos segun paginas de fabricante citadas; confirmar antes de comprar, especialmente desde Argentina.
> Decision de arquitectura: Nivel B predecible. No se reutilizan bateria, ESC, camara ni electronica propietaria del V6. La mecanica/casco del V6 solo entra si pasa inspeccion; este BOM cubre la electronica y partes consumibles principales.

---

## 1. Configuracion recomendada

- Autopilot/companion: Blue Robotics Navigator + Raspberry Pi 4.
- Propulsion: 6 x T200 con penetrador + 6 x Basic ESC.
- Profundidad: Bar30.
- Comunicacion: Fathom-X par + tether Fathom de 100 m.
- Potencia: bateria 4S 14.8 V 18 Ah + Power Sense Module + cargador.
- Video: Raspberry Pi Camera Module 3 Wide como camara FPV inicial.
- Software: ArduSub + BlueOS + QGroundControl/Cockpit.

Referencia de costo: Blue Robotics lista el BlueROV2 "From: $4,900". Un Nivel B con partes nuevas puede acercarse a ese orden si se compra todo nuevo; la economia real depende de que partes del V6 se reutilicen con seguridad.

---

## 2. BOM principal

| Item | Componente | Cant. | Costo unit. est. | Subtotal est. | Link / evidencia | Notas |
|---|---|---:|---:|---:|---|---|
| 1 | Navigator Flight Controller | 1 | $220 | $220 | https://bluerobotics.com/store/comm-control-power/control/navigator/ | Pagina: "From: $220". Para Raspberry Pi 4; incluye IMU, compas, barometro, 16 PWM, ADC y leak probes. |
| 2 | Raspberry Pi 4 Model B, 4 GB u 8 GB | 1 | $55-75 | $55-75 | https://www.raspberrypi.com/products/raspberry-pi-4-model-b/ | Pagina oficial: "From $35" y variantes 1/2/4/8 GB. Estimacion 4/8 GB; verificar reseller. Navigator declara compatibilidad con Raspberry Pi 4. |
| 3 | Raspberry Pi Camera Module 3 Wide | 1 | $25-35 | $25-35 | https://www.raspberrypi.com/products/camera-module-3/ | Pagina: "Available from $25". Elegir Wide 120 grados para FPV. Requiere optica/montaje sumergible. |
| 4 | Basic ESC bidireccional | 6 | $40 | $240 | https://bluerobotics.com/store/thrusters/speed-controllers/besc30-r3/ | 7-26 V, 30 A segun enfriamiento, PWM 1100/1500/1900 us, bidireccional. |
| 5 | T200 Thruster with Penetrator, BlueROV2 spare | 6 | $240 | $1,440 | https://bluerobotics.com/store/thrusters/t100-t200-thrusters/t200-thruster-r2-rp/ y spare https://bluerobotics.com/store/rov/bluerov2-components-spares/t200-thruster-brov2-spare-r1-vp/ | Spare con penetrador lista $240. Normal "From $230". Comprar mezcla CW/CCW segun frame. 7-20 V; 24 A a 16 V; 32 A a 20 V. |
| 6 | Bar30 depth/pressure sensor | 1 | $80-90 | $80-90 | https://bluerobotics.com/store/sensors-cameras/sensors/bar-depth-pressure-sensor/ | Bar30: 0-30 bar, hasta ~295.6 m en agua dulce, I2C. Secar elemento sensor segun fabricante. |
| 7 | Power Sense Module | 1 | $92 | $92 | https://bluerobotics.com/store/comm-control-power/control/psm-asm-r2-rp/ | 25.2 V max, 100 A no continuo, sensado voltaje/corriente; no entrega 5 V. |
| 8 | 5V 6A Power Supply | 1 | $32 | $32 | https://bluerobotics.com/store/comm-control-power/control/bec-5v6a-r1/ | Alimentacion dedicada para Navigator/Raspberry Pi/perifericos segun diseno. |
| 9 | Fathom-X Tether Interface Board, par | 1 | $280 | $280 | https://bluerobotics.com/store/comm-control-power/tether-interface/fathom-x/ | Par $280; single $140. Ethernet sobre un par, 80 Mbps practicos, 300 m probados. |
| 10 | Fathom ROV tether, 100 m | 100 m | $5-8/m | $500-800 | https://bluerobotics.com/store/cables-connectors/cables/fathom-rov-tether-by-the-meter/ | Enlazado desde Fathom-X: "1 or 4 UTP, neutrally buoyant". Elegir variante segun datos/potencia. |
| 11 | Lithium-ion Battery 14.8 V, 18 Ah | 1 | $425 | $425 | https://bluerobotics.com/store/comm-control-power/powersupplies-batteries/battery-li-4s-18ah-r3/ | Enlazada desde Basic ESC. 4S entra en rango T200/Basic ESC/PSM/Fathom-X. Verificar BMS, charger y normas de transporte. |
| 12 | H6 PRO Lithium Battery Charger | 1 | $196 | $196 | https://bluerobotics.com/store/comm-control-power/powersupplies-batteries/battery-charger-h6pro-r1/ | Enlazado desde Basic ESC. Confirmar compatibilidad con la bateria elegida. |
| 13 | Penetradores, O-rings, conectores, cableado, soportes | lote | $300-700 | $300-700 | Categorias: https://bluerobotics.com/product-category/cables-connectors/ y https://bluerobotics.com/product-category/watertight-enclosures/ | Estimacion por lote. Incluye WetLink/penetradores, grasa de silicona, tornilleria, terminales, fusibles y montajes. |
| 14 | Luces submarinas + driver | lote | $150-300 | $150-300 | Categoria: https://bluerobotics.com/product-category/thrusters/lights/ | Estimacion generica. No conectar LEDs potentes directo al autopilot; usar driver/MOSFET. |
| 15 | Joystick USB | 1 | $30-60 | $30-60 | Generico | Logitech F310/F710 o equivalente. Laptop/GCS se asume existente. |
| 16 | Consumibles y pruebas | lote | $150-400 | $150-400 | Generico | Grasa silicona, IPA, toallitas, termocontraible, conectores, pruebas de vacio/presion, repuestos. |

### Subtotal principal estimado

- Items 1-12, electronica/propulsion/tether/potencia: aprox. **$3,585-3,925**.
- Items 13-16, integracion/luces/joystick/consumibles: aprox. **$630-1,460**.
- Total Nivel B estimado: **$4,215-5,385**.

Comparacion: BlueROV2 nuevo aparece "From: $4,900" en paginas de Blue Robotics citadas. Por eso la conversion Nivel B solo se justifica si hay aprendizaje, donante, reutilizacion segura o requerimientos especiales.

---

## 3. Ahorros posibles por reutilizacion del V6

| Parte del V6 | Ahorro potencial | Condicion para contar el ahorro | Riesgo |
|---|---:|---|---|
| Thrusters Q-Motor | hasta $1,440 | Resistencias equilibradas, aislamiento OK, rodamientos OK, ESC nuevo compatible, banco de empuje aceptable | Falla tardia por corrosion; mezcla de curvas desconocidas |
| Tether original | $500-800 | Mapa de conductores + prueba Fathom-X/Ethernet a 100 m superada | Perdida de telemetria o imposibilidad de usar un par |
| Casco/domo/penetradores | parte de $300-700 | Superficies de sello sanas, espacio y prueba de estanqueidad | Fuga a profundidad |
| LEDs | parte de $150-300 | Voltaje/corriente medidos y driver propio | Sobrecalentamiento o corte |
| Bateria | $425 | No recomendado salvo ensayo completo | Riesgo termico; no contar ahorro por defecto |

Regla: el ahorro solo se reconoce despues de pasar el checklist de `reporte-reutilizacion-v6.md`.

---

## 4. Alternativas para bajar costo

| Alternativa | Cambio | Efecto | Riesgo |
|---|---|---|---|
| Usar T200 normal en vez de spare con penetrador | Item 5 baja de $240 a "From $230" | Ahorro pequeno por unidad | Hay que resolver penetrador igualmente |
| Comprar Fathom-X single + par ya existente | No aplica si no hay segundo | Solo valido si ya se tiene una unidad | No asumir |
| Tether generico validado | Sustituir item 10 | Puede bajar mucho | Debe pasar prueba de throughput y flotabilidad |
| Camara USB generica | Sustituir item 3 | Similar o menor | Latencia/calidad variable |
| Autopilot Pixhawk en vez de Navigator + Pi | Cambia items 1-2 | Puede bajar integracion inicial | Requiere companion separado para BlueOS/video |
| No comprar cargador dedicado inicialmente | Quita item 12 temporal | Ahorro $196 | Solo si ya hay cargador seguro compatible |

---

## 5. Validaciones antes de cerrar compra

- Confirmar que la geometria final sea Vectored 6 o definir frame custom.
- Confirmar cantidad CW/CCW de helices antes de comprar T200.
- Confirmar que la bateria 4S alimenta: ESC/T200, Fathom-X y regulacion de 5 V sin exceder ratings.
- Calcular consumo maximo: 6 x T200 a 20 V puede superar 190 A teorico; limitar por software y disenar cableado/fusible para la mision real, no para full throttle continuo irreal.
- Confirmar penetraciones: 6 thrusters, tether, Bar30, luces y futuro.
- Confirmar refrigeracion de ESC y Raspberry Pi dentro del casco.
- Confirmar que Bar30 queda correctamente instalado en bulkhead; la parte trasera no es estanca por si sola.
- Confirmar logistica a Argentina: bateria Li-ion puede tener restricciones de envio.

---

## 6. Software y versiones a congelar

| Software | Costo | Link | Nota |
|---|---:|---|---|
| ArduSub | $0 | https://ardupilot.org/sub/ | Elegir release estable y registrar version |
| BlueOS | $0 | https://blueos.cloud/ | Imagen/companion para Navigator/Raspberry Pi |
| QGroundControl | $0 | https://qgroundcontrol.com/ | GCS principal |
| Cockpit | $0 | https://github.com/bluerobotics/cockpit | GCS web alternativa |
| ArduPilot GitHub | $0 | https://github.com/ArduPilot/ardupilot | Fuente y issues |

---

## 7. Evidencia principal usada

- Navigator: precio "From $220", Raspberry Pi 4, 16 PWM, IMU/compas/barometro/ADC/leak probes.
- T200: spare con penetrador $240; 7-20 V; 24 A a 16 V; 32 A a 20 V.
- Basic ESC: $40; 7-26 V; 30 A; PWM bidireccional 1100-1900 us.
- Bar30: rango $80-90; 0-30 bar; hasta ~295.6 m en agua dulce; I2C.
- Power Sense Module: $92; 25.2 V max; 100 A no continuo.
- Fathom-X: $140 single / $280 par; 80 Mbps practicos; 300 m probados.
- Bateria/cargador/5V: enlazados como productos relacionados desde Basic ESC/PSM.
- Raspberry Pi 4: pagina oficial "From $35" con variantes de RAM.
- Camera Module 3: pagina oficial "Available from $25".

---

## 8. Conclusion

El BOM final Nivel B queda definido alrededor de **Navigator + Raspberry Pi 4 + BlueOS + 6 T200 + 6 Basic ESC + Bar30 + Fathom-X + bateria 4S**. El costo estimado total cae en el rango de varios miles de USD y se acerca al precio de entrada de un BlueROV2 nuevo si se compra todo. La decision racional sigue siendo: reparar el V6 si se quieren conservar funciones originales; convertir solo si el objetivo es aprender/construir una plataforma abierta y se acepta el costo/riesgo.
