# Informe a gerencia — FIFISH V6 Expert: análisis, veredicto final y dos opciones de resolución

> **Para:** Gerente responsable del proyecto ROV
> **De:** Equipo técnico
> **Fecha:** 2026-08-13
> **Equipo:** QYSEA FIFISH V6 Expert — Serial `QY50004707`
> **Adjuntos:** `diagnostico/fifish-v6-expert.md` (diagnóstico completo), `Fotos/IMG_9338.jpeg` … `Fotos/IMG_9349.jpeg` (evidencia de la placa)

---

## 1. Resumen ejecutivo

- El ROV **QYSEA FIFISH V6 Expert** quedó expuesto a **agua salada** y fue encendido después del incidente.
- La inspección e intervención final localizó el daño en la **placa electrónica principal** (controlador principal del ROV): la **humedad atacó varios sectores** de la placa.
- Se **reconstruyeron pistas** en las zonas afectadas, pero queda **una conexión (pad/vía) en los pines del microcontrolador sin reparación viable**.
- Resultado: el ROV **arranca parcialmente**, pero las **funciones de control de motores quedaron mal / sin respuesta**.
- La reparación a nivel componente de la placa **no es viable** en el estado actual.
- Se presentan **dos opciones** para decidir:
  - **Opción A — Reparar:** reemplazar la placa main (serie similar `ATL574600045`, validando **versión de firmware**) y comprar un **par de motores de repuesto** (proveedor autorizado: Blue Skies Drones).
  - **Opción B — ROV propio:** estamos en condiciones de **desarrollar nuestro propio ROV** o **adecuar y reutilizar las partes** del equipo dañado. El estudio técnico ya está hecho (arquitectura ArduSub/BlueOS, BOM estimado ~USD 4.215–5.385).

---

## 2. Análisis completo (línea de tiempo)

| # | Paso | Resultado |
|---|---|---|
| 1 | Incidente | Exposición a **agua salada** del FIFISH V6 Expert |
| 2 | Encendido posterior | ROV encendido tras el incidente; **condensación en el lente del domo** |
| 3 | Prueba breve en aire | **5 thrusters** giraron bien; **1 thruster frontal** (cercano al domo) **tembló**; carcasa **~40–45 °C** |
| 4 | Diagnóstico razonado | Falla localizada: hipótesis de **fase perdida / canal ESC corroído** (Rama B eléctrica) vs obstrucción mecánica (Rama A) |
| 5 | Intervención sobre el equipo | Apertura e inspección de la **placa electrónica principal** |
| 6 | Hallazgo de placa | **Humedad en varios sectores**; **pistas reconstruidas**; **una conexión (pad/vía) en los pines del microcontrolador sin reparación viable** |
| 7 | Prueba funcional | **Arranque parcial**; **funciones de control de motores mal / sin respuesta** |
| 8 | Veredicto | Daño distribuido de placa principal; reparación a nivel componente **inviable**; decisión pasa a gerencia |

**Conclusión técnica:** el síntoma inicial (un thruster temblando) era la cara visible de un daño mayor: la humedad alcanzó la placa principal en varios sectores, y la conexión sin reparar sobre los pines del microcontrolador impide que el control de motores funcione, aunque el equipo arranque parcialmente.

**Evidencia:** fotografías de la placa y de la intervención en el repositorio — `Fotos/IMG_9338.jpeg` … `Fotos/IMG_9349.jpeg`.

---

## 3. Veredicto final

| Aspecto | Estado |
|---|---|
| Placa electrónica principal (controlador principal) | Atacada por **humedad en varios sectores** |
| Pistas en zonas afectadas | **Reconstruidas** |
| Conexión (pad/vía) en pines del microcontrolador | **Sin reparación viable** |
| Arranque del ROV | **Parcial** |
| Funciones de control de motores | **Mal / sin respuesta** |

Implicancias:

1. La reparación de la placa actual **no es viable**.
2. La vía de postventa/RMA de QYSEA queda **fuera de alcance** para esta unidad (apertura e intervención no autorizada — descargo del manual oficial, pág. 36).
3. El camino a seguir se reduce a las **dos opciones** de la sección siguiente.

---

## 4. Opción A — Reparación por reemplazo de la placa principal

**Alcance:** devolver el ROV a operación con arquitectura original QYSEA.

**Repuestos y servicios requeridos:**

