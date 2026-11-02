# CanSat ÁBREGO-1 · Misión VIGÍA

Proyecto de un equipo que participa en **CanSat Spain 2026/2027** (ESERO Spain / ESA). Un CanSat es un minisatélite del tamaño de una lata de refresco. Un cohete lo suelta a 500–1000 m y desciende en paracaídas mientras mide y transmite datos.

Este repositorio contiene, por ahora, **una sola cosa**: `cansat.html`, un modelo 3D interactivo que sirve de **propuesta de diseño**. Muestra la estructura, los componentes, el presupuesto, los requisitos de las bases y una simulación del descenso con la cámara térmica.

> **Importante:** nada está construido ni comprado. Los modelos de componentes, las masas, los costes y la autonomía son **estimaciones** de una propuesta. El equipo puede cambiar cualquier decisión. El diseño original del modelo era solo un ejemplo y la misión secundaria se redefinió después (ver «Historia»).

---

## 1. Cómo verlo

- Abre `cansat.html` con doble clic en Chrome, Edge, Firefox o Safari. **Necesita internet**: carga three.js desde jsDelivr y las fuentes desde Google Fonts.
- No hay build, ni dependencias locales, ni servidor. Es un único archivo HTML con CSS y JS en línea.
- Pestañas:
  - **Modelo**: lata montada, con cotas, rayos X, recorrido de cámara y etiquetas clicables.
  - **Despiece**: piezas separadas y flujo de datos y energía animado.
  - **Descenso**: simulación del vuelo con telemetría, gráficas y **vista de la cámara térmica**.
- **Ficha técnica** (botón arriba a la derecha): misión secundaria, lista de materiales (BOM) con totales y requisitos R1–R15.

El archivo no lleva `<!doctype>`, `<html>` ni `<head>`. Está pensado para publicarse como artefacto de claude.ai, que lo envuelve en un documento completo, aunque también funciona abierto en local.

---

## 2. La competición (bases 2026/2027)

