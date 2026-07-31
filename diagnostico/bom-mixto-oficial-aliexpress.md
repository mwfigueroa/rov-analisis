# BOM mixto: oficial + AliExpress validado + solo banco

> Fecha: 2026-07-31.
> Moneda: USD estimativa, sin impuestos, envio, aduana ni mano de obra.
> Base: `bom-final-nivel-b.md` para precios oficiales con evidencia; `aliexpress-sourcing-nivel-b.md` para busquedas y riesgos AliExpress.
> Advertencia: los precios AliExpress son rangos de mercado orientativos, no listings validados. Todo componente AliExpress que entre al ROV debe pasar prueba de banco y tener repuesto o plan B.

---

## 1. Regla de decision

| Columna | Significado | Entra al lazo critico? |
|---|---|---:|
| Oficial | Comprar a fabricante/distribuidor con datasheet y garantia | Si |
| AliExpress validado | Comprar en AliExpress solo si tiene datasheet, reviews y pasa banco | Condicionado |
| Solo banco | Sirve para aprender/probar, no para profundidad ni operacion final | No |

Lazo critico: control, propulsion, potencia, profundidad, estanqueidad, tether y failsafes.

---

## 2. Matriz de compra por componente

| Componente | Oficial | AliExpress validado | Solo banco | Comentario |
|---|---:|---:|---:|---|
| Navigator Flight Controller | Si | No | No | Nucleo del control; no arriesgar. |
| Raspberry Pi 4 4/8 GB | Si | Si | Si | AliExpress solo si es original con fotos de PCB y vendedor fuerte. |
| Camara Pi/USB/IP | Si | Si | Si | Validar latencia y compatibilidad antes de montar. |
| Basic ESC bidireccional | Si | Condicionado | Si | Para T200 final, oficial. Generico solo si prueba reversa, corriente y temperatura. |
| T200 thrusters | Si | No para profundidad | Si | Generico solo banco; no mezclar con lazo final. |
| Bar30 / MS5837 | Si | Condicionado | Si | El modulo puede ser generico para banco; la instalacion estanca debe ser confiable. |
| Power Sense Module | Si | Condicionado | Si | Generico requiere calibracion contra instrumento. |
| Fuente 5V 6A | Si | Si | Si | UBEC generico aceptable si se mide ripple, corriente y temperatura. |
| Fathom-X | Si | No | Parcial con PLC hogareno | No reemplazar en operacion final sin subproyecto de validacion. |
| Tether 100 m | Si | Condicionado | Si | Para profundidad, official o cable con prueba de datos/aislamiento. |
| Bateria 4S | Si | No recomendado internacional | Solo local/banco | Riesgo termico y de envio; mejor local o certificada. |
| Cargador | Si | Si | Si | Cuidado con clones; debe balancear y soportar quimica exacta. |
| Penetradores/O-rings | Si | No | Solo maqueta | Para 100/200 m, official o prueba de presion formal. |
| Luces | Si | Condicionado | Si | Driver propio; validar profundidad y termica. |
| Joystick USB | Si | Si | Si | Bajo riesgo. |
| JST GH, cables, fusibles, termocontraible | Si | Si | Si | Buenos candidatos AliExpress con control de calidad. |
| microSD alta resistencia | Si | Si | Si | Verificar capacidad real. |
| USB Ethernet laptop | Si | Si | Si | Bajo riesgo. |

---

## 3. Variante A: oficial completa

Usar el BOM final con componentes oficiales para todo lo critico y la mayoria de perifericos.

| Bloque | Estimado |
|---|---:|
| Electronica/propulsion/tether/potencia oficiales, items 1-12 de `bom-final-nivel-b.md` | $3,585-3,925 |
| Integracion/luces/joystick/consumibles | $630-1,460 |
| **Total Variante A** | **$4,215-5,385** |

Uso recomendado: conversion Nivel B seria, operacion en agua y menor incertidumbre.

---

## 4. Variante B: mixta recomendada

Mantiene oficiales las partes criticas y mueve a AliExpress validado solo periferia no critica.

### Oficial en Variante B

| Item | Estimado |
|---|---:|
| Navigator | $220 |
| 6 x T200 con penetrador | $1,440 |
| 6 x Basic ESC | $240 |
| Bar30 | $80-90 |
| Power Sense Module | $92 |
| Fathom-X par | $280 |
| Tether Fathom 100 m | $500-800 |
| Bateria 4S 14.8 V 18 Ah | $425 |
| Cargador H6 PRO | $196 |
| Penetradores/O-rings/conectores criticos | $300-700 |
| **Subtotal oficial critico** | **$3,783-4,483** |

