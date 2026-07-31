# Diagnóstico — QYSEA FIFISH V6 Expert (ingreso de agua salada)

> Documento de trabajo. Recopila el diagnóstico realizado tras exposición a **agua salada**, con **encendido posterior** al incidente y **condensación observada en el lente del domo**.
> Fecha: 2026-07-30

---

## 0. Estado actual / bitácora

**Última actualización:** 2026-07-30 (2.ª revisión: documentación oficial revisada + derivación a servicio)

**Fase del proceso:** diagnóstico *razonado* completo — **pendiente de mediciones físicas**. No se ha hecho ninguna intervención ni reparación todavía.

**Confirmado hasta ahora:**
- Exposición a **agua salada**.
- Se **encendió después** del incidente.
- **Condensación en el lente del domo** (única evidencia inicial de humedad; falta confirmar si fue niebla benigna o ingreso salino real).
- Prueba realizada **en aire**.
- **Un solo thruster** tembló: el **frontal, cercano al domo**.
- Carcasa **tibia ~40–45 °C**.

**Hipótesis principal:** falla **localizada en un thruster frontal** (junto al domo). Lo más probable, por el temblor + calor + exposición a mar, es **"fase perdida" (Rama B eléctrica)** por corrosión salina en fase/lead/conector o canal del ESC. **No descartada** aún la **Rama A mecánica** (obstrucción/rodamiento).

**Próximo paso decisivo (no realizado):**
1. Girar a mano el thruster sospechoso vs. los sanos → separa mecánico de eléctrico (§8, Paso 1).
2. Medir **fase-a-fase** y **aislamiento** con multímetro (§15, plantilla).

**Pendiente de datos del usuario:** resultados de giro a mano, mediciones del multímetro, inspección de sal/corrosión, y código de falla en la app.

**Avance de la sesión (2026-07-30):**
- Revisada documentación oficial de QYSEA (Quick Start Manual V2.2 + Motors & Battery Maintenance Guide). Ver §17.
- **Hallazgo clave:** la documentación oficial **NO publica pasos de desarmado del casco**; abrir el casco sin autorización **anula la postventa** (descargo, pág. 36). El único desmontaje a nivel usuario es la tapa MicroSD y el programa de limpieza de motores.
- Precauciones generales (no oficiales) para abrir un ROV a presión tras agua salada → §18.
- Centros de reparación autorizados identificados (EU + Norteamérica); usuario en **Argentina** → §19.
- Generados correos de RMA (ES/EN) y resumen de caso en PDF → §20.

**Avance de la sesión (2026-07-31):**
- 🆕 Análisis de firmware open-source alternativo (ArduSub, Betaflight, INAV) → documento separado: [`firmware-open-source-rov.md`](firmware-open-source-rov.md)
- **Conclusión:** ArduSub es la única opción viable, pero migrar el V6 Expert es impractico. El problema es de hardware (thruster), no de firmware.

**Decisión de ruta:** (a) completar diagnóstico **no destructivo** (giro a mano + multímetro) para afinar el alcance y, luego, (b) **enviar a centro autorizado** para desarmado, reparación del thruster y **re-certificación del sellado a 100–200 m**. No se recomienda abrir el casco sin certificación de estanqueidad.

---

## 1. Contexto del equipo

**QYSEA FIFISH V6 Expert** — ROV submarino profesional.

| Aspecto | Detalle |
|---|---|
| Propulsión | 6 thrusters vectoriales → movimiento omnidireccional 360° |
| Profundidad | Versiones **M100 (100 m)** y **M200 (200 m)** según largo del umbilical |
| Cámara | 4K UHD, ~12 MP, FOV amplio, f/1.8 (buena baja luz) |
| Estabilización | Depth lock, heading lock, estabilización por IMU |
| Batería | ~4 h típicas; accesorio de alimentación desde superficie ("onshore power") |
| Accesorios | Brazo/gripper, sonar, láser de medición, muestreador de agua |
| Control | Mando + app; soporte de visor VR con head-tracking |
| Sellado | Casco a presión; kit de fábrica incluye O-rings, tornillos y herramientas |