Fuente: [bases del desafío CanSat 2026/2027](https://esero.es/wp-content/uploads/2026/09/bases-del-desafio-cansat-2026-2027.pdf) · [página del desafío](https://esero.es/desafios/cansat/)

**Misiones**
- **Primaria (§3.1), obligatoria:** medir temperatura del aire y presión durante el descenso, transmitirlas en vuelo y analizarlas después.
- **Secundaria (§3.2), libre:** debe tener objetivos claros, valor científico, tecnológico o innovador, una forma de evaluar los resultados y ser viable. El §3.3 da seis ejemplos: telemetría avanzada, telecontrol, aterrizaje controlado, sistema alternativo de aterrizaje, estudio ambiental y sonda planetaria u observación del terreno.

**Requisitos técnicos clave**

| | Requisito |
|---|---|
| R1 | Volumen de Ø66 × 115 mm antes de la liberación. Antenas solo arriba o abajo. Nada sobresale lateralmente. Como máximo 45 mm extra en altura. |
| R2 | Masa entre 300 y 350 g, con lastre si hace falta. |
| R3 | Prohibidos explosivos, pirotecnia, materiales inflamables y **baterías LiPo**. |
| R4 | Al menos **4 h encendido**. Baterías fácilmente accesibles. |
| R5 / R11 | Interruptor general e interruptor de transmisión accesibles. |
| R6–R8 | Recuperación reutilizable. La unión debe soportar 50 N. Se recomiendan 8–12 m/s y como máximo 120 s de vuelo. |
| R9 | Soportar 20 g en el lanzamiento. |
| R10 / R12 | Frecuencia asignada por la organización y configurable. Datos identificables (ID propio). |
| R13 | Localización recomendada: GPS o zumbador. |
| R14 | **500 € como máximo** en componentes de vuelo, con los patrocinios a valor de mercado. |

**Evaluación (§8.2):** logros técnicos 35 %, **valor científico 30 %** (con atención especial a la misión secundaria), competencias profesionales 25 % y difusión y financiación 10 %. Si el vuelo falla, también se valora saber analizar el fallo.

**Fechas:**

| Hito | Fecha |
|---|---|
| Inscripción | Hasta el 30 de noviembre de 2026 |
| **PDR** | **Antes del 18 de diciembre de 2026** |
| CDR | Según la comunidad autónoma |
| Finales autonómicas | Marzo–abril de 2027 |
| Final nacional | 12–13 de mayo de 2027, en Granada |
| Cumbre en ESTEC | 30 de junio–2 de julio de 2027 |

---

## 3. Misión secundaria: VIGÍA, sonda lanzable para incendios forestales

**Pregunta científica:** ¿puede una sonda lanzable de bajo coste dar, en menos de dos minutos, la información meteorológica y térmica que necesita el puesto de mando de un incendio forestal?

**Motivación:** en 2025 ardieron en España más de 350.000 ha. Se decide con estaciones lejanas y modelos de varios kilómetros. El viento local, la humedad del combustible fino y la estabilidad del aire sobre el fuego no se miden in situ. La misión enlaza con LSTM, la futura misión térmica de Copernicus, y con las *dropsondes* de huracanes.

**Objetivos**

| | Objetivo | Cómo |
|---|---|---|
| O1 | Perfil vertical de viento y estabilidad | Viento a partir de la deriva GNSS. Gradiente térmico, inversiones y punto de rocío con TMP117 y SHT45. |
| O2 | Mapa térmico del terreno en descenso | MLX90640 mirando abajo y orientación con BNO085. Un píxel mide unos 30 m a 1000 m y unos 3 m a 100 m: la caída es un «zoom» de la escala de satélite a la de dron. Experimento estrella: **curva de detectabilidad de un blanco de 3 × 3 m frente a la altitud**. |
| O3 | Modo centinela tras aterrizar (≥ 4 h) | T y HR, más la humedad del combustible fino muerto (HCFM) y la probabilidad de ignición, calculadas a bordo con las tablas que usan los bomberos forestales y enviadas cada 30 s. |
| O4 | Telecontrol | Enlace bidireccional para cambiar el modo o la frecuencia, pedir una imagen o activar el zumbador. |

**Validación prevista:**
- Viento frente a un anemómetro en tierra y ERA5 (diferencia media menor de 2 m/s).
- Mosaico térmico frente a la temperatura de superficie de Landsat 8/9.
- HCFM frente a muestras reales pesadas, secadas en horno y pesadas otra vez.
- 4 h de prueba junto a una estación de AEMET o SIAR.
- Caídas desde dron con un operador autorizado: en categoría abierta no se puede soltar carga.

**Niveles de alcance:**
- **Nivel 1, garantizado:** O1 y O3.
- **Nivel 2:** imagen térmica y detección de focos.
- **Nivel 3:** mosaico georreferenciado, curva de detectabilidad y telecontrol completo.

**Riesgos y mitigación:**

| Riesgo | Mitigación |
|---|---|
| Imagen emborronada por el giro | Destorcedor y descarte de fotogramas con el giroscopio |
| Retardo del SHT45 (unos 40 m de caída) | Corrección de retardo |
| Ciclo de trabajo de radio en 433 MHz (suele ser del 10 %) | Paquetes compactos |
| La organización no permite colocar el blanco | Contrastes naturales: caminos, edificios o agua |

---

## 4. Hardware propuesto

La **fuente de verdad es el array `BOM` de `cansat.html`**: los totales de la ficha se calculan a partir de él. Resumen actual: **unos 328 g y unos 300 €** (sin IVA ni envío).

| Grupo | Pieza |
|---|---|
| Estructura | Tubo de aluminio 6063 de Ø66 × 1,2 mm. Tapas y 6 bandejas de PETG impresas en 3D. La tapa inferior tiene una ventana de Ø12 mm. 3 varillas M3. Cáncamo M4. **Lastre: anillo de latón Ø62/Ø40 × 4 mm, unos 60 g** |
| Misión primaria | Adafruit BMP390 (presión) y TMP117 (temperatura) |
| Placa | Adafruit Feather RP2040 Adalogger con microSD de 16 GB. La cámara va en el 2.º bus I²C, a 1 MHz |
| Batería | 4 × NiMH AAA eneloop (4,8 V, 750 mAh). Unos 105 mA de consumo y unas 6 h de autonomía |
| Controles | 2 interruptores EG1218: PWR y TX |
| Comunicaciones | RFM96W LoRa a 433 MHz (frecuencia supuesta), antena helicoidal Linx en la base y coaxial de 15 cm |
| Misión secundaria | **MLX90640 55° (Adafruit 4407, unos 70 €, agotada al consultarla el 2 de noviembre de 2026)**, **BNO085 (Adafruit 4754)**, SHT45, GNSS MAX-M10S con parche y zumbador PS1240 |
| Paracaídas | Ripstop naranja de Ø30 cm y 8 gajos, cuerdas de Dyneema y **destorcedor** |

### Distribución interna (de abajo arriba, coordenadas `y` en cm)

| `y` (cm) | Elemento |
|---|---|
| < 0 | Antena LoRa, fuera de la lata, desplazada a `ANT=[-1.5,-.7]` (unos 17 mm del eje) para no entrar en el campo de visión de la cámara |
| 0–0,34 | Tapa inferior con ventana de Ø12 mm en el centro |
| 0,36–0,76 | Anillo de lastre. La cámara pasa por su hueco central |
| 0,1–1,41 | **MLX90640**, colgada bajo la bandeja de la batería, con la lente mirando por la ventana |
| 1,5 | Bandeja de la batería: 4 × AAA en dos portapilas, accesibles por la ventana lateral (+z, `y` 1,45–5,35) |
| ~4,8 | Interruptores PWR y TX en la ventana lateral |
| 5,45 | Feather RP2040 Adalogger |
| 6,6 | BMP390 y TMP117 (taladros de ventilación en `y` 6,6–9,0) |
| 8,0 | SHT45 y BNO085 |
| 9,4 | GNSS MAX-M10S y zumbador |
| 10,55 | **Radio RFM96W**, con el coaxial bajando por el lateral hasta la antena |
| 11,16–11,5 | Tapa superior, cáncamo y parche GNSS. Encima, el paracaídas plegado (30 mm) |

La cámara mide 16 mm de alto, por eso la radio subió arriba y el lastre pasó de disco a anillo.

---

## 5. Arquitectura de `cansat.html`

Un único archivo con un `<style>`, el HTML de la interfaz y un `<script>` en una IIFE con `"use strict"`. Usa **three.js r147** (`three.min.js` y `examples/js/controls/OrbitControls.js`, global `THREE`) desde jsDelivr. No hay módulos ni framework.

Para orientarte, busca estos comentarios de sección (`/* ---------- … ---------- */`). Los números de línea cambian; los nombres no.

| Sección | Qué contiene |
|---|---|
| `CONTENIDO` | **Datos editables**: `BOM`, `CATS`, `SEC`, `REQS` y el render de la ficha técnica |
| `MOTOR` | Renderer, escena, cámara, OrbitControls, entornos PMREM (estudio y cielo) y luces |
| `TEXTURAS` / `MATERIALES` | Texturas generadas en `<canvas>` y el objeto `M` de materiales |
| `CONSTRUCCIÓN` | Geometría del CanSat: llamadas a `body(...)`, cableado (`harness`), cotas y `ROUTES` |
| `RIG DESPLEGADO` | Paracaídas abierto: dosel deformable, cuerdas, *riser* y destorcedor (`updateCanopy`) |
| `ENTORNO DE VUELO` | Cielo, terreno (`fieldTex`), hierba, nubes, estelas y anillos de telemetría |
| `PERFIL DE VUELO` | Física simple del descenso (`buildProfile`), atmósfera ISA, gráficas y `PHASES` |
| `CÁMARA TÉRMICA SIMULADA` | Simulación de la MLX90640 (ver 5.5) |
| `ESTADO` | Estado global `S`, tweens de cámara, etiquetas con líneas guía, selección, rayos X y modos |
| `ACTUALIZACIÓN DE VUELO` | `updateFlight(dt,time)`: anima el vuelo, la cámara, el HUD, el paquete y la térmica |
| `BUCLE` / `INTERACCIÓN` | `frame()`, cotas, flujo y todos los *listeners* |
| Arranque | Modo inicial según `#hash`, parámetros de prueba `?still…` |

### 5.1 Datos (sección CONTENIDO)

- **`BOM`**: filas `[grupo, pieza, modelo, cantidad, masa_total_g, coste_total_€, enlace?]`. `AF` es el prefijo de Adafruit. `TOTAL_G` y `TOTAL_E` se calculan solos. **La masa total alimenta la física del descenso** (`MASS`) y el diámetro de paracaídas mostrado.
- **`CATS`**: una entrada por categoría con etiqueta en 3D y ficha:
  ```js
  clave:{n, name, short, status, anchorBody, anchor:[x,y,z], side:'L'|'R', fn, parts:[], req:[], check}
  ```
  - `anchorBody` es el id de un `body`. `anchor` está en coordenadas de la lata, en cm.
  - `side` indica el lado de pantalla de la etiqueta.
  - La clave debe coincidir con el `cat` de los `body` que pertenecen a esa categoría.
  - Categorías actuales: `estructura`, `sensores`, `placa`, `bateria`, `controles`, `comms`, `paracaidas`, `secundaria`, `termica` (9).
- **`SEC`**: categorías que se pintan en ámbar como misión secundaria.
- **`REQS`**: `[id, título, texto de las bases, estado en el diseño, 'ok'|'pend']`.

### 5.2 Geometría (sección CONSTRUCCIÓN)

- **Unidades:** 1 = 1 cm. La base de la lata está en `y=0` y el eje en `x=z=0`. La ventana de acceso mira a **+z**. Constantes: `R=3.3` (radio exterior), `RI=3.18`, `H=11.5`, `TRAY=3.05` (radio de bandeja), `RODS`, `TRAYS`, `WIN`, `VENT`, `ANT`.
- **`body(id, cat, path, win, builder)`** crea un grupo dentro de `canRoot`:
  - `path` es la lista de desplazamientos de despiece (se antepone `[0,0,0]`).
  - `win=[t0,t1]` es la ventana del parámetro de despiece `S.e` (0–1) en la que se mueve la pieza.
  - Las piezas se construyen **en coordenadas finales de la lata**, no locales.
  - Todas las mallas heredan `userData.cat`, que sirve para el clic, el resaltado y los rayos X.
- **Ayudantes:**
  - `box`, `cyl`, `mesh` y `tubeCurve` para cables.
  - `brd(g,w,d,x,y,z,label,…,trayY)`: placa con textura y separadores; devuelve la `y` de su cara superior.
  - `addChip`, `qt` (conector STEMMA QT), `headerRow`, `tray(g,y)`.
  - `tubeSeg`, `annSector`, `edgeQuad` para la carcasa con ventana.
  - `V2` + `LatheGeometry` para tapas y anillo. Los perfiles van **en sentido antihorario** en el plano (r, y) para que las normales salgan hacia fuera.
- **Resaltado:** al arrancar, cada material se clona por categoría en `catMats`. `select(id)` cambia su emisivo. `setXray` hace transparentes la carcasa y las tapas (`DoubleSide` o color de `M.cap`) y el paracaídas.
- **`harness`**: cables visibles solo con la lata montada.
- **`ROUTES`**: líneas de flujo animadas en Despiece, de la forma `{a:[bodyId,[x,y,z]], b:[…], c:color}`. Colores: cian = datos, coral = energía, ámbar = misión secundaria.

### 5.3 Modos y cámara

- `setMode('modelo'|'despiece'|'descenso')` gestiona las barras, el HUD, el panel térmico, el fundido y el entorno (`applyEnv`). El modo se refleja en `#hash`.
- `camTo(target, dir, dist, dur)` interpola la cámara en coordenadas esféricas y devuelve una promesa. `runTour()` encadena pasos con `caption()`.
- `layoutLabels` proyecta los anclajes a pantalla y apila las etiquetas por lado sin solaparlas.

### 5.4 Simulación del descenso

- `buildProfile()`:
  - Caída libre 2 s → apertura en 1,5 s → velocidad constante `fl.v`.
  - Viento de ejemplo `windAt(h)` (ley potencial) que da la deriva `d`.
  - Calcula el diámetro de paracaídas con Cd 0,75 y coloca el blanco térmico (`TH.tgt.x = deriva − 18 m`).
- **Escala del vuelo:** 6 unidades de escena = 1 m. El CanSat se queda en x=0 y **es el suelo el que se mueve** (`ground.position.x = −d·6`, `gy = −1,25 − alt·6`).
- P y T salen de la atmósfera ISA. HR y viento son perfiles de ejemplo. El HUD lo indica: **no inventes datos de vuelo**.
- Paquete simulado: `AB1,seg,P,T,HR,deriva,Tmáx`.

### 5.5 Cámara térmica simulada (MLX90640)

- **Sensor:** 32 × 24 píxeles, campo de 55° × 35°, `ifov` de unos 1,72°/píxel, unos 8 fotogramas/s (redibujado cada 0,125 s).
- **Terreno térmico:** se genera a partir de la **misma textura `fieldTex`** que se ve en 3D, reducida a 1024² (1 píxel ≈ 1,95 m; la tesela mide 2 km y se repite):
  - T ≈ 44 − 140·(G−R)/(G+R), de modo que lo más verde es más fresco.
  - Las zonas claras, como los caminos, suman +3 °C.
  - Se guarda como **tabla de sumas acumuladas periódica** (`TH.S`, `thP`).
- `groundT(X,Z,s)` da la temperatura media de un cuadrado de lado `s`. Así cada píxel **promedia su huella** (píxel mixto), igual que un sensor real.
- La lona de 65 °C se mezcla de forma analítica según la fracción de área que cubre.
- **Simplificación:** se mezcla la temperatura de forma lineal, no la radiancia.
- **Mapeo entre terreno y textura:** `col = (0,5 + X/2000)·N`, `fila = (0,5 + Z/2000)·N`. X y Z están en metros respecto al origen del suelo; el CanSat está en `X = d`, `Z = 0`.
- `thermalFrame(alt, d, quaternion)` lanza un rayo por píxel con la actitud real del rig (`flightRig.quaternion`). Los rayos por encima del horizonte devuelven −10 °C (cielo).
- `updateThermal(...)` dibuja el panel:
  - Paleta tipo *inferno* (`THLUT`), escala fija 20–65 °C y modo «Radio 8×6» (bloques de 4×4).
  - Detección: máximo por encima de 55 °C, con histéresis a 53 °C.
  - Marca «foco» cuando detecta. Si no, dibuja un círculo discontinuo «lona» en la posición prevista.
  - Mensajes de espera, descarte por giro (t < 3,5 s) y modo centinela.
- En 3D, `tarp` es la lona en el suelo y `thFr` dibuja las líneas del campo de visión hasta el suelo.

---

## 6. Recetas habituales

- **Cambiar un componente:** edita su fila en `BOM`, el texto en `CATS[...].parts` y, si cambia el tamaño, su `body(...)`. Revisa los textos con números fijos (ver más abajo).
- **Añadir una pieza nueva al 3D:**
  1. `body('id','categoria',[[dx,dy,dz]],[t0,t1],g=>{…})` con coordenadas finales en cm. Comprueba choques con `RODS` (radio 0,15) y con el radio interior 3,18.
  2. Opcional: cable en `harness`, ruta en `ROUTES` y paso en `runTour`.
- **Añadir una categoría:** entrada nueva en `CATS` con `n` correlativo. Si es de la misión secundaria, añádela a `SEC`. El contador «n / total» se ajusta solo.
- **Cambiar la misión:** textos en la ficha técnica (`<aside id="req">`), `CATS.secundaria`, `CATS.termica`, `PHASES`, `runTour`, el rótulo de la lata (`decalTex`) y el aviso del HUD.

**Textos con números escritos a mano** que hay que revisar si cambia el diseño:
- R1: «30 mm arriba + 12 mm de antena abajo = 42 mm».
- R4 y la batería: «unas 6 h», «105 mA».
- Comunicaciones: «a 17 mm del eje».
- Paracaídas: «ocupa 30 mm».
- Cámara térmica: «unos 70 €» y la fecha de «agotada».
- Ficha de la misión: «350.000 ha».
- Ficha de la misión y ficha de la cámara: «30 m a 1000 m».

La masa total (`TOTAL_G`) y el coste (`TOTAL_E`) sí son dinámicos.

---

## 7. Cómo probar los cambios

**1. Sintaxis del JS** (desde la raíz del repo, en Git Bash):
```bash
awk '/^<script>$/{f=1;next}/^<\/script>$/{f=0}f' cansat.html > /tmp/main.js && node --check /tmp/main.js
```

**2. Capturas con Chrome sin interfaz** (WebGL por software):
```bash
CH="/c/Program Files/Google/Chrome/Application/chrome.exe"
URL="file:///C:/ruta/al/repo/cansat.html"
"$CH" --headless=new --use-angle=swiftshader --enable-unsafe-swiftshader --hide-scrollbars \
  --window-size=1400,860 --virtual-time-budget=10000 --user-data-dir="$TMP/chr" \
  --screenshot="$TMP/shot.png" "$URL?still&t=90#descenso"
```
- Con `--dump-dom` en lugar de `--screenshot` se puede leer el DOM ya renderizado, por ejemplo para comprobar los totales de `#bomFoot`.
- Chrome sin interfaz no reduce de verdad la ventana por debajo de unos 500 px. Para ver el diseño móvil (≤ 760 px), usa por ejemplo `--window-size=560,900`.

**3. Parámetros de URL para pruebas** (`?still` congela la escena sin animaciones de entrada):

| Parámetro | Efecto |
|---|---|
| `#modelo` / `#despiece` / `#descenso` | Modo inicial |
| `?still&t=90#descenso` | Vuelo en T+90 s, con la térmica ya calculada |
| `?still&xray` | Rayos X |
| `?still&sel=termica` | Abre la ficha de una categoría |
| `?still&req` | Abre la ficha técnica |
| `?still&zoom=30&dir=[0.45,-0.8,0.55]&ty=0.4#modelo` | Cámara libre. `dir` debe ser JSON válido (`0.45`, no `.45`) y codificado en la URL |
| `?autoplay#descenso` | Lanza el vuelo al cargar |

---

## 8. Convenciones

- **Idioma:** todo el texto visible está en español de España, con tono técnico, frases cortas y unidades del SI con coma decimal (`fmtN`). Cita los requisitos como `<span class="ref">R7</span>` o `§3.2`.
- **Colores** (variables en `:root`):

  | Variable | Uso |
  |---|---|
  | `--cyan` | Datos y selección |
  | `--coral` | Energía |
  | `--amber` | Misión secundaria |
  | `--orange` | Paracaídas |
  | `--ok` | Cumplido |

  Solo hay tema oscuro (`color-scheme: dark`).
- **Honestidad de los datos:** todo lo simulado se etiqueta como «SIMULACIÓN» o «ejemplo». No presentes estimaciones como medidas.
- `[hidden]{display:none!important}` es necesario: `#err` y `.bar` declaran `display:flex`, que si no anularía el atributo `hidden`.
- Accesibilidad:
  - Botones reales con `aria-pressed` y `aria-selected`.
  - `aria-label` en los lienzos.
  - Se respeta `prefers-reduced-motion`.
  - La tecla Escape cierra los paneles.
- Si se cambia de versión de three.js, fíjala a una versión exacta. r147 es la última con `examples/js` (globales). Las versiones posteriores requieren módulos ES.

---

## 9. Pendiente y siguientes pasos

- [ ] **PDR antes del 18 de diciembre de 2026**: objetivos, plan inicial de misión y presupuesto inicial. Plantilla: https://esero.es/wp-content/uploads/2026/01/2026_PDR_CanSat.pdf
- [ ] Confirmar la disponibilidad de la MLX90640 55° (o la versión de 110°) en otro distribuidor.
- [ ] Prototipo en protoboard: lectura de la MLX90640 en el RP2040 por el 2.º bus I²C, registro en microSD y paquete LoRa con miniatura de 8 × 6.
- [ ] Firmware: detección de liberación y aterrizaje, modo centinela, telecomandos, ID `AB1` y CRC (R12).
- [ ] Cálculo a bordo de la HCFM y la probabilidad de ignición: conseguir las tablas oficiales.
- [ ] Estación de tierra: recepción LoRa, mapa, perfil y vista térmica en directo. Queda fuera del presupuesto de 500 €.
- [ ] Ensayos:
  - Descarga de 4 h.
  - Tracción de 50 N con destorcedor.
  - Caída para medir velocidad y giro.
  - Vibración (20 g).
  - Emisividad frente a termómetro IR.
- [ ] Pesar el conjunto real y ajustar el lastre (R2). Confirmar la frecuencia asignada (R10).
- [ ] Ideas para el modelo:
  - La simulación térmica no incluye la hierba que rodea el punto de liberación.
  - La mezcla es lineal en temperatura.
  - El modo centinela solo se anuncia; no se simula.

---

## 10. Historia

- `f3b086e`: modelo 3D inicial con una misión de ejemplo, «perfil de viento y humedad».
- Rediseño de la misión secundaria a **VIGÍA**:
  - Componentes nuevos: cámara térmica, BNO085 y destorcedor.
  - Nueva distribución interna, con la radio arriba, la cámara abajo y el lastre en anillo.
  - Panel de cámara térmica simulada en la pestaña Descenso.
  - Corrección de la regla `[hidden]`.
