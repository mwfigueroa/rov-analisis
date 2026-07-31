# ROV Analisis

Proyecto de análisis y seguimiento de caso para un **QYSEA FIFISH V6 Expert** expuesto a **agua salada**, con falla localizada en un **thruster frontal** y plan de derivación a servicio autorizado para reparación y re-certificación del sellado.

- Repositorio sugerido: `rov-analisis`
- Versión en inglés: `README.en.md`
- Índice corto: `Readme.md`

---

## 1. Estado actual del caso

| Campo | Estado |
|---|---|
| Equipo | QYSEA FIFISH V6 Expert |
| Ubicación del usuario | Argentina |
| Fecha base del diagnóstico | 2026-07-30 |
| Evento | Exposición a agua salada; el ROV se encendió después del incidente |
| Evidencia inicial de humedad | Condensación en la cara interna del lente del domo |
| Confirmación de ingreso salino | Pendiente: prueba de residuo salino |
| Prueba realizada | Bench test breve en aire |
| Resultado de la prueba | 5 thrusters giraron normalmente; 1 thruster frontal cercano al domo tembló/sacudió |
| Temperatura observada | Carcasa aprox. 40–45 °C cerca de la zona afectada |
| Seguridad eléctrica | Batería Li-ion retirada y guardada de forma segura |
| Estado de intervención | No re-energizado, sin desarmado, sin reparación intentada |

**Fase del proceso:** diagnóstico razonado completo, pendiente de mediciones físicas no destructivas.

---

## 2. Hipótesis de trabajo

### Hipótesis principal
Falla localizada en el **thruster frontal más cercano al domo**, muy probablemente por **fase perdida**:
- fase abierta o de alta resistencia;
- conector corroído;
- canal del ESC dañado por corrosión salina.

Encaja con: exposición a agua salada, condensación cerca del domo, temblor del motor, calentamiento localizado y falla de un solo thruster mientras los otros 5 funcionan.

### Hipótesis alternativa
Rama mecánica: obstrucción o rodamiento trabado por línea, pelo, arena, sal cristalizada o corrosión del eje.

---

## 3. Objetivo del proyecto

Centralizar diagnóstico técnico, evidencia, mediciones pendientes, documentación oficial, correos de RMA y decisión final de reparación/envío/re-certificación.

La meta práctica es llegar a una derivación clara y defendible ante un centro autorizado, evitando daños adicionales por encendidos de prueba, apertura no autorizada o pérdida de estanqueidad.

---

## 4. Alcance

### Incluye
- Análisis del incidente y síntomas.
- Separación entre condensación benigna e ingreso real de agua salada.
- Plan de diagnóstico no destructivo.
- Plantilla de registro de mediciones.
- Correos de solicitud de RMA en inglés y español.
- Resumen de caso de una página en inglés.
- Referencias a documentación oficial QYSEA.
- Ruta recomendada de reparación y envío desde Argentina.

### No incluye
- Desarmado del casco a presión.
- Reparación interna del ROV.
- Re-sellado sin prueba de estanqueidad o certificación.
- Manipulación de batería Li-ion dañada o expuesta a sal.

---

## 5. Estructura del repositorio

| Archivo | Contenido | Uso principal |
|---|---|---|
| `Readme.md` | Índice corto bilingüe | Portada del repositorio |
| `README.es.md` | Documentación completa en español | Este archivo |
| `README.en.md` | Full documentation in English | English version |
| `diagnostico/fifish-v6-expert.md` | Diagnóstico técnico completo, plan de pruebas, plantilla de mediciones, centros de servicio y fuentes | Documento central de análisis |
| `diagnostico/firmware-open-source-rov.md` | Análisis de firmware open-source alternativo | Referencia técnica adicional |
| `diagnostico/ardusub-adaptacion-fifish-v6.md` | Adaptación correlativa ArduSub/ArduPilot | Diseño técnico propuesto |
| `diagnostico/reporte-reutilizacion-v6.md` | Reporte de reutilización de componentes del V6 | Evaluación técnica detallada |
| `diagnostico/informe-conversion-completa-rov.md` | Informe práctico de conversión completa del ROV | Exploración de viabilidad y plan |
| `diagnostico/componentes-nivel-b-especificaciones.md` | Componentes Nivel B con especificaciones | BOM técnico candidato |
| `diagnostico/bom-final-nivel-b.md` | BOM final Nivel B con links y costos | Compra estimada basada en evidencia |
| `diagnostico/matriz-proveedores-rov.md` | Matriz de proveedores ROV/ArduSub | Comparación por origen y conveniencia |
| `diagnostico/diagrama-ardusub-bloques.md` | Diagrama en bloques ArduSub | Arquitectura de componentes |
| `diagnostico/rpi4-imagen-interfaces-firmware.md` | Imagen RPi4, interfaces y firmware ArduSub | Stack de software/hardware |
| `diagnostico/aliexpress-sourcing-nivel-b.md` | Sourcing AliExpress BOM Nivel B | Búsquedas y riesgos por componente |
| `resumen/case-summary-en.pdf` | Resumen de caso de 1 página en inglés | Adjunto para RMA |
| `rma/correos-reparacion-EN.md` | Correos de RMA en inglés para Europa y Norteamérica | Enviar a centros oficiales |
| `rma/correos-reparacion-ES.md` | Versión en español de los correos | Referencia hispanohablante |
| `docs/QYSEA_V6Expert_QuickStart_Manual_V2.2_EN.pdf` | Manual oficial Quick Start V2.2 | Referencia oficial |
| `docs/QYSEA_V6Expert_QuickStart_Manual_V2.2_EN_extracted-text.txt` | Texto extraído del Quick Start | Búsqueda y citas rápidas |
| `docs/QYSEA_V6Expert_Motors_Battery_Maintenance_Guide.pdf` | Guía oficial de mantenimiento de motores y batería | Referencia oficial de mantenimiento |

