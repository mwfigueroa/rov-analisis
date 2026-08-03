# Especificaciones de batería para ROV Nivel B con 6 x T200

> Fecha: 2026-08-03.
> Objetivo: definir la batería necesaria para alimentar 6 thrusters T200 + electrónica en el ROV correlativo ArduSub/ArduPilot.
> Base: `diagnostico/bom-final-nivel-b.md` y especificaciones oficiales de Blue Robotics (T200, Basic ESC, Navigator, Fathom-X, PSM).

---

## 1. Requisitos del sistema

| Consumidor | Consumo estimado |
|---|---|
| 6 x T200 a pleno @ 16 V | 6 x 24 A = **144 A** (pico de banco/prueba) |
| 6 x T200 en crucero @ 16 V | 6 x (5–10 A) ≈ **30–60 A** (operación real mixta) |
| 6 x T200 a velocidad baja / depth hold | 6 x (2–5 A) ≈ **12–30 A** |
| Electrónica (Navigator + Raspberry Pi + Fathom-X + cámara) | ~1–2 A @ 5 V (10 W), despreciable frente a motores |

El consumo real del ROV completo en navegación normal es del orden de **20–40 A**, no de 144 A, porque los 6 motores no van a pleno al mismo tiempo.

---

## 2. Especificación recomendada

| Parámetro | Valor | Justificación |
|---|---|---|
| Química | Li-ion (preferido) o LiPo de calidad | Densidad de energía y perfil de descarga adecuado |
| Configuración | **4S** (14.8 V nominal, 16.8 V carga completa) | Rango ideal del T200 (12–16 V recomendado); máximo 20 V |
| Capacidad | **18 Ah** (266 Wh) | Blue Robotics estándar para BlueROV2; buena relación peso/autonomía |
| Capacidad alternativa | 24 Ah (355 Wh) si se busca mayor autonomía | Aumenta peso y volumen; evaluar según misión |
| Corriente continua | **≥ 100 A** | Cubre picos reales de navegación con margen |
| Corriente de pico (10 s) | **≥ 150 A** | Para arranque simultáneo o maniobra brusca |
| C-rate continuo | ≥ 5C (para 18 Ah → 90 A) | Adecuado para navegación; no requiere celdas de ultra-alta descarga |
| BMS | Integrado, con balanceo, protección de sobrecarga/sobredescarga/cortocircuito/temperatura | Seguridad obligatoria; sin BMS no usar |
| Conector de potencia | 5.5 mm bullet o XT60/XT90 según diseño del PSM y distribución | Compatible con Blue Robotics PSM (5.5 mm bullet) o Power Module genérico |
| Rango de temperatura | -10 °C a +60 °C operación; ideal 10–35 °C | Igual que el ROV; no cargar bajo 0 °C |
| Peso estimado | ~1.5–2 kg para 18 Ah 4S Li-ion | Similar a la batería Blue Robotics |
| Dimensiones estimadas | ~150 x 90 x 70 mm (orientativo) | Debe caber en el casco o bahía de batería |

---

## 3. Autonomía estimada

| Régimen | Consumo total estimado | Autonomía con 18 Ah (266 Wh) | Autonomía con 24 Ah (355 Wh) |
|---|---:|---:|---:|
| Navegación suave (depth hold, desplazamiento lento) | ~15 A | ~1 h 12 min | ~1 h 36 min |
| Navegación normal (crucero 25–50 % throttle) | ~30 A | ~36 min | ~48 min |
| Navegación exigente (corriente, maniobras) | ~50 A | ~21 min | ~29 min |
| Full throttle continuo (solo banco/prueba) | ~144 A | ~7 min | ~10 min |

Nota: la autonomía real depende de velocidad, corriente, maniobras, lastre/flotabilidad y condiciones de agua. Los valores son estimaciones de gabinete; validar con logs de corriente en pileta.

---

## 4. ¿3S o 4S?

| Parámetro | 3S (11.1 V) | 4S (14.8 V) |
|---|---|---|
| Empuje T200 | Menor; los charts de 10–12 V muestran caída de empuje | Mayor; 5.25 kgf @ 16 V |
| Corriente a igual empuje | Mayor corriente para mismo empuje (P = V×I) | Menor corriente para mismo empuje |
| Compatibilidad ESC | Basic ESC 7–26 V → OK | OK, en rango recomendado |
| Compatibilidad PSM | 25.2 V max → OK | OK |
| Compatibilidad Fathom-X | 7–28 V → OK | OK |
| Cargador QYSEA existente | 12.6 V → compatible con 3S cargado | No compatible con 4S (necesita 16.8 V) |
| Recomendación | Si se quiere reutilizar cargador QYSEA o mantener voltaje correlativo | Mejor rendimiento y eficiencia para T200; recomendado para Nivel B |