---

## 2. Síntomas reportados

1. Exposición a **agua salada**.
2. **Se intentó encender** después del incidente.
3. **Condensación en el lente del domo** (única evidencia inicial de "humedad").
4. Al probar (**en aire**): **un solo thruster tembló**, el **cercano al domo**.
5. **Calentamiento** de la carcasa a nivel tibio, ~**40–45 °C**.

---

## 3. Triage de emergencia (agua salada + encendido)

El daño real rara vez es el agua: es el **cortocircuito con corriente** y la **corrosión electrolítica** (agresiva con sal).

1. **No volver a encender** en ciclos de prueba.
2. **Retirar la batería** de inmediato (Li-ion + agua salada = riesgo térmico).
3. Si hubo **líquido salado**: la sal es el enemigo (residuo conductor y corrosivo).
   - Enjuagar zonas afectadas con **agua destilada/desionizada** (arrastra sales).
   - Desplazar agua con **alcohol isopropílico ≥99 %** (baño/botella; se evapora sin residuo).
   - Cepillo antiestático suave; PCB muy afectada → **baño ultrasónico** con IPA.
4. **Secado**: aire seco tibio, **40–50 °C máximo** (sin pistola de calor ni horno), con desecante (sílica gel). 24–48 h.
5. **Inspeccionar corrosión**: residuo verde/blanco en pads, conectores y flex de la cámara.

---

## 4. Condensación ≠ inundación

Ver el **lente empañado por dentro NO prueba ingreso de agua**. En un domo es común el **punto de rocío**: aire húmedo atrapado al cerrar la carcasa que condensa contra la superficie fría al sumergir en agua fría. Es **agua destilada del propio aire interno** — cosmética, sin sal, suele disiparse al calentar. Por eso se recomiendan **pastillas desecantes** dentro.

### Cómo distinguir niebla (benigno) de ingreso real (grave)

| Señal | Significado |
|---|---|
| Niebla uniforme que desaparece al calentar | Condensación de aire atrapado → benigno |
| Gotas / escurrimientos / agua encharcada | Ingreso real de líquido |
| Tras secar, costra/residuo **blanco (sal)** | Entró agua de mar → corrosivo, grave |
| Humedad en bahía de batería u otros compartimentos | Ingreso real |
| Alarma de fuga/humedad en la app (sensor interno) | Ingreso real detectado |
| El ROV pesa más o "suena" a líquido al inclinar | Ingreso real |

**Prueba:** secar y pasar algodón limpio por superficies internas → si sale **residuo salino**, entró mar; si sale seco y solo hubo niebla, fue condensación.

---

## 5. Interpretación del síntoma: temblor + calor

**Motor BLDC que tiembla + calienta = consume corriente alta sin producir giro útil.** Esa energía se vuelve calor (→ 40–45 °C).

Dos ramas posibles:

- **Rama A — Mecánica (obstrucción / rotor trabado):** línea de pesca, pelo, arena o crecimiento marino trabando eje o entrehierro. El motor se atasca (locked rotor), tira corriente altísima → traquetea y calienta. Causa más común y benigna.
- **Rama B — Eléctrica / conmutación (aquí entra la sal):** corrosión en una **fase** del motor, un empalme o los MOSFET del **ESC/driver** → fase débil o parcialmente en corto → conmutación desbalanceada → vibración + sobrecalentamiento del driver.

### Matiz del bench-test en aire
Los thrusters son **refrigerados y cargados por agua**. Correrlos **en aire** > 2–3 s sobrecalienta y da comportamiento errático. **PERO**: como aquí **5 giraron bien y solo 1 tembló** en la misma condición de aire, el aire **no es la excusa** — un BLDC sano en aire gira *más* suelto. **Ese motor está genuinamente fallado.**

---

## 6. Hallazgo clave: falla localizada en 1 thruster (cerca del domo)