### AliExpress validado en Variante B

| Item | Estimado | Condicion |
|---|---:|---|
| Raspberry Pi 4 original | $45-70 | Verificar originalidad |
| Camara Pi/USB/IP | $20-35 | Probar antes de montar |
| UBEC 5V 6A | $15-30 | Medir ripple/corriente/temperatura |
| Joystick USB | $25-50 | Probar ejes/botones |
| JST GH, cable tinned, fusibles, termocontraible | $60-120 | Control de seccion y crimpado |
| microSD alta resistencia | $10-20 | Verificar capacidad real |
| USB Ethernet laptop | $10-20 | Bajo riesgo |
| Luces genericas con driver | $80-180 | Solo si validan profundidad/termica |
| Consumibles y pruebas | $100-250 | Grasa silicona, IPA, repuestos |
| **Subtotal AliExpress validado** | **$365-775** |

| Total Variante B | Estimado |
|---|---:|
| **Mixta recomendada** | **$4,148-5,258** |

Lectura: el ahorro contra Variante A es modesto, aproximadamente decenas a pocos cientos de USD, porque el costo esta dominado por thrusters, tether, bateria, Navigator y estanqueidad. La Variante B tiene sentido para reducir friccion de compra, no para cambiar el orden de costo total.

---

## 5. Variante C: agresiva AliExpress / solo banco

Objetivo: aprender ArduSub y validar control en banco o frame abierto de agua dulce. **No es para 100/200 m ni para operacion final.**

| Bloque | Estimado | Nota |
|---|---:|---|
| Control: Navigator + Raspberry Pi 4 + BlueOS SD + camara + UBEC + joystick | $335-425 | Se mantiene Navigator para no cambiar el stack |
| Propulsion generica de banco: 6 thrusters + 6 ESC | $450-900 | Solo banco; no profundidad |
| Sensores genericos: MS5837 modulo + power sense generico | $30-80 | Calibracion obligatoria |
| Potencia de banco: bateria 4S local/generica + cargador | $190-380 | BMS conocido; no Li-ion dudoso |
| Comunicacion de banco: Ethernet directo/USB | $10-50 | Sin Fathom-X |
| Frame abierto / maqueta | $100-300 | Sin exigencia de estanqueidad |
| **Total Variante C** | **$1,115-2,135** | Solo aprendizaje/banco |

Uso recomendado: validar ArduSub, mezcla de motores, depth hold basico en pileta y failsafes antes de comprometer dinero en propulsion/tether/estanqueidad reales.

---

## 6. Comparacion de variantes

| Variante | Costo estimado | Riesgo | Sirve para operacion final | Comentario |
|---|---:|---:|---:|---|
| A Oficial completa | $4,215-5,385 | Bajo | Si | Maxima previsibilidad |
| B Mixta recomendada | $4,148-5,258 | Medio-bajo | Si, con validaciones | Ahorro modesto; buena friccion de compra |
| C Agresiva banco | $1,115-2,135 | Alto | No | Aprendizaje, pileta, frame abierto |

---

## 7. Decision practica

1. Si el objetivo es aprender: empezar con Variante C en banco/pileta.
2. Si el objetivo es convertir en serio: saltar a Variante A o B; no intentar estirar la Variante C a profundidad.
3. Si se reutiliza el V6: cualquier ahorro real viene de reutilizar mecanica/tether/thrusters solo despues de ensayo, no de comprar genericos para el lazo critico.

---

## 8. Checklist para que un componente AliExpress pase de "solo banco" a "validado"

- [ ] Datasheet o modelo exacto visible.
- [ ] Reviews tecnicos con fotos.
- [ ] Prueba electrica en banco superada.
- [ ] Prueba termica superada.
- [ ] Prueba funcional con ArduSub/BlueOS superada.
- [ ] Si es sumergible, prueba de presion/estanqueidad superada.
- [ ] Hay repuesto o plan B documentado.
- [ ] Se registro link, vendedor, fecha, precio y resultado de prueba.

---

## 9. Referencias internas

- `diagnostico/bom-final-nivel-b.md`
- `diagnostico/aliexpress-sourcing-nivel-b.md`
- `diagnostico/componentes-nivel-b-especificaciones.md`
- `diagnostico/informe-conversion-completa-rov.md`
- `diagnostico/reporte-reutilizacion-v6.md`
