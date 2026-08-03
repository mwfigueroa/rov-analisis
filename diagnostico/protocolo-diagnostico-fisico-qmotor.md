# Protocolo de diagnóstico físico del thruster Q-Motor (FIFISH V6 Expert)

> Fecha: 2026-08-03.
> Objetivo: guiar paso a paso el giro a mano, las mediciones fase-fase y el aislamiento del thruster sospechoso del FIFISH V6 Expert, sin depender de datos de fábrica (no disponibles públicamente).
> Premisa de seguridad: batería retirada, ROV apagado, seco, sin encender hasta que se indique. No abrir el casco si el ROV está en garantía o en proceso de RMA. Las mediciones eléctricas detalladas en este protocolo **requieren acceso a los leads del motor** (externos al casco o accesibles sin desarmado completo); si no son accesibles, limitarse al giro a mano, la inspección visual y la telemetría de la app.

---

## 1. Material necesario

- Multímetro digital con autorango, capaz de medir bajas resistencias (hasta 0.1 Ω) y altas resistencias (hasta MΩ).
- Adaptador/gancho miniatura para puntas de multímetro (hacen más segura la medición de pines).
- Linterna o lupa de inspección.
- Alcohol isopropílico ≥99% y bastoncillos de algodón (solo si se necesita limpiar contactos).
- Marcador o cinta de color para etiquetar el thruster sospechoso y el de referencia.
- Cámara (celular suficiente) para fotografiar hallazgos y rotulado de fases.
- Hoja de registro (física o usar la tabla del §2.3 de este documento).

---

## 2. Procedimiento

### 2.1 Giro a mano (sin energía, sin batería)

1. Con la batería fuera, sujetar el ROV de forma segura.
2. Girar **cada uno de los 6 thrusters** con los dedos, aplicando un par suave.
3. Sentir lo siguiente en cada thruster:
   - **Cogging normal:** el imán "paso a paso" típico de un BLDC; debe ser igual en todos.
   - **Áspero / rugoso / arenoso:** sugiere arena, sal cristalizada o rodamiento dañado.
   - **Trabado / no gira o requiere mucha más fuerza:** obstrucción mecánica.
   - **Libre total (sin cogging):** puede indicar imán desprendido o bobinado en corto, pero raro.
4. Revisar la base del eje de cada thruster con linterna:
   - ¿Hay línea de pesca, pelo, arena, sal blanca?
   - ¿Hay corrosión verde o azul alrededor del eje o del conector?
   - ¿Se ve deformación, grietas o decoloración en el pod?
5. **Comparar el sospechoso contra el sano.** Anotar la experiencia como "Libre y con cogging normal", "Áspero", "Trabado", etc.
6. Si el sospechoso está trabado o áspero, **no intentar forzarlo**. Probar remojo en agua dulce y volver a girar después del secado. Si sigue trabado, detener: el diagnóstico es mecánico y va a servicio.

**Nota sobre hélices:** si las hélices son removibles y se puede acceder mejor al eje, quitarlas (ver guía oficial QYSEA de cambio de hélice). No dañar las hélices ni forzar el eje.

### 2.2 Medición eléctrica (fase-fase y aislamiento)

Este paso asume que los leads del motor son accesibles sin abrir el casco (por ejemplo, a través de un conector externo en el penetrador del thruster). Si los leads no son accesibles, **no abrir el casco solo para esta medición**; usar el giro a mano + telemetría de app + consulta a servicio técnico. Si se decide abrir el casco, aplicar las precauciones del documento principal §18 (no oficiales) y asumir pérdida de postventa.

#### 2.2.1 Identificación de los leads

- Localizar los 3 cables/conectores del thruster (fases U, V, W).
- Si no están identificados con colores o letras, etiquetarlos arbitrariamente como U, V y W con cinta y marcador.
- **Usar la misma convención en el thruster de referencia.**

#### 2.2.2 Resistencia fase-fase