- **Un solo motor** afectado → descarta causas sistémicas (batería, placa principal, control de actitud), que harían temblar a varios/todos.
- **Ubicación cercana al domo** → correlaciona con la zona de condensación. Historia coherente de humedad/ingreso localizado alcanzando la ruta eléctrica de ese thruster.

### Síntoma clásico: "fase perdida"
Motor de 3 fases con una **fase abierta o de alta resistencia** (corrosión salina en lead/empalme/canal del ESC) → no arranca limpio: cojea, vibra, traquetea. Es el caso típico y encaja con exposición a mar en la zona del domo.

---

## 7. ⚠️ Advertencias antes de intervenir

1. **Dejar de encender/probar en ciclos** — con motor atascado o mal conmutado, cada intento mete corriente de rotor bloqueado a bobinas/ESC y **acumula daño y calor**.
2. **No probar en aire** — refrigeración y carga son por agua.
3. **Vigilar la batería** — si el punto tibio está **sobre la bahía de batería** y vio agua salada, una celda húmeda/dañada bajo carga = **riesgo de fuga térmica**. Retirar, guardar en sitio seguro/ignífugo, inspeccionar.

---

## 8. Plan de diagnóstico (no destructivo, en orden)

### Paso 1 — Girar a mano los 6 thrusters (SIN energía)  ← el más decisivo
Deben girar libres con ligero "escalonado" magnético (cogging normal). En el motor sospechoso:
- **Trabado / áspero / rechina** vs los otros 5 → **Rama A (mecánico)**: obstrucción o rodamiento corroído.
- **Libre como los demás** pero temblaba con corriente → **Rama B (eléctrico)**: fase/ESC corroído.
- Revisar la base del eje buscando línea/pelo enrollado.

### Paso 2 — Tests eléctricos con multímetro (acceso a los 3 leads del thruster)
1. **Resistencia fase-a-fase** (3 combinaciones):
   - Sano: **equilibradas y bajas** (fracciones de ohm a pocos ohms).
   - Una combinación **mucho más alta o abierta** → **fase corroída/abierta confirmada**.
2. **Aislamiento fase-a-carcasa/tierra**:
   - Sano: **muy alto / abierto** (megaohms).
   - **Baja resistencia / continuidad** → aislamiento de bobinado comprometido (peor caso, raro en motores inundados bien encapsulados).

### Paso 3 — Inspección visual dirigida
Thruster cercano al domo y su **penetrador/cable de entrada al casco**:
- Costra **blanca (sal)** o **verde/azul (corrosión de cobre)** en leads, pines o soldaduras.
- Humedad/sal en el **canal del ESC** que maneja ese motor.

### Paso 4 — Telemetría de la app
Estado por thruster, **códigos de falla**, voltaje de batería bajo carga, alarmas de fuga/temperatura.

---

## 9. Tabla de interpretación

| Observación | Causa probable | Gravedad |
|---|---|---|
| 1 motor trabado/áspero a mano | Obstrucción o rodamiento corroído | Media (mecánico) |
| 1 motor gira libre pero temblaba con corriente | Fase/ESC corroído | Alta |
| Fase-a-fase desbalanceada/abierta | Fase abierta confirmada | Alta |
| Aislamiento fase-a-tierra bajo | Bobinado comprometido | Muy alta |
| Punto caliente sobre batería | Celda dañada/húmeda | **Riesgo térmico** |
| Todo empeora solo en aire | Artefacto de correr en seco | Baja (operacional) |

---

## 10. Panorama de reparación (de mejor a peor)

Al ser **un solo canal**, el alcance es acotado (no un bay de electrónica inundado completo):

1. **Obstrucción mecánica** → liberar el eje, limpiar. *Trivial.*
2. **Fase / lead / conector corroído** → limpiar con IPA, re-terminar/re-soldar. *Intermedio.*
3. **Canal del ESC dañado** → reparación a nivel placa (reemplazo de MOSFET/driver de esa fase). *Avanzado.*
4. **Bobinado en corto** → reemplazo del thruster completo. *Peor caso.*

