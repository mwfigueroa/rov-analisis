# Project Schedule — ROV Nivel B ArduSub / ArduPilot

> Estado: Draft listo para ejecutar.
> Alcance: armado de ROV correlativo Nivel B hasta pool-ready. No incluye certificación final 100–200 m ni modificación del FIFISH V6.
> Moneda/tiempos: semanas de proyecto, 1 persona a tiempo parcial, 10–15 h/semana.

## Leyenda

- Estado: Pendiente / En curso / Bloqueado / Hecho
- Gate: criterio de aceptación que habilita la siguiente fase
- Responsable: PM = proyecto, HW = hardware, SW = software, TEST = pruebas, LOG = compras/logística

## Vista Gantt resumida

| ID | Fase | W0 | W1 | W2 | W3 | W4 | W5 | W6 | W7 | W8 | W9 | W10 | W11-12 |
|---|---|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:------:|
| A | Diseño/BOM | X |   |   |   |   |   |   |   |   |   |    |        |
| B | Compras |   | X | X | X | X |   |   |   |   |   |    |        |
| C | Control BlueOS/ArduSub |   |   | X | X |   |   |   |   |   |   |    |        |
| D | Potencia y sensores |   |   |   | X | X |   |   |   |   |   |    |        |
| E | Propulsión |   |   |   |   | X | X | X |   |   |   |    |        |
| F | Mecánica/estanqueidad |   |   |   |   |   | X | X | X | X |   |    |        |
| G | Pileta/validación |   |   |   |   |   |   |   |   |   | X | X |        |
| H | Documentación/handoff |   |   |   |   |   |   |   |   |   |   |    | X      |

## Tabla de tareas

| ID | Semana | Fase | Tarea | Responsable | Duración | Dependencia | Materiales/requerimientos | Evidencia / Gate | Estado |
|---:|---:|---|---|---:|---:|---|---|---|---|
| A1 | 0 | Diseño | Congelar arquitectura Variante B y criterios de aceptación | PM | 1–2 días | — | BOM final, matriz proveedores, informe conversión | G0: BOM y plan de pruebas aprobados | Pendiente |
| B1 | 1 | Compras | Pedir largo lead time: tether, penetradores, batería/cargador, Navigator | LOG | 1 día | A1 | Tether 100 m, penetradores/O-rings, batería 4S, cargador, Navigator | Orden y tracking registrados | Pendiente |
| B2 | 1 | Compras | Pedir propulsión y sensores oficiales | LOG | 1 día | A1 | T200 x6 CW/CCW, Basic ESC x6, Bar30, PSM, Fathom-X par | Orden y tracking registrados | Pendiente |
| B3 | 1 | Compras | Pedir periferia AliExpress validada | LOG | 1 día | A1 | Raspberry Pi 4, cámara, UBEC, joystick, JST GH, cables, fusibles, microSD, USB Ethernet | Lista de vendedores/listings con criterio | Pendiente |
| B4 | 1–4 | Compras | Incoming inspection de todo lo recibido | HW/TEST | continuo | B1-B3 | Multímetro, planilla de inspección | G1: materiales correctos, sin daño, modelo verificado | Pendiente |
| C1 | 2 | Control | Flashear/verificar BlueOS en Raspberry Pi 4 | SW | 0.5–1 día | Navigator/Pi recibidos | Raspberry Pi 4, microSD BlueOS, fuente 5V | BlueOS bootea y expande filesystem | Pendiente |
| C2 | 2 | Control | Montar Navigator + heatsink y alimentar | HW | 0.5 día | C1 | Navigator, heatsink, fuente 5V | Navigator detectada, sin calentamiento anormal | Pendiente |
| C3 | 2 | Control | Acceder a BlueOS y actualizar | SW | 0.5 día | C2 | Laptop, Ethernet directo, red 192.168.2.x | Acceso a `192.168.2.2` / `blueos.local` | Pendiente |
| C4 | 2–3 | Control | Instalar ArduSub desde BlueOS | SW | 0.5 día | C3 | BlueOS con internet | ArduSub instalado; versión registrada | Pendiente |
| C5 | 3 | Control | Conectar QGroundControl/Cockpit y joystick | SW | 1 día | C4 | Laptop, joystick USB, QGC/Cockpit | Telemetría y joystick responden | Pendiente |
| C6 | 3 | Control | Probar video con BlueOS | SW | 1 día | C3 | Cámara Pi/USB/IP, BlueOS/mavlink-camera-manager | Video estable 30 min | Pendiente |
| C7 | 3 | Control | Probar Fathom-X en banco | HW/SW | 1 día | Fathom-X recibido | Fathom-X par, tramo tether de prueba | Throughput video + MAVLink sin cortes | Pendiente |
| D1 | 4 | Potencia | Verificar batería y cargador | HW | 1 día | Batería recibida | Batería 4S, cargador, multímetro | Voltaje por celda OK, BMS OK, sin daño | Pendiente |
| D2 | 4 | Potencia | Cablear fusible, PSM y UBEC | HW | 1 día | D1 | Fusible, PSM, UBEC, cable tinned | Consumo reposo coherente, sin calentamiento | Pendiente |
| D3 | 4 | Potencia | Calibrar battery monitor en QGC | SW | 0.5–1 día | D2 | QGC, PSM, carga conocida | Voltaje/corriente coinciden con referencia | Pendiente |
| D4 | 4 | Sensores | Probar Bar30 en aire y balde | HW/SW | 0.5–1 día | C4, Bar30 | Bar30, balde, I2C | Lectura de profundidad coherente | Pendiente |
| D5 | 4 | Sensores | Probar leak probe y failsafe | TEST | 0.5 día | C4 | Sondas SOS, config failsafe | Fuga simulada dispara acción configurada | Pendiente |
| E1 | 5 | Propulsión | Probar 1 ESC + 1 thruster | HW/TEST | 1 día | D2 | ESC, thruster, fuente/batería, balde | Inicialización 1500 µs, dirección, corriente y temperatura OK | Pendiente |
| E2 | 6 | Propulsión | Conectar 6 ESC y configurar frame | SW/HW | 1–2 días | E1 | 6 ESC, 6 thrusters, ArduSub | Frame Vectored 6 o custom configurado | Pendiente |
| E3 | 6 | Propulsión | Verificar dirección sin hélices | TEST | 0.5–1 día | E2 | ArduSub, GCS | Cada motor gira en el sentido esperado | Pendiente |
| E4 | 6 | Propulsión | Probar mezcla en agua controlada con límites bajos | TEST | 1 día | E3 | Balde/pileta, límites de motor | Mezcla correcta, sin canal cruzado | Pendiente |
| F1 | 7 | Mecánica | Layout interno y cableado | HW | 1–2 días | E4 | Casco/frame, soportes, cableado | Potencia y señal separadas, alivio de tracción | Pendiente |
| F2 | 7 | Mecánica | Instalar penetradores y O-rings | HW | 1 día | F1 | Penetradores, O-rings, grasa silicona | Ranuras limpias, lubricación correcta | Pendiente |
| F3 | 8 | Mecánica | Prueba de vacío/presión antes de agua | TEST | 0.5–1 día | F2 | Bomba/vacío o método definido | G5: no caída anormal de vacío/presión | Pendiente |
| F4 | 8 | Mecánica | Ajuste inicial de flotabilidad/trim | HW | 0.5–1 día | F3 | Lastre/flotación | ROV queda neutro o levemente positivo en seco/teórico | Pendiente |
| G1 | 9 | Pileta | Sesión 1: flotación, manual y estabilizado | TEST | 0.5–1 día | F4 | Pileta, GCS, kit rescate | Control predecible, sin fuga | Pendiente |
| G2 | 9 | Pileta | Sesión 2: Bar30 y depth hold inicial | TEST | 0.5–1 día | G1 | Bar30, ArduSub | Depth hold sin oscilaciones grandes | Pendiente |
| G3 | 10 | Pileta | Sesión 3: heading hold y recorrido corto | TEST | 0.5–1 día | G2 | ArduSub, joystick | Heading estable, recuperación simple | Pendiente |
| G4 | 10 | Pileta | Failsafes: batería baja, telemetría, fuga simulada | TEST | 0.5–1 día | G3 | Config failsafes | Acciones seguras verificadas y logueadas | Pendiente |
| G5 | 10 | Pileta | Acumular 30 min y revisar logs | TEST/PM | 1 día | G1-G4 | Logs, video, telemetría | G6: pool-ready, sin alarmas no explicadas | Pendiente |
| H1 | 11 | Cierre | Exportar parámetros y versiones | SW | 0.5 día | G6 | ArduSub, BlueOS, QGC/Cockpit | Archivos de parámetros y versiones guardados | Pendiente |
| H2 | 11 | Cierre | Diagrama final de cableado y potencia | HW | 1 día | G6 | Fotos, esquema, mediciones | Diagrama actualizado | Pendiente |
| H3 | 12 | Cierre | Checklist pre-dive/post-dive y spares | PM | 0.5–1 día | H1-H2 | Lista de repuestos, consumibles | G7: paquete de operación listo | Pendiente |

