# Comparación de motores: QYSEA Q-Motor del FIFISH V6 Expert vs Blue Robotics T200

> Fecha: 2026-08-03.
> Alcance: comparar el motor/propulsor del caso FIFISH V6 Expert contra el thruster de referencia usado en el BOM ArduSub Nivel B.
> Advertencia central: la comparación **no es apples-to-apples**. El Q-Motor no tiene datasheet público a nivel motor; el T200 sí. No usar valores del T200 para diagnosticar el Q-Motor.

---

## 1. Identidad de cada motor

| Punto | QYSEA Q-Motor / Smart Thruster Array | Blue Robotics T200 |
|---|---|---|
| Rol en este proyecto | Motor original del ROV FIFISH V6 Expert en diagnóstico | Candidato de propulsión para ROV correlativo ArduSub Nivel B |
| Documentación pública | Muy limitada; sistema/marketing, no datasheet de motor | Completa: specs, performance charts, guía de uso |
| Ecosistema | Cerrado QYSEA / FIFISH App | Abierto ArduSub / BlueOS / QGC / Cockpit |
| Recomendación de uso | Mantener para reparación/RMA del V6 | Usar para prototipo/conversión documentada |

---

## 2. Información disponible del Q-Motor

Fuentes revisadas:
- Quick Start V2.2 local.
- QYSEA product page V6 Expert.
- QYSEA support/guides V6 Expert.
- PDF oficial **FIFISH V6 EXPERT – Technical Specifications**: https://drive.google.com/file/d/1riSwzhdGuCJ65pwEIcieNLVhPdvefdmO/view?usp=sharing

Datos conocidos a nivel ROV/sistema:

| Parámetro | Valor público |
|---|---|
| Propulsión | 6 thrusters, Smart Thruster Array |
| Manual | `Thrusters Q-Motor Tech × 6`, `4 × Vector + 2 × Horizontal` |
| Movimiento | 6 DOF: izquierda/derecha, arriba/abajo, adelante/atrás, yaw/pitch/roll 360° |
| Velocidad | 3 knots / 1.5 m/s |
| Profundidad | 100 m en specs técnicas; versiones M100/M200 por tether |
| Peso ROV | 4.6 kg |
| Payload | 5 kg |
| Batería | 156 Wh; quick charge 90% en 1 h |
| Cargador ROV | salida 12.6 V, 10 A |
| Q-Interface | 9–12 V, 5 A, Ethernet/UART |
| Marketing Q-MOTOR | Protection Against Corrosion & Sediment |
| Mantenimiento oficial | remojo/agua dulce después de cada inmersión + Maintain/Thrusters |
| Seguridad oficial | no correr thrusters en aire más de 30 s |

Datos **no públicos** del Q-Motor:

- KV / velocidad específica.
- Empuje por motor en kgf.
- Corriente nominal/pico por motor.
- Rango de tensión del motor solo.
- Resistencia/inductancia de fases.
- Cantidad de polos.
- Tipo de rodamiento/sello.
- Rating de profundidad del thruster individual.
- Protocolo de ESC: PWM estándar, DShot o propietario.
- Part number de thruster/ESC.

Consecuencia: para el diagnóstico del V6 no existe un valor de fábrica público de fase-fase o corriente. La referencia válida es un thruster sano del mismo ROV.

---

## 3. Información disponible del T200

Fuente: Blue Robotics T200 — https://bluerobotics.com/store/thrusters/t100-t200-thrusters/t200-thruster-r2-rp/

| Parámetro | Valor |
|---|---|
| Voltaje operativo | 7–20 V |
| Recomendado | 12–16 V |
| Empuje FWD/REV @16 V | 5.25 / 4.1 kgf |
| Empuje FWD/REV @20 V | 6.7 / 5.05 kgf |
| Empuje mínimo | 0.02 kgf, limitado por ESC |
| Corriente full throttle @16 V | 24 A |
| Corriente full throttle @20 V | 32 A |
| Potencia @16 V | 390 W |
| Potencia @20 V | 645 W |
| Hélice | 76 mm |
| Cable | 16 AWG |
| Penetrador | M10 según variante |
| Materiales mojados | policarbonato glass-filled, epoxi, acero inoxidable, plástico, poliuretano, aluminio 7075-T6, FKM, Buna-N |
| Uso en seco | no más de ~10 s; rodamientos requieren agua |
| Datos de performance | charts públicos 10–20 V |

ESC asociado Basic ESC — https://bluerobotics.com/store/thrusters/speed-controllers/besc30-r3/

| Parámetro | Valor |
|---|---|
| Voltaje | 7–26 V / 2–6S |
| Corriente constante | 30 A según enfriamiento |
| Señal | PWM 1100–1900 µs, stop/init 1500 µs |
| Deadband | 1500 µs ± 25 µs |
| Firmware | BLHeli_S |
| Dirección | bidireccional |
| Protección térmica | reduce potencia desde ~140 °C |

---

## 4. Comparación directa

