# ROV FIFISH V6 Expert — caso cerrado técnicamente

**QYSEA FIFISH V6 Expert**, serial **QY50004707**, expuesto a agua salada el
**2026-07-30** y encendido después del incidente.

Esto no es un proyecto de desarrollo: es un caso técnico **con veredicto final
emitido el 2026-08-13**. La decisión que falta no es técnica, es gerencial.

## Veredicto (2026-08-13)

La humedad atacó **varios sectores de la placa electrónica principal**. Se
reconstruyeron pistas, pero quedó **una conexión (pad/vía) en los pines del
microcontrolador sin reparación viable**.

Resultado: el ROV **arranca parcialmente** y el **control de motores no
responde**. La reparación a nivel componente **está descartada**.

Antes de eso, en la prueba breve en aire: 5 thrusters giraron bien, **1 thruster
frontal** cercano al domo tembló, carcasa a **40–45 °C**. Esa hipótesis de fase
perdida quedó superada por el hallazgo en la placa.

## La decisión abierta — no la cierres vos

`diagnostico/informe-gerencia-resolucion.md` presenta dos opciones y la elección
es de gerencia:

| | |
|---|---|
| **Opción A — Reparar** | Reemplazar la placa main (serie similar `ATL574600045`, **validando versión de firmware**) + par de motores de repuesto. Proveedor autorizado: Blue Skies Drones |
| **Opción B — ROV propio** | Desarrollar uno propio o reutilizar las partes. Estudio técnico ya hecho: arquitectura ArduSub/BlueOS, BOM estimado **USD 4.215–5.385** |

Si te preguntan "qué conviene", podés analizar las dos — pero no presentes una
como decidida. El informe está escrito para que decida otra persona.

**La Opción B ya no es hipotética.** Los documentos de conversión
(`ardusub-adaptacion-fifish-v6.md`, `reporte-reutilizacion-v6.md`,
`informe-conversion-completa-rov.md`, BOM nivel B, sourcing) y las imágenes
BlueOS en `docker/` son insumo de una opción sobre la mesa, no material de
descarte.

## Estado del equipo

La batería Li-ion está retirada y guardada. La placa **ya fue intervenida**: no
apliques las precauciones de la fase de diagnóstico previo (no abrir, no
re-energizar) como si el equipo estuviera intacto — ese momento pasó.

## Estructura

| Carpeta | Qué hay |
|---|---|
| `diagnostico/` | 18 documentos: diagnóstico completo, informe a gerencia, protocolo Q-Motor, BOM nivel B, sourcing, comparación de motores |
| `rma/` | correos de reparación, ES y EN |
| `docs/` | manuales oficiales QYSEA + texto extraído |
| `Fotos/` | evidencia; `IMG_9338`–`IMG_9349` son las fotos de la placa citadas por el informe |
| `resumen/` | `case-summary-en.pdf` |
| `docker/` | imágenes BlueOS core (amd64 y arm64) |

Documentación bilingüe: `README.es.md` / `README.en.md`, índice en `Readme.md`.
Al editar, **actualizá las dos versiones**: el caso se comparte en inglés.
