# Sourcing AliExpress para BOM Nivel B: ROV correlativo ArduSub / ArduPilot

> Fecha: 2026-07-31.
> Importante: AliExpress cambia listings constantemente y el scraping/directo puede fallar. Los links de este documento son **busquedas de AliExpress**, no listings validados uno por uno. Antes de comprar, revisar seller rating, reviews con fotos, especificaciones reales, origen, garantia y posibilidad de devolucion.
> Regla de seguridad: para partes criticas de presion, propulsion o bateria, AliExpress solo se acepta si el componente pasa validacion propia. Si no hay margen para probar, comprar oficial/distribuidor.

---

## 1. Resumen por riesgo

| Riesgo | Componentes | Criterio |
|---|---|---|
| Bajo | Joystick, JST GH, cables, termocontraible, fusibles, microSD, USB Ethernet, grasa de silicona si es original | Comprar con reviews y especificacion clara |
| Medio | Raspberry Pi 4, camara, BEC 5V, sensores MS5837 para banco, cargador, LEDs genericos | Validar antes de integrar; no usar en lazo critico sin prueba |
| Alto | ESC bidireccionales, thrusters submarinos, bateria Li-ion, tether submarino, penetradores/O-rings, Fathom-X equivalente | No comprar generico para profundidad sin ensayo; preferir oficial |

---

## 2. Matriz de busqueda AliExpress