---

## 6. Hallazgos clave de la documentación oficial

- Los thrusters **no deben girar en aire más de 30 segundos**.
- Después de cada inmersión, el ROV debe enjuagarse/limpiarse con **agua dulce**.
- La app FIFISH incluye una rutina de limpieza de motores de aproximadamente **10 minutos**.
- La batería debe almacenarse aproximadamente entre **50 % y 60 %** y cargarse a full cada **90 días** si no se usa.
- QYSEA **no publica un procedimiento de desarmado del casco a presión** para el V6 Expert.
- La postventa puede quedar excluida por modificación no autorizada, desarmado/apertura no conforme o servicio no autorizado.

**Conclusión operativa:** no abrir el casco. Completar diagnóstico no destructivo y derivar a centro autorizado.

---

## 7. Síntomas e interpretación

### Síntomas observados
1. Exposición a agua salada.
2. Encendido posterior al incidente.
3. Condensación dentro del lente del domo.
4. En prueba breve en aire, solo un thruster tembló: el frontal cercano al domo.
5. Carcasa tibia/caliente, aprox. 40–45 °C, cerca de la zona afectada.

### Interpretación técnica
Un motor BLDC que **tiembla y calienta** está consumiendo corriente sin producir giro útil.

| Rama | Causa probable | Indicador |
|---|---|---|
| Mecánica | Obstrucción, eje trabado, rodamiento corroído | Al girarlo a mano se siente áspero, trabado o distinto a los demás |
| Eléctrica | Fase perdida, conector corroído, canal ESC dañado | Gira libre a mano, pero con corriente tiembla; resistencias fase-fase desbalanceadas |

**Dato clave:** 5 thrusters funcionaron bien en la misma prueba en aire; el aire no explica la falla de uno solo.

---

## 8. Condensación vs. ingreso real de agua salada

| Señal | Interpretación |
|---|---|
| Niebla uniforme que desaparece al calentar | Probable condensación benigna del aire interno |
| Gotas, escurrimientos o agua acumulada | Ingreso real de líquido |
| Residuo blanco después de secar | Sal: entró agua de mar |
| Residuo verde/azul | Corrosión de cobre |
| Humedad en bahía de batería u otros compartimentos | Ingreso real |
| Alarma de fuga/humedad en app | Ingreso real detectado |

**Prueba pendiente:** pasar algodón limpio por superficies internas accesibles y verificar si queda residuo salino.

---

## 9. Diagnóstico pendiente

- [ ] Identificar físicamente el thruster sospechoso: frontal izquierdo o frontal derecho.
- [ ] Elegir un thruster sano de referencia del mismo tipo.
- [ ] Giro a mano sin energía: libre, áspero o trabado.
- [ ] Revisar si hay línea, pelo, arena o sal cristalizada en el eje.
- [ ] Medir resistencia fase-fase: U-V, V-W, U-W.
- [ ] Comparar sospechoso contra referencia.
- [ ] Medir aislamiento fase-tierra/carcasa.
- [ ] Inspeccionar leads, pines, penetrador y zona del ESC si fuera accesible sin abrir el casco.
- [ ] Revisar app: código de falla, thruster señalado, voltaje, alarmas de fuga/temperatura.
- [ ] Confirmar si la condensación fue benigna o ingreso salino real.
- [ ] Confirmar versión del equipo: M100 o M200.
- [ ] Confirmar largo del umbilical.
- [ ] Confirmar número de serie.
- [ ] Confirmar datos de compra: distribuidor y fecha.

