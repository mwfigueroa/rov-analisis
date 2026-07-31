# ROV Analisis / ROV Analysis

Índice corto del proyecto / Short project index.

## Idiomas / Languages

- Español: `README.es.md`
- English: `README.en.md`

## Resumen rápido / Quick summary

**ES:** Caso de análisis para un **QYSEA FIFISH V6 Expert** expuesto a agua salada. Se encendió después del incidente, hubo condensación en el domo y un thruster frontal cercano al domo tembló en una prueba breve en aire. La batería está retirada, el equipo no debe re-energizarse y no se recomienda abrir el casco. Próximo paso: completar mediciones no destructivas y enviar RMA a servicio autorizado.

**EN:** Analysis case for a **QYSEA FIFISH V6 Expert** exposed to saltwater. It was powered on after the incident, condensation appeared inside the dome, and one front thruster near the dome trembled during a brief air test. The battery is removed, the unit must not be re-energized, and opening the hull is not recommended. Next step: complete non-destructive measurements and send an RMA request to an authorized service center.

## Archivos clave / Key files

| Archivo / File | Contenido / Content |
|---|---|
| `diagnostico/fifish-v6-expert.md` | Diagnóstico técnico completo (ES) |
| `diagnostico/firmware-open-source-rov.md` | 🆕 Análisis de firmware open-source alternativo (ES) |
| `diagnostico/ardusub-adaptacion-fifish-v6.md` | Adaptación correlativa ArduSub/ArduPilot (ES) |
| `diagnostico/reporte-reutilizacion-v6.md` | Reporte de reutilización de componentes del V6 (ES) |
| `diagnostico/informe-conversion-completa-rov.md` | Informe práctico de conversión completa del ROV (ES) |
| `diagnostico/componentes-nivel-b-especificaciones.md` | Componentes Nivel B con especificaciones (ES) |
| `diagnostico/bom-final-nivel-b.md` | BOM final Nivel B con links y costos (ES) |
| `diagnostico/matriz-proveedores-rov.md` | Matriz de proveedores ROV/ArduSub (ES) |
| `diagnostico/diagrama-ardusub-bloques.md` | Diagrama en bloques ArduSub (ES) |
| `diagnostico/rpi4-imagen-interfaces-firmware.md` | Imagen RPi4, interfaces y firmware ArduSub (ES) |
| `diagnostico/aliexpress-sourcing-nivel-b.md` | Sourcing AliExpress BOM Nivel B (ES) |
| `diagnostico/bom-mixto-oficial-aliexpress.md` | BOM mixto oficial/AliExpress/banco (ES) |
| `resumen/case-summary-en.pdf` | Resumen para RMA / RMA summary (EN) |
| `rma/correos-reparacion-EN.md` | Correos RMA inglés / EN emails |
| `rma/correos-reparacion-ES.md` | Correos RMA español / ES emails |
| `docs/` | Manuales oficiales QYSEA |

## Links open-source recomendados / Recommended open-source links

**ArduSub / ArduPilot**
- Documentación principal / Main docs: https://ardupilot.org/sub/
- Código fuente / Source code: https://github.com/ArduPilot/ardupilot
- Carpeta ArduSub / ArduSub folder: https://github.com/ArduPilot/ardupilot/tree/master/ArduSub
- Foro ArduPilot / ArduPilot forum: https://discuss.ardupilot.org/

**Blue Robotics / ecosistema ROV**
- Open-source Blue Robotics: https://bluerobotics.com/open-source/
- GitHub Blue Robotics: https://github.com/bluerobotics
- Foro Blue Robotics: https://discuss.bluerobotics.com/
- Builds / experiencias: https://discuss.bluerobotics.com/c/build/7
- Adventure logs / bitácoras: https://discuss.bluerobotics.com/c/adventure-log/10
- Guías / Learn: https://bluerobotics.com/learn/
- Referencia técnica / Technical reference: https://bluerobotics.com/learn/technical-reference/

**Software de control / companion**
- BlueOS: https://blueos.cloud/ · Docs: https://blueos.cloud/docs/
- Cockpit: https://github.com/bluerobotics/cockpit
- QGroundControl: https://qgroundcontrol.com/ · Docs: https://docs.qgroundcontrol.com/
- MAVLink: https://mavlink.io/

**Hardware usado en el BOM Nivel B / Level B BOM hardware**
- Navigator: https://bluerobotics.com/store/comm-control-power/control/navigator/
- T200 thruster: https://bluerobotics.com/store/thrusters/t100-t200-thrusters/t200-thruster-r2-rp/
- Basic ESC: https://bluerobotics.com/store/thrusters/speed-controllers/besc30-r3/
- Bar depth/pressure sensors: https://bluerobotics.com/store/sensors-cameras/sensors/bar-depth-pressure-sensor/
- Fathom-X: https://bluerobotics.com/store/comm-control-power/tether-interface/fathom-x/
- Power Sense Module: https://bluerobotics.com/store/comm-control-power/control/psm-asm-r2-rp/

## Nota Fathom-X / Fathom-X note

**ES:** La Fathom-X es reemplazable solo si otra tecnología cumple la misma función: Ethernet/IP de largo alcance por tether con video + MAVLink. Para banco/pileta puede usarse Ethernet directo o WiFi. Para 100 m sumergido, mantener Fathom-X salvo prueba end-to-end de otra opción. Alternativas: otro HomePlug AV/IEEE-1901 compatible, extensores VDSL2/G.hn/EoC, fibra óptica, o RS-485/CAN solo para control sin video. No mezclar HomePlug con G.hn/VDSL/MoCA. Si se reutiliza el tether del V6, primero mapear conductores y probar; si no tiene un par sano, usar tether nuevo.

**EN:** Fathom-X is replaceable only if another technology performs the same function: long-range Ethernet/IP over tether with video + MAVLink. For bench/pool tests, direct Ethernet or WiFi may work. For 100 m submerged, keep Fathom-X unless another option passes end-to-end testing. Alternatives: compatible HomePlug AV/IEEE-1901, VDSL2/G.hn/EoC extenders, fiber optics, or RS-485/CAN for control-only without video. Do not mix HomePlug with G.hn/VDSL/MoCA. If reusing the V6 tether, map conductors and test first; if no healthy pair exists, use a new tether.

## Estado / Status

**ES:** listo para completar mediciones y enviar solicitud de RMA.  
**EN:** ready to complete measurements and send the RMA request.