| BOM Nivel B | Conviene AliExpress? | Riesgo | Que buscar exactamente | Link de busqueda AliExpress | Criterio de aceptacion |
|---|---|---:|---|---|---|
| Navigator Flight Controller | No recomendado | Alto | Mejor oficial/distribuidor | https://www.aliexpress.com/wholesale?SearchText=blue+robotics+navigator+flight+controller | Si aparece, sospechar de precio/origen; verificar SKU BR-100367 y garantia. |
| Raspberry Pi 4 Model B 4GB/8GB | Tal vez | Medio | Original Raspberry Pi 4 Model B 4GB o 8GB | https://www.aliexpress.com/wholesale?SearchText=raspberry+pi+4+model+b+4gb | Debe decir Raspberry Pi 4 Model B original; evitar "compatible", "clone", sin fotos de PCB. |
| Raspberry Pi Camera Module 3 Wide | Tal vez | Medio | Camera Module 3 Wide IMX708 120 grados | https://www.aliexpress.com/wholesale?SearchText=raspberry+pi+camera+module+3+wide+imx708 | Sensor IMX708, CSI 15-pin, Wide 120; probar con Raspberry Pi antes de montar. |
| Basic ESC bidireccional 30A | Tal vez, con validacion | Alto | 30A bidirectional brushless ESC BLHeli_S / AM32 2-6S | https://www.aliexpress.com/wholesale?SearchText=30A+bidirectional+brushless+ESC+BLHeli_S+2-6S | Debe ser bidireccional real, 7-26 V o 2-6S, PWM 1100-1900 us, corriente acorde al thruster; probar reversa y temperatura. |
| T200 thruster con penetrador | No para profundidad | Muy alto | underwater thruster brushless ROV | https://www.aliexpress.com/wholesale?SearchText=underwater+thruster+brushless+rov | Solo banco si es generico. Para 100/200 m, exigir datasheet, rating, penetrador, materiales y repuestos; si no, T200 oficial. |
| Bar30 / MS5837-30BA | Tal vez para banco | Medio-alto | MS5837-30BA pressure sensor module I2C | https://www.aliexpress.com/wholesale?SearchText=MS5837-30BA+pressure+sensor+module+I2C | Debe ser MS5837-30BA, I2C 0x76, 0-30 bar; la estanqueidad depende del bulkhead, no solo del modulo. |
| Power Sense Module | Tal vez con calibracion | Medio | hall current sensor 100A voltage sensor module | https://www.aliexpress.com/wholesale?SearchText=hall+current+sensor+100A+voltage+sensor+module | Debe medir voltaje y corriente; calibrar contra multimetro/pinza; preferir PSM oficial si no hay banco. |
| Fuente 5V 6A / UBEC | Si, con prueba | Medio | UBEC 5V 6A 8A 10A input 4S | https://www.aliexpress.com/wholesale?SearchText=UBEC+5V+6A+4S+ESC+BEC | Salida 5 V estable, 6 A continuos o mas, entrada compatible con 4S, baja emision de ruido, disipador. |
| Fathom-X | No equivalente directo | Alto | powerline ethernet adapter HomePlug AV | https://www.aliexpress.com/wholesale?SearchText=powerline+ethernet+adapter+HomePlug+AV | Solo para experimentar; no es reemplazo submarino directo. Requiere integracion, alimentacion, conectores y prueba de tether. |
| Tether submarino | Tal vez, con mucha validacion | Alto | underwater robot cable ROV tether twisted pair PUR | https://www.aliexpress.com/wholesale?SearchText=underwater+robot+cable+ROV+tether+twisted+pair+PUR | Chaqueta PUR, par trenzado para datos, flotabilidad deseada, 100 m, AWG adecuado; probar continuidad, aislamiento y throughput. |
| Bateria 4S 14.8V 18Ah | No recomendado por envio | Muy alto | 4S 14.8V lithium ion battery pack BMS | https://www.aliexpress.com/wholesale?SearchText=4S+14.8V+lithium+ion+battery+pack+BMS | Riesgo de envio Li-ion a Argentina y seguridad. Mejor proveedor local o bateria certificada con BMS conocido. |
| Cargador H6 PRO / balance charger | Tal vez | Medio | H6 PRO charger / iMAX B6 4S balance charger | https://www.aliexpress.com/wholesale?SearchText=H6+PRO+lithium+battery+charger+4S | Cuidado con clones iMAX. Debe soportar quimica de la bateria, balanceo 4S y corriente adecuada. |
| Penetradores / O-rings | No para profundidad | Muy alto | M10 underwater cable penetrator bulkhead | https://www.aliexpress.com/wholesale?SearchText=M10+underwater+cable+penetrator+bulkhead | Para 100/200 m no usar genericos sin prueba. Preferir WetLink/penetradores con rating y O-rings correctos. |
| Luces submarinas | Tal vez | Medio | underwater LED light ROV 12V 24V PWM | https://www.aliexpress.com/wholesale?SearchText=underwater+LED+light+ROV+12V+24V+PWM | Verificar rating de profundidad, voltaje, corriente, optica y driver; no alimentar desde autopilot. |
| Joystick USB | Si | Bajo | Logitech F310 F710 USB gamepad | https://www.aliexpress.com/wholesale?SearchText=Logitech+F310+F710+USB+gamepad | Original o generico reconocido; probar ejes/botones en Cockpit/QGC. |
| JST GH cables/conectores | Si | Bajo | JST GH 1.25mm cable connector 4 pin 6 pin | https://www.aliexpress.com/wholesale?SearchText=JST+GH+1.25mm+cable+connector+4+pin+6+pin | Verificar pitch 1.25 mm, orientacion y crimpado; comprar surtido. |
| Grasa de silicona para O-rings | Si, si es original | Bajo-medio | Molykote 111 silicone grease O-ring | https://www.aliexpress.com/wholesale?SearchText=Molykote+111+silicone+grease+O-ring | Debe ser grasa de silicona compatible con elastomeros; nunca base petroleo; sospechar de precios muy bajos. |
| Cable tinned marine 14/16 AWG | Si | Bajo | tinned copper marine wire 14AWG 16AWG | https://www.aliexpress.com/wholesale?SearchText=tinned+copper+marine+wire+14AWG+16AWG | Cobre estañado, seccion real, flexible; medir resistencia si se duda. |
| Termocontraible adhesivo | Si | Bajo | adhesive heat shrink tubing waterproof | https://www.aliexpress.com/wholesale?SearchText=adhesive+heat+shrink+tubing+waterproof | Con adhesivo interno para alivio de humedad; no sustituye penetrador estanco. |
| Fusibles/portafusibles | Si | Bajo | inline fuse holder blade fuse 30A 50A waterproof | https://www.aliexpress.com/wholesale?SearchText=inline+fuse+holder+blade+fuse+30A+50A+waterproof | Dimensionar por rama; mejor portafusibles con buena seccion y proteccion. |
| microSD 32/64/128 GB high endurance | Si, con cuidado | Bajo-medio | Samsung SanDisk high endurance microSD 64GB 128GB | https://www.aliexpress.com/wholesale?SearchText=Samsung+SanDisk+high+endurance+microSD+64GB+128GB | Comprar solo vendedores muy confiables; probar capacidad real con H2testw/F3. |
| USB Ethernet adapter laptop | Si | Bajo | USB 3.0 to Ethernet adapter gigabit | https://www.aliexpress.com/wholesale?SearchText=USB+3.0+to+Ethernet+adapter+gigabit | Para laptop sin RJ45; verificar chip y drivers. |
| Heatsink/fan Raspberry Pi | Si | Bajo | Raspberry Pi 4 heatsink fan case | https://www.aliexpress.com/wholesale?SearchText=Raspberry+Pi+4+heatsink+fan+case | No tapar CSI/GPIO; dentro de casco priorizar conduccion termica. |
| Conectores bullet/spade | Si | Bajo-medio | 5.5mm bullet connector spade terminal 6 AWG 14AWG | https://www.aliexpress.com/wholesale?SearchText=5.5mm+bullet+connector+spade+terminal+14AWG | Compatibilidad con PSM/BEC/ESC; crimpado y soldadura correctos. |