Para el BOM Nivel B, la recomendación es **4S** salvo que haya una razón fuerte para quedarse en 3S (cargador existente, reguladores ya elegidos, etc.).

---

## 5. Opciones concretas

| Opción | Batería | Capacidad | Voltaje | Corriente continua | Precio estimado | Fuente |
|---|---:|---:|---:|---:|---:|---|
| A — Oficial Blue Robotics | Lithium-ion Battery 14.8 V 18 Ah | 18 Ah / 266 Wh | 4S | No publicado exacto; soporta BlueROV2 con 6 x T200 | $425 | https://bluerobotics.com/store/comm-control-power/powersupplies-batteries/battery-li-4s-18ah-r3/ |
| B — Alternativa LiPo | LiPo 4S ≥ 16 Ah ≥ 100 A continua | ≥ 16 Ah | 4S | ≥ 100 A continua | $200–400 | HobbyKing, distribuidores locales de LiPo |
| C — Li-ion DIY seguro | Pack 4S con celdas 21700 conocidas (Samsung 50S, Molicel P42A) y BMS documentado | ≥ 15 Ah | 4S | ≥ 100 A (depende de las celdas) | $150–300 | Armar con celdas nuevas, BMS y documentación |
| D — 3S correlativo | Li-ion 3S ≥ 22 Ah para equivalencia de energía (~244 Wh) | ≥ 22 Ah | 3S | ≥ 100 A | $300–450 | Si se quiere mantener voltaje tipo QYSEA |

La opción **A** es la de menor riesgo. Las opciones B y C requieren validación de BMS, conectores, carga segura y comportamiento térmico; no usarlas sin prueba de banco.

---

## 6. Carga

| Elemento | Recomendación |
|---|---|
| Cargador | H6 PRO (Blue Robotics, $196) o balance charger equivalente de buena marca |
| Capacidad de carga | ≥ 10 A para cargar 18 Ah en ~2 h; ≥ 5 A si no hay apuro |
| Balanceo | Obligatorio; cargador debe balancear celdas individuales |
| Química | Configurable para Li-ion (4.2 V/celda) o LiPo (4.2 V/celda) |
| Seguridad | Cargar en superficie, sobre superficie no inflamable, con supervisión; no cargar dentro del ROV cerrado |

---

## 7. Seguridad y advertencias

- **No reutilizar batería del FIFISH V6** para este ROV si tuvo exposición a agua salada o está en diagnóstico.
- No comprar baterías Li-ion de origen dudoso (AliExpress sin marca/certificación).
- El envío de baterías Li-ion por vía aérea tiene restricciones IATA; verificar con el proveedor y la aduana argentina antes de comprar.
- Almacenar la batería al 50–60 % de carga si no se usa por períodos largos.
- Cargar a full al menos cada 90 días según manual QYSEA y Blue Robotics.
- No perforar, no exponer a agua salada, no cortocircuitar.
- Ante cualquier signo de hinchazón, olor, decoloración o voltaje anormal: retirar de servicio y reciclar.

---

## 8. Referencias

- Blue Robotics T200: https://bluerobotics.com/store/thrusters/t100-t200-thrusters/t200-thruster-r2-rp/
- Blue Robotics Basic ESC: https://bluerobotics.com/store/thrusters/speed-controllers/besc30-r3/
- Blue Robotics Battery 4S 18 Ah: https://bluerobotics.com/store/comm-control-power/powersupplies-batteries/battery-li-4s-18ah-r3/
- Blue Robotics H6 PRO Charger: https://bluerobotics.com/store/comm-control-power/powersupplies-batteries/battery-charger-h6pro-r1/
- Power Sense Module: https://bluerobotics.com/store/comm-control-power/control/psm-asm-r2-rp/
- Documentos internos: `diagnostico/bom-final-nivel-b.md`, `diagnostico/bom-mixto-oficial-aliexpress.md`, `diagnostico/comparacion-motores-qmotor-vs-t200.md`, `diagnostico/protocolo-diagnostico-fisico-qmotor.md`