| Ítem | Detalle |
|---|---|
| Placa main de reemplazo | Serie **similar: `ATL574600045`**. Validar contra la unidad: número/revisión de placa y **versión de firmware** (la placa debe ser compatible con el resto del sistema: ESC, cámara, batería/BMS, app FIFISH) |
| Motores de repuesto | **Un par (2) de thrusters** de repuesto — proveedor autorizado: **Blue Skies Drones** (https://www.blueskiesdroneshop.com/), centro de servicio FIFISH para Norteamérica |
| Trabajos asociados | Reemplazo de placa, re-sellado del casco, **prueba de estanqueidad/vacío antes de sumergir**, calibración por app, prueba funcional en agua dulce |

**Riesgos y consideraciones:**

- **Compatibilidad de firmware/revisión** de la placa `ATL574600045` con esta unidad (punto crítico: pedir confirmación escrita al vendedor).
- El resto del sistema puede tener **daño latente** por la misma humedad (validar durante el reemplazo).
- Logística desde **Argentina**: cotización de importación, aduana y plazos; batería Li-ion sujeta a restricciones IATA para flete aéreo.
- Sin re-certificación oficial local: la prueba de estanqueidad queda a cargo del equipo.

**Costo estimado:** *por cotizar* — placa `ATL574600045` + 2 motores + envío + aduana. *(Completar con cotización.)*

---

## 5. Opción B — ROV propio / reutilización de partes del V6

**Alcance:** independizarse del hardware propietario QYSEA. Dos variantes complementarias:

- **B1 — Desarrollar nuestro propio ROV** con arquitectura abierta **ArduSub/BlueOS** (capacidad interna confirmada; el estudio técnico completo ya está en el repositorio).
- **B2 — Adecuar y reutilizar las partes** del V6 dañado: casco/domo/mecánica, **5 thrusters sanos** (si pasan medición fase-fase y aislamiento), tether (si pasa prueba), lastre y accesorios mecánicos.

**Base técnica ya disponible en el repositorio:**

| Documento | Contenido |
|---|---|
| `informe-conversion-completa-rov.md` | Conversión completa a ArduSub: niveles A/B/C, precondiciones go/no-go |
| `bom-final-nivel-b.md` | BOM Nivel B con links y costos: **~USD 4.215–5.385** (Navigator + RPi4 + 6× T200 + Basic ESC + Fathom-X + batería 4S + consumibles) |
| `matriz-proveedores-rov.md` | Matriz de proveedores ROV/ArduSub |
| `project-schedule-nivel-b.md` | Cronograma y gates del proyecto |
| `reporte-reutilizacion-v6.md` | Qué partes del V6 se reutilizan y con qué condiciones |
| `protocolo-diagnostico-fisico-qmotor.md` | Ensayos para habilitar los thrusters Q-Motor sanos |

**Ventajas:**

- Sin dependencia de repuestos QYSEA ni de su firmware propietario.
- Repuestos estándar del ecosistema Blue Robotics; mantenimiento local total.
- El V6 pasa a ser **donante** de casco y mecánica (B2) o queda como repuesto.

**Desventajas:**

- Se pierden funciones propietarias: cámara 4K original, app FIFISH, batería inteligente, accesorios Q-Interface.
- Proyecto de semanas/meses; la reutilización de partes exige ensayos previos.
- Costo comparable a un BlueROV2 nuevo (~USD 4.900) si se compra todo nuevo; el ahorro real depende de cuánto del V6 se reutilice con seguridad.

---

## 6. Comparativa de opciones

| Criterio | A — Reparar (placa + motores) | B — ROV propio / reutilizar |
|---|---|---|
| Tiempo para volver a operar | Semanas (según importación) | Semanas a meses |
| Costo estimado | Por cotizar (placa + 2 motores + envío/aduana) | ~USD 4,2–5,4 k nuevo; menor si se reutiliza el V6 |
| Dependencia de QYSEA (placa, firmware, repuestos) | Alta | Baja (ecosistema abierto) |
| Funciones originales (cámara 4K, app, accesorios) | Se conservan | Se pierden |
| Riesgo técnico | Compatibilidad firmware/revisión de placa; daño latente | Riesgo de proyecto, controlado y ya documentado |
| Mantenimiento futuro | Sujeto a repuestos QYSEA | Total, con repuestos estándar |

---

## 7. Recomendación

1. **Camino principal propuesto — Opción A**, condicionado a dos validaciones previas:
   - (a) que la placa serie similar `ATL574600045` esté disponible y sea **compatible en revisión y firmware** con la unidad (confirmación escrita del vendedor);
   - (b) que el **costo total importado** (placa + 2 motores + envío + aduana) sea razonable frente al valor del equipo.
   - Si cualquiera de las dos falla → pasar directamente a la **Opción B**.
2. **En paralelo:** cotizar los **2 motores de repuesto** en Blue Skies Drones (https://www.blueskiesdroneshop.com/) y pedir confirmación de compatibilidad de firmware de la placa.
3. **Opción B como plan de respaldo y estratégico:** el estudio técnico está completo; solo requiere aprobación de presupuesto y cronograma para arrancar.

---

## 8. Decisión solicitada

- [ ] Aprobar cotización de la **Opción A**: placa `ATL574600045` + par de motores (Blue Skies Drones).
- [ ] Aprobar presupuesto de la **Opción B**: ROV propio (ArduSub/BlueOS) y/o reutilización de partes del V6.
- [ ] Aprobar **ambas en paralelo** (A como recuperación del equipo, B como proyecto de desarrollo).