---

## 11. Nota sobre re-sellado (importante)

Volver a sellar un casco a presión para **100–200 m no es trivial**. Un O-ring mal asentado o par de apriete incorrecto aguanta en tina pero **vuelve a inundarse a profundidad**. Tras reparar, hacer **prueba de estanqueidad / vacío** antes de sumergir, o pasar por centro autorizado para re-certificar el sellado.

**O-rings:** inspeccionar a contraluz (cortes, poros), limpiar la ranura, re-lubricar con **grasa de silicona** (Molykote 111 / Dow Corning) — **nunca** base petróleo (hincha el elastómero). Un pelo o grano de arena = fuga garantizada.

---

## 12. Mantenimiento rutinario (según manuales QYSEA)

- Enjuagar con **agua dulce** tras **cada** inmersión.
- Correr la rutina de motores desde la app (conectar el mando).
- Mantener el plug del tether **seco y con su tapa** siempre (sal/humedad corroen el conector).
- Revisar y reemplazar juntas/washers de goma desgastadas.
- Cap del microSD/hot-shoe: limpio, seco, capa fina de grasa.

---

## 13. Puntos pendientes de confirmar

- [ ] Girar a mano el thruster sospechoso: ¿**trabado/áspero** o **libre**?
- [ ] Medición **fase-a-fase** y **aislamiento** con multímetro.
- [ ] ¿**Sal/corrosión** visible en ese motor / conector?
- [ ] ¿**Código de falla** en la app señalando ese thruster?
- [ ] Confirmar si la condensación fue **niebla benigna** o **ingreso salino real** (test de residuo).
- [ ] Ubicación exacta del punto caliente (¿sobre batería?).

---

## 14. Diagrama del arreglo de thrusters (referencia)

Vista **superior** (top view). El **domo/cámara mira al frente**. Los thrusters cercanos al domo son los **frontales** → aquí el sospechoso.

```
                 FRENTE  (DOMO / CÁMARA)
                          ▲
              T1 ◹  ┌─────────────┐  ◸ T2
          (frente-izq)│             │(frente-der)   ← candidatos "cerca del domo"
                      │             │
              T5 ●    │    CASCO    │  ● T6          ← verticales (heave / arriba-abajo)
          (vertical)  │  (cuerpo)   │(vertical)
                      │             │
              T3 ◺  └─────────────┘  ◿ T4
          (atrás-izq)     ▼        (atrás-der)
                        ATRÁS
```

**Cómo identificar el sospechoso:**
- Es uno de los **frontales (T1 o T2)**, los más próximos al domo.
- Confírmalo físicamente: con energía **desconectada**, marca cuál es el que **temblaba** y anótalo abajo.
- Usa un thruster **sano del mismo tipo** (p. ej. el frontal opuesto) como **referencia de comparación** en las mediciones.

> ⚠️ Esquema orientativo para ubicar posiciones. La geometría exacta (ángulos de vectorización, posición precisa de los verticales) puede variar respecto a tu unidad — confírmalo contra el ROV físico. Lo que importa: **el fallo está en un frontal, junto al domo.**

---

## 15. Plantilla de registro de mediciones

> Rellenar con el ROV **apagado** y la **batería fuera**. Comparar siempre el thruster sospechoso contra uno **sano de referencia**.

**Identificación**