## Gates

| Gate | Nombre | Criterio |
|---|---|---|
| G0 | Diseño congelado | BOM, variante, presupuesto y plan de pruebas aprobados |
| G1 | Materiales OK | Todo recibido, inspeccionado y con modelo correcto |
| G2 | Telemetría/video banco | BlueOS + ArduSub + GCS + joystick + video estables |
| G3 | Sensores OK | Bar30, PSM, fuga y failsafes funcionando |
| G4 | Propulsión OK | 6 motores con mezcla, dirección, consumo y temperatura correctos |
| G5 | Estanqueidad seca | Prueba de vacío/presión superada antes de agua |
| G6 | Pool-ready | 30 min acumulados en pileta, failsafes probados, logs limpios |
| G7 | Handoff | Parámetros, diagramas, checklist y spares documentados |

## Riesgos y mitigaciones

| Riesgo | Semana crítica | Impacto | Mitigación |
|---|---:|---|---|
| Batería Li-ion no llega/no se puede enviar | 1–4 | Alto | Comprar local/certificada o definir banco con fuente antes |
| Tether/penetradores demorados | 1–4 | Alto | Pedir semana 1; tener frame abierto para banco |
| CW/CCW de hélices incorrecto | 1–6 | Medio | Definir frame antes de comprar T200 |
| Fathom-X/tether no rinde | 3 | Medio | Probar temprano con tramo corto |
| Fuga en estanqueidad | 8 | Alto | No entrar a agua sin G5; revisar O-rings/penetradores |
| AliExpress tardío o listing incorrecto | 1–4 | Medio | Solo periferia; tener alternativa local/oficial |
| Sobrecalentamiento ESC/Raspberry Pi | 5–8 | Medio | Banco térmico, disipación, límites de motor |

## Regla de seguridad del schedule

- No comprar batería hasta confirmar cargador/BMS/envío.
- No instalar hélices hasta verificar dirección de motores.
- No correr thrusters en seco más que lo mínimo indispensable.
- No entrar a agua sin prueba de vacío/presión.
- No usar componentes AliExpress en lazo crítico sin prueba de banco y plan B.