| Dimensión | QYSEA Q-Motor | Blue Robotics T200 | Lectura práctica |
|---|---|---|---|
| Datasheet público | No a nivel motor | Sí, completo | T200 es diseñable; Q-Motor se diagnostica por comparación |
| Voltaje | No público por motor; sistema sugiere 3S por cargador 12.6 V, verificar | 7–20 V, recomendado 12–16 V | No asumir equivalencia eléctrica |
| Corriente | No pública | 24 A @16 V; 32 A @20 V | No usar como límite del Q-Motor |
| Empuje | No público | 5.25/4.1 kgf @16 V; 6.7/5.05 kgf @20 V | No estimar empuje del V6 desde T200 |
| Control | FIFISH App/ESC QYSEA, protocolo no publicado | PWM 1100–1900 µs con Basic ESC/BLHeli_S | Para ArduSub, T200 es integrable; Q-Motor exige reverse engineering |
| Mantenimiento | Agua dulce después de cada dive + Maintain/Thrusters | Guía de uso T200; mantenimiento y limpieza de partículas | Ambos sufren con sal/sedimento; prevención es clave |
| Corrosión/sedimento | Marketing: protección contra corrosión y sedimento | Materiales y guía pública; requiere mantenimiento | La sal sigue siendo riesgo real en ambos |
| Uso en seco | QYSEA: no más de 30 s en aire | Blue Robotics: no más de ~10 s en seco | Regla práctica: evitar correr en seco salvo segundos |
| Diagnóstico | Comparar contra thruster sano del mismo ROV | Comparar contra datasheet y banco conocido | Métodos distintos por disponibilidad de datos |
| Reparación | Mejor por centro autorizado/RMA | Reparación/integración más abierta | V6: RMA; prototipo: T200 |
| Riesgo de conversión | Alto si se intenta reutilizar sin protocolo | Bajo/medio con ArduSub | No migrar Q-Motor a ArduSub sin caracterización |

---

## 5. Qué se puede y qué no se puede inferir

### Sí se puede inferir del Q-Motor
- Es un arreglo de 6 thrusters para 6 DOF.
- La falla de un solo thruster puede bloquear el sistema por protección.
- La sal/cristalización es un mecanismo plausible de atascamiento.
- La guía oficial confirma mantenimiento con agua dulce y limpieza de motores.
- El cargador 12.6 V sugiere arquitectura tipo 3S, pero debe verificarse antes de diseñar alrededor.

### No se puede inferir del Q-Motor usando T200
- Resistencia fase-fase esperada.
- Corriente de rotor bloqueado.
- Empuje real.
- Voltaje máximo seguro.
- Compatibilidad con Basic ESC o ArduSub.

---

## 6. Plan de caracterización no destructiva del Q-Motor

Objetivo: crear una mini-datasheet local del thruster sano y del sospechoso sin abrir el casco.

1. **Identificación**
   - Marcar sospechoso y referencia sana del mismo tipo.
   - Registrar posición: frontal izquierdo/derecho, vertical/horizontal si aplica.

2. **Giro a mano**
   - Libre/áspero/trabado.
   - Presencia de línea, pelo, arena o sal.

3. **Resistencia fase-fase**
   - U-V, V-W, U-W.
   - Criterio: sospechoso debe estar cerca del sano; una combinación abierta o mucho más alta indica daño.

4. **Aislamiento**
   - Fase a carcasa/tierra.
   - Criterio: alto/abierto; baja resistencia indica bobinado comprometido.

5. **Inspección**
   - Sal blanca, corrosión verde/azul, humedad en conector/penetrador.

6. **Telemetría**
   - Código de falla, thruster marcado, voltaje, alarmas de fuga/temperatura.

7. **Solo si pasa todo**
   - Prueba breve en agua dulce, no en aire, con límites y monitoreo de temperatura/consumo.
   - Si tiembla, calienta o re-bloquea, detener y derivar.

---

## 7. Decisión práctica

| Caso | Decisión recomendada |
|---|---|
| Diagnóstico/reparación del FIFISH V6 | No usar T200 como referencia numérica. Medir contra Q-Motor sano y derivar a RMA si hay anomalía. |
| Conversión ArduSub Nivel B | Usar T200 + Basic ESC por documentación completa y soporte ArduSub. |
| Reutilización de Q-Motor en prototipo | Solo si pasa caracterización completa y se resuelve ESC/control; no es ruta inicial. |
| Motor atascado por sal | Limpiar/remojar, verificar giro libre, medir, y solo después reset/prueba. Si re-bloquea, servicio. |

---

## 8. Conclusión

El Q-Motor del FIFISH V6 Expert y el Blue Robotics T200 no son comparables por datasheet porque QYSEA no publica las características eléctricas/mecánicas del motor. La comparación útil es de método:

- **Q-Motor:** diagnosticar por evidencia local, comparación contra thruster sano y RMA.
- **T200:** diseñar por datasheet, integración ArduSub y pruebas de banco.