| Campo | Valor |
|---|---|
| Thruster sospechoso (T#) | _______ |
| Thruster de referencia (T#) | _______ |
| Multímetro / rango usado | _______ |
| Fecha de medición | _______ |

**A. Giro a mano (sin energía)**

| Thruster | Gira libre / áspero / trabado | ¿Línea o pelo en el eje? | Notas |
|---|---|---|---|
| Sospechoso | | | |
| Referencia | | | |

**B. Resistencia fase-a-fase** (3 combinaciones) — *esperado: equilibradas y bajas (fracciones de Ω a pocos Ω)*

| Combinación | Sospechoso (Ω) | Referencia (Ω) | ¿Balanceada? (Sí/No) |
|---|---|---|---|
| U – V | | | |
| V – W | | | |
| U – W | | | |

> **Veredicto:** una combinación **mucho más alta o abierta** en el sospechoso → **fase corroída/abierta confirmada**.

**C. Aislamiento fase-a-carcasa/tierra** — *esperado: muy alto / abierto (MΩ)*

| Fase | Sospechoso (Ω / MΩ) | Referencia (Ω / MΩ) | ¿OK? (alto/abierto) |
|---|---|---|---|
| U – tierra | | | |
| V – tierra | | | |
| W – tierra | | | |

> **Veredicto:** **baja resistencia o continuidad** a tierra → **aislamiento de bobinado comprometido** (peor caso).

**D. Inspección visual**

| Punto | Sal (blanco) | Corrosión (verde/azul) | Notas |
|---|---|---|---|
| Leads del motor | | | |
| Pines del conector | | | |
| Penetrador/cable al casco | | | |
| Canal del ESC (si se abre) | | | |

**E. Telemetría de la app**

| Dato | Valor |
|---|---|
| Código de falla | _______ |
| Thruster señalado | _______ |
| Voltaje batería (reposo / bajo carga) | _______ / _______ |
| Alarma de fuga / temperatura | _______ |

**Conclusión del diagnóstico**

- [ ] Rama A — Mecánico (obstrucción / rodamiento)
- [ ] Rama B — Fase abierta / conector corroído
- [ ] Rama B — Canal del ESC dañado
- [ ] Bobinado comprometido (reemplazo de thruster)

Acción decidida: ________________________________________

---

## 16. Fuentes (documentación oficial)

- [FIFISH V6 Expert — QYSEA product page](https://www.qysea.com/products/fifish-v6-expert.html)
- [FIFISH V6 Expert support & guides](http://www.qysea.com/support/guides/fifish-v6-expert/)
- [QYSEA FIFISH V6 Expert Quick Start Manual — ManualsLib](https://www.manualslib.com/manual/2613565/Qysea-Fifish-V6-Expert.html)
- [QYSEA FIFISH V6 Maintenance section — ManualsLib](https://www.manualslib.com/manual/1673005/Qysea-Fifish-V6.html?page=14)
- [Qysea FIFISH V6/V6S User Guide — Manuals+](https://manuals.plus/qysea/fifish-v6v6s-manual)
- Centros de reparación / after-sales: `qysea.com/support/repair/` y `/support/policies/`

---

## 17. Documentación oficial revisada (2026-07-30)

**Archivos oficiales guardados en `P:\Rov`:**
- `QYSEA_V6Expert_QuickStart_Manual_V2.2_EN.pdf` (40 págs.) + `QYSEA_V6Expert_QuickStart_Manual_V2.2_EN_extracted-text.txt`
- `QYSEA_V6Expert_Motors_Battery_Maintenance_Guide.pdf` (V1.0)

**Fuentes (accedidas):**
- Página de guías oficial: https://www.qysea.com/support/guides/fifish-v6-expert/
- PDFs directos (servidor QYSEA hwres): Quick Start V2.2 EN y Motors & Battery Maintenance Guide V1.0.
- ManualsLib devolvió 403 (bloqueado); no accesible por descarga automática.

**Contenido relevante encontrado:**
- Desmontaje a nivel usuario documentado: **tapa protectora MicroSD** (girar en sentido antihorario; aplicar capa fina de grasa en la ranura interior). El casco lleva *venting holes*.
- Mantenimiento de motores: remojo en agua dulce tras cada inmersión + **Cleaning Program** desde la app (~10 min). Batería: 50–60 % para almacenaje; carga completa cada 90 días.
- Seguridad oficial: **no hacer girar los thrusters en aire más de 30 s** (coincide con la prueba realizada).

**Descargo oficial (Quick Start Manual, pág. 36):** QYSEA excluye de la postventa los daños por *"unauthorized modification, disassembly, or shell opening not in accordance with official instructions or manuals"* y los causados por *"a non-authorized service provider"*.

**Conclusión:** la documentación oficial **no publica un procedimiento de desarmado del casco a presión** del V6 Expert. Es un procedimiento restringido de centro de servicio; abrirlo sin autorización anula la postventa.

---

## 18. Desarmado seguro: precauciones (NO oficiales)

> Generales para cualquier casco a presión tras ingreso de agua salada. **No sustituyen** el procedimiento de servicio de QYSEA ni autorizan la apertura.

- **Eléctrico:** no volver a encender; retirar la batería Li-ion y guardarla en sitio ignífugo; descargar condensadores del ESC antes de manipular la placa.
- **Entorno:** superficie limpia, seca, bien iluminada y antiestática (ESD); fotografiar todo y etiquetar tornillos por posición/longitud (no todos son iguales).
- **Apertura del casco:** ventar/despresurizar antes de soltar tornillos; aflojar en patrón **cruzado/estrella y gradual**; no palanquear sobre superficies de sello ni sobre el domo; levantar la tapa en línea recta sin deslizar.
- **Control de sal:** aislar el compartimento afectado; enjuagar con agua destilada/desionizada → alcohol isopropílico ≥99 % → secar a aire tibio 40–50 °C (sin pistola de calor).
- **O-rings y penetradores:** no estirar ni pinzar; inspeccionar a contraluz (cortes/poros); lubricar **solo con grasa de silicona** (Molykote 111 / Dow Corning 111), **nunca** base petróleo; tapar conectores y no tensar cables.
- **Reensamblado (lo más crítico para 100–200 m):** ranuras y asientos del O-ring impecables (un pelo/grano = fuga); apriete en estrella al par correcto; **prueba de estanqueidad/vacío** antes de sumergir o re-certificar en centro autorizado.

---

## 19. Centros de reparación autorizados y derivación

- Fuente: https://www.qysea.com/support/repair/ · Postventa: `support@qysea.com`
- Usuario en **Argentina** (sin centro local directo). Opciones de envío:

| Centro | Región | Contacto |
|---|---|---|
| FIFISH E.U. Service Center (Hochheim, Alemania) | Europa (recomendado) | `support@qysea.com` / `info@vitechgmbh.com` · +49 1735817666 |
| Blue Skies Drones (Centralia, WA, EE.UU.) | Norteamérica (USA/Canadá/México) | `support@blueskiesdroneshop.com` · +1 844 474 8833 |
| Dominion Drones (Portsmouth, VA, EE.UU.) | EE.UU. (alternativa) | `sales@dominiondrones.com` · (757) 300-9183 |

- **Ruta decidida:** completar diagnóstico no destructivo local (§8) → enviar a centro autorizado para desarmado, reparación de thruster/ESC y **re-certificación del sellado a 100–200 m**.
- **Notas de envío desde Argentina:** hacer **exportación temporal** aduanera al sacar el equipo (para no pagar importación al reintroducirlo); conservar factura de compra; consultar al centro si la **batería Li-ion** puede viajar con el equipo (restricciones IATA para flete aéreo) o si se envía sin ella.

---

## 20. Archivos generados en P:\Rov (2026-07-30)

| Archivo | Descripción |
|---|---|
| `case-summary-en.pdf` | Resumen de caso en 1 página (inglés) para adjuntar a los correos |
| `correos-reparacion-centros.md` | Correos de RMA en inglés (Europa + Norteamérica) |
| `correos-reparacion-centros-ES.md` | Mismos correos en español |
| `QYSEA_V6Expert_QuickStart_Manual_V2.2_EN.pdf` | Manual oficial Quick Start V2.2 EN (+ `_extracted-text.txt`) |
| `QYSEA_V6Expert_Motors_Battery_Maintenance_Guide.pdf` | Guía oficial de mantenimiento de motores y batería |