---

## 3. Que compraria en AliExpress y que no

### Comprable con riesgo bajo/controlado
- Joystick USB.
- JST GH, cables, termocontraible, fusibles, bullet/spade.
- microSD de marca con verificacion.
- USB Ethernet.
- BEC 5V si se prueba ripple y corriente.
- LEDs genericos solo si se valida profundidad y driver.

### Comprable solo para banco, no para profundidad sin validacion
- MS5837-30BA generico.
- ESC bidireccionales genericos.
- Thrusters genericos.
- Tether generico.
- Camaras compatibles.

### No comprable para este proyecto salvo evidencia muy fuerte
- Bateria Li-ion internacional por AliExpress.
- Penetradores/O-rings genericos para 100/200 m.
- Navigator/Basic ESC/T200/Bar30/PSM/Fathom-X si se necesita confiabilidad inmediata.
- Cualquier cosa sin datasheet o sin reviews tecnicos.

---

## 4. Checklist antes de pagar en AliExpress

- [ ] El listing nombra el chip/modelo exacto, no solo "compatible".
- [ ] Hay datasheet o fotos de PCB donde se vea el modelo.
- [ ] Reviews con fotos y uso tecnico, no solo "llego rapido".
- [ ] El vendedor acepta devolucion o tiene reputacion alta.
- [ ] La especificacion electrica coincide con el BOM: voltaje, corriente, logica, conector.
- [ ] Si es sumergible, el rating de profundidad esta escrito y es creible.
- [ ] Si es bateria, se reviso envio Li-ion a Argentina.
- [ ] Si es critico, hay un plan de prueba antes de integrarlo.
- [ ] El precio final con envio/impuestos sigue teniendo sentido frente a distribuidor local.

---

## 5. Recomendacion practica

Para Nivel B confiable: comprar oficiales/distribuidores Navigator, T200, Basic ESC, Bar30, PSM, Fathom-X, penetradores y bateria. Usar AliExpress para periferia y consumibles: joystick, JST GH, cableado, fusibles, termocontraible, USB Ethernet, microSD verificada, BEC si se prueba, y algunos sensores solo para banco.

---

## 6. Referencias internas

- `diagnostico/bom-final-nivel-b.md`
- `diagnostico/componentes-nivel-b-especificaciones.md`
- `diagnostico/matriz-proveedores-rov.md`
- `diagnostico/informe-conversion-completa-rov.md`