Para el caso actual del V6, la información pública alcanza para sostener la hipótesis de falla localizada por sal/atascamiento/fase/ESC, pero no para definir valores eléctricos de fábrica. Para ArduSub Nivel B, el T200 queda como referencia documentada de propulsión.

---

## 9. AQUROV / T200 genérico como reemplazo del Q-Motor

> Punto de partida: si el propulsor AQUROV se presenta como “T200 ROV”, hay que tratarlo como **T200-compatible genérico hasta demostrar lo contrario**. No asumir equivalencia con Blue Robotics T200 ni con QYSEA Q-Motor sin datasheet y ensayo.

### 9.1 Diferencias que pueden importar

| Dimensión | Por qué puede diferir | Qué verificar antes de usarlo |
|---|---|---|
| Datasheet | Un genérico puede copiar forma, no performance | Empuje, corriente, voltaje, KV, curva 10–20 V |
| Calidad de bobinado | Cambia resistencia, temperatura y eficiencia | Fase-fase equilibrada y comparación contra referencia |
| Rodamientos/eje | Punto típico de falla por sal/sedimento | Giro a mano, juego axial/radial, ruido, material |
| Sellos/estanqueidad | No todo “T200 style” soporta la misma profundidad | Rating de profundidad, O-rings, potting, penetrador |
| Hélice/nozzle | Cambia empuje, carga y vibración | Diámetro, paso, CW/CCW, balanceo, nozzle |
| Cable/penetrador | Suele ser el problema real, no solo el motor | AWG, largo, aislación, penetrador M10 u otro, alivio de tracción |
| ESC requerido | Todo BLDC necesita ESC compatible | PWM estándar 1100–1900 µs, bidireccional, corriente suficiente |
| Protocolo QYSEA | El V6 puede no usar PWM estándar | Confirmar si Q-Motor es PWM/DShot/propietario antes de mezclar |
| Peso/flotación | Cambia trim y actitud | Peso en aire/agua, empuje vertical, lastre |
| Control de calidad | Un genérico puede variar entre unidades | Comprar repuesto, probar una unidad antes de comprometer seis |
| Garantía/postventa | Reemplazar Q-Motor implica abrir/modificar | Si hay RMA viable, no reemplazar con genérico |

### 9.2 Si se quiere usar como reemplazo del Q-Motor en el FIFISH V6

No es la ruta recomendada si todavía existe opción de RMA/servicio. Reemplazar un Q-Motor por un AQUROV/T200 genérico implica:

- abrir o modificar el ROV;
- confirmar protocolo eléctrico del canal QYSEA;
- adaptar montaje, hélice, cable y penetrador;
- recalibrar mezcla/trim;
- validar que el sistema no bloquee por diferencia de corriente o respuesta;
- re-certificar estanqueidad si se toca el casco.

Riesgo principal: **mismatch de empuje**. Aunque el motor gire, una respuesta distinta puede hacer que el ROV quede inestable o vuelva a bloquear motores por protección.

### 9.3 Si se quiere usar en prototipo ArduSub

Es más razonable, pero con validación:

1. Medir fase-fase y aislamiento.
2. Confirmar que el ESC estándar lo arranca en ambos sentidos.
3. Banco en agua: empuje/corriente/temperatura a 12 V y 16 V si el ESC lo permite.
4. Comparar contra T200 oficial o contra una unidad sana conocida.
5. Probar una unidad antes de comprar/usar seis.
6. Usar hélices CW/CCW correctas y configurar dirección en ArduSub.

### 9.4 Veredicto

| Uso propuesto | Veredicto |
|---|---|
| Reemplazo directo de Q-Motor en V6 con garantía/RMA vigente | No recomendado |
| Reemplazo de Q-Motor en V6 fuera de garantía | Solo con ingeniería inversa + caracterización + re-certificación |
| Banco/pileta ArduSub | Aceptable si pasa ensayo eléctrico/térmico |
| Propulsión ArduSub Nivel B para agua real | Preferir T200 oficial o AQUROV ya validado por banco |

---

## 10. Referencias

- QYSEA V6 Expert product page: https://www.qysea.com/products/fifish-v6-expert.html
- QYSEA V6 Expert guides: https://www.qysea.com/support/guides/fifish-v6-expert/
- QYSEA Technical Specifications PDF: https://drive.google.com/file/d/1riSwzhdGuCJ65pwEIcieNLVhPdvefdmO/view?usp=sharing
- Blue Robotics T200: https://bluerobotics.com/store/thrusters/t100-t200-thrusters/t200-thruster-r2-rp/
- Blue Robotics Basic ESC: https://bluerobotics.com/store/thrusters/speed-controllers/besc30-r3/
- Documentos internos: `diagnostico/fifish-v6-expert.md`, `diagnostico/bom-final-nivel-b.md`, `diagnostico/componentes-nivel-b-especificaciones.md`, `diagnostico/reporte-reutilizacion-v6.md`