---

## 10. Matriz de decisión

| Resultado | Conclusión probable | Acción |
|---|---|---|
| Sospechoso trabado/áspero a mano | Rama mecánica | No energizar; derivar a limpieza/reparación mecánica |
| Sospechoso libre a mano pero tembló con corriente | Rama eléctrica | Medir fases y derivar a reparación de thruster/ESC |
| Una combinación fase-fase abierta o muy alta | Fase corroída/abierta | Reparación de fase/conector o reemplazo |
| Aislamiento fase-tierra bajo | Bobinado comprometido | Probable reemplazo de thruster |
| Residuo blanco/verde/azul | Sal/corrosión confirmada | Limpieza/neutralización + revisión de electrónica asociada |
| Punto caliente sobre bahía de batería | Riesgo térmico | No usar batería; guardar en lugar seguro e inspeccionar |

---

## 11. Ruta recomendada

1. No volver a encender el ROV.
2. Mantener la batería retirada y en lugar seguro.
3. Completar el diagnóstico no destructivo.
4. Completar los datos faltantes del equipo.
5. Enviar los correos de RMA en inglés con adjuntos.
6. Consultar envío desde Argentina, batería Li-ion/IATA, aduana, garantía y re-certificación.
7. Hacer exportación temporal y conservar factura/comprobantes.
8. En servicio autorizado: desarmado, reparación, limpieza salina, prueba de estanqueidad/vacío y re-certificación antes de sumergir.

---

## 12. Centros de servicio considerados

| Centro | Región | Contacto |
|---|---|---|
| FIFISH E.U. Service Center, Hochheim, Alemania | Europa | `support@qysea.com` / `info@vitechgmbh.com` |
| Blue Skies Drones, Centralia, WA, EE.UU. | Norteamérica | `support@blueskiesdroneshop.com` |
| Dominion Drones, Portsmouth, VA, EE.UU. | EE.UU. | `sales@dominiondrones.com` |

Nota: confirmar aceptación de envío internacional antes de despachar.

---

## 13. Advertencias críticas

- Li-ion + agua salada = riesgo térmico.
- No hacer ciclos de encendido para probar.
- No correr thrusters en aire más de 30 segundos.
- No abrir el casco a presión sin autorización.
- O-rings solo con grasa de silicona compatible; nunca base petróleo.
- Un pelo, grano de arena o resto de sal en la ranura del O-ring puede causar una fuga a profundidad.
- Después de cualquier reparación, exigir prueba de estanqueidad/vacío o re-certificación antes de sumergir.

---

## 14. Entregables disponibles

- Diagnóstico técnico completo.
- Correos RMA listos para completar y enviar.
- Resumen de caso en inglés para adjuntar.
- Manuales oficiales y texto extraído.
- Ruta de derivación y envío internacional.

---

## 15. Próximos pasos inmediatos

1. Reemplazar los campos entre corchetes en los correos.
2. Completar la plantilla de mediciones del diagnóstico.
3. Decidir centro de servicio según respuesta de RMA, costo, plazo y logística.
4. Preparar embalaje y documentación para exportación temporal.
5. Confirmar manejo de batería Li-ion según IATA y criterio del centro de servicio.

---

## 16. Fuentes y referencias

Archivos locales:
- `diagnostico/fifish-v6-expert.md`
- `resumen/case-summary-en.pdf`
- `rma/correos-reparacion-EN.md`
- `rma/correos-reparacion-ES.md`
- `docs/QYSEA_V6Expert_QuickStart_Manual_V2.2_EN.pdf`
- `docs/QYSEA_V6Expert_QuickStart_Manual_V2.2_EN_extracted-text.txt`
- `docs/QYSEA_V6Expert_Motors_Battery_Maintenance_Guide.pdf`

Referencias externas:
- QYSEA FIFISH V6 Expert: https://www.qysea.com/products/fifish-v6-expert.html
- QYSEA guides: https://www.qysea.com/support/guides/fifish-v6-expert/
- QYSEA repair: https://www.qysea.com/support/repair/
- QYSEA after-sales: https://www.qysea.com/support/after-sales/

---

## 17. Estado del proyecto

Listo para completar mediciones y enviar solicitud de RMA.  
Bloqueante principal: faltan datos físicos del thruster sospechoso y datos del equipo.  
Decisión ya tomada: no abrir el casco localmente; derivar a servicio autorizado con re-certificación de sellado.