1. Configurar el multímetro en la escala más baja de resistencia (Ω) que ofrezca buena resolución (típicamente 200 Ω o autorango).
2. Tocar las puntas entre sí y anotar la resistencia de las puntas (cero de referencia).
3. Medir las tres combinaciones en **el thruster de referencia sano**:

   | Combinación | Resistencia (Ω) |
   |---|---|
   | U – V | |
   | V – W | |
   | U – W | |

   Los valores deben ser **bajos y equilibrados** (fracciones de ohmio a unos pocos ohmios). Como referencia, Blue Robotics documenta para el T200 que los pares de fase deben tener la misma resistencia dentro de **0.1–0.2 Ω** entre sí (fuente: https://github.com/bluerobotics/bluerobotics.github.io/blob/master/thrusters/t200.md). Un BLDC de este tamaño suele estar en el rango de décimas de ohmio, pero el valor exacto depende del cable, conectores y motor.

4. Medir las mismas tres combinaciones en **el thruster sospechoso** y registrar.

5. Comparar sospechoso contra referencia:

   - Si los tres valores están cercanos a los del sano → las fases están bien.
   - Si **una combinación es mucho más alta o el multímetro muestra "OL" (circuito abierto)** → **fase abierta / corroída**, hipótesis eléctrica confirmada.
   - Si los tres valores son mucho más bajos de lo esperado (casi 0 Ω) → posible cortocircuito entre fases.

   **Criterio práctico:** diferencias mayores al 20–30 % respecto del sano se consideran anormales. Como no hay valor de fábrica, la referencia es el thruster sano.

#### 2.2.3 Aislamiento fase-carcasa/tierra

1. Configurar el multímetro en la escala más alta de resistencia (MΩ). Si el multímetro no tiene escala de MΩ, usar la máxima disponible.
2. Identificar un punto de tierra o carcasa del ROV:
   - Si hay metal expuesto en el cuerpo del thruster o su penetrador, usarlo como referencia.
   - Si el ROV tiene algún tornillo o pieza metálica accesible y conectada a la estructura, puede servir.
   - Si no hay tierra accesible, **no hacer mediciones improvisadas en agua**; anotar "no accesible".
3. Medir cada fase contra la carcasa en el thruster de referencia:

   | Fase | Resistencia a carcasa (Ω / MΩ) |
   |---|---|
   | U | |
   | V | |
   | W | |

   Lo esperado es **muy alta resistencia o "OL"** (circuito abierto).

4. Medir las mismas fases en el thruster sospechoso.
5. Comparar:

   - Si todos muestran alta/abierto como el sano → aislamiento OK.
   - Si **una o más fases muestran baja resistencia o continuidad a carcasa** → **aislamiento de bobinado comprometido**, caso grave.
   - Si el multímetro muestra valores variables o ruidosos, puede haber humedad o corrosión parcial.

### 2.3 Tabla de registro (plantilla)

Copiar esta tabla y completar con los valores reales.

```text
## Datos de identificación
Thruster sospechoso: T# ______ (posición: ________________________________)
Thruster de referencia: T# ______ (posición: _____________________________)
Multímetro modelo: ________________  Rango usando: ________
Fecha: ________  Hora: ________  Operador: ________

## A. Giro a mano (sin energía)
| Thruster  | Sensación (libre/áspero/trabado) | ¿Residuo visible? | Notas |
|-----------|---------------------------------|-------------------|-------|
| Sospechoso|                                 |                   |       |
| Referencia|                                 |                   |       |

## B. Resistencia fase-fase (escala baja, Ω)
| Combinación | Sospechoso (Ω) | Referencia (Ω) | ¿Balanceada? (Sí/No) |
|-------------|---------------|----------------|----------------------|
| U – V       |               |                |                      |
| V – W       |               |                |                      |
| U – W       |               |                |                      |

## C. Aislamiento fase-carcasa (escala alta, MΩ o máx. disponible)
| Fase | Sospechoso (Ω/MΩ) | Referencia (Ω/MΩ) | ¿OK? (alto/abierto) |
|------|------------------|-------------------|---------------------|
| U    |                  |                   |                     |
| V    |                  |                   |                     |
| W    |                  |                   |                     |

## D. Inspección visual
| Punto                | Sal (blanco) | Corrosión (verde/azul) | Notas |
|----------------------|-------------|-----------------------|-------|
| Leads del motor      |             |                       |       |
| Pines del conector   |             |                       |       |
| Penetrador/cable     |             |                       |       |
| Eje y base del pod   |             |                       |       |

## E. Conclusión del diagnóstico físico
- [ ] Girado a mano normal y sin residuo.
- [ ] Girado a mano anormal (trabado / áspero).
- [ ] Fase-fase balanceada y cercana a referencia.
- [ ] Fase-fase desbalanceada o abierta.
- [ ] Aislamiento fase-carcasa alto.
- [ ] Aislamiento fase-carcasa bajo.
- [ ] Sal o corrosión visible.
- [ ] No se pudo acceder a los leads eléctricos.

Acción decidida: _________________________________________________________
```

---

## 3. Interpretación

| Resultado | Significado probable | Próximo paso |
|---|---|---|
| Giro libre, fase-fase OK, aislamiento OK | El thruster está mecánica y eléctricamente sano a nivel de motor | Si el sistema sigue bloqueando, el problema está en el ESC, sensor o software (§7A) |
| Giro áspero o trabado | Obstrucción mecánica o rodamiento dañado | No energizar; intentar limpieza con agua dulce y cepillo suave; si no cede, servicio |
| Fase-fase desbalanceada o una abierta | Fase perdida / conector corroído / cable dañado | No energizar; requiere reparación de cableado o canal |
| Aislamiento bajo a carcasa | Bobinado comprometido (cortocircuito a tierra) | Muy grave; reemplazo probable del thruster |
| Sal o corrosión visible | Exposición a agua salada confirmada | Limpiar contactos con IPA, inspeccionar canal ESC, proteger de humedad |
| Leads no accesibles | Diagnóstico eléctrico inviable sin abrir casco | Limitarse a giro a mano + telemetría app; si hay sospecha eléctrica, derivar a servicio autorizado |

---

## 4. Después del diagnóstico

- Si el thruster pasa todas las pruebas (giro libre, fase-fase balanceada, aislamiento alto, sin corrosión) y **el sistema seguía bloqueado**, aplicar la **secuencia de reset por software y falla latcheada** descrita en §7A de `fifish-v6-expert.md`.
- Si se confirma una anomalía eléctrica o mecánica, documentar con fotos y valores, y usar esa evidencia para la solicitud de RMA.
- No reenergizar con medición anormal.
- Si se decide abrir el casco, seguir las precauciones de §18 del documento principal (entorno ESD, fotos, etiquetado de tornillos, control de sal, IPA, desecante, O-rings).

---

## 5. Referencias internas

- `diagnostico/fifish-v6-expert.md` — diagnóstico completo, §8 plan de diagnóstico, §7A reset por software, §15 plantilla de registro, §18 desarmado y precauciones.
- `diagnostico/comparacion-motores-qmotor-vs-t200.md` — comparación con T200 y caracterización no destructiva.
- `rma/correos-reparacion-EN.md` / `rma/correos-reparacion-ES.md` — correos de RMA listos para enviar con el resultado del diagnóstico.
