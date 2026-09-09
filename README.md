# Experimentos de inferencia causal en vivo

Cuatro demos para una clase de inferencia causal, pensadas para correrse en
vivo: los estudiantes responden desde su celular (una URL/QR por módulo) y
los datos se analizan en clase, en el momento.

| Módulo | Qué muestra | Diseño |
|---|---|---|
| **A — Vender o comprar** | Efecto dotación (Kahneman, Knetsch & Thaler, 1990): cada estudiante es asignado al azar a "vendedor" (dueño, WTA) o "comprador" (no dueño, WTP) del mismo objeto. ATE = brecha WTA−WTP; heterogeneidad por **género**. | Experimento aleatorizado + heterogeneidad (CATE) |
| **B — Ganar o perder** | Efecto marco (Tversky & Kahneman, 1981, "Asian disease problem"): 5 dilemas de dominios distintos (salud, empleo, medio ambiente, inversión, educación), cada uno descrito en términos de **ganancia o pérdida** al azar, orden de presentación también aleatorio, para ver si el efecto decae con la repetición. | Experimento aleatorizado, panel 5×N |
| **C — El dilema y tú** | Variante del tranvía (palanca vs. empujar) asignada al azar; heterogeneidad del efecto por género, religiosidad y personalidad (TIPI-10). | Experimento aleatorizado + heterogeneidad (CATE) |
| **D — Aceptar o rechazar** | Juego del ultimátum (Güth, Schmittberger & Schwarze, 1982): oferta asignada al azar (justa 5/5 vs. injusta 2/8 sobre 10 lucas); ¿la rechazan aunque eso signifique quedarse sin nada? | Experimento aleatorizado + heterogeneidad (CATE) |

Los 4 módulos son experimentos aleatorizados de verdad (cada uno con su
propia moneda al aire hecha por el sitio), aplicados a preguntas de
investigación distintas — y A, C y D muestran explícitamente que un mismo
tratamiento puede tener efectos heterogéneos según el subgrupo (género en
A y D, género/religiosidad/personalidad en C).

**Un solo link:** `docs/flow.html` encadena los 4 módulos en un único
flujo secuencial (A → B → C → D) y pide las preguntas demográficas una
sola vez, al final, en vez de 4 veces. Es la opción recomendada para
proyectar en clase. Por debajo manda **un solo envío** a Web3Forms con los
4 módulos como campos separados (`payload_a_json` … `payload_d_json`,
`modulo: 'FLOW'`) — probamos primero mandar 4 POSTs independientes como
hacen las páginas individuales, pero el filtro antispam de Web3Forms
terminaba marcando los últimos como spam (la API igual respondía
`success: true`, y el dato se perdía en silencio). Un solo envío por
persona evita ese problema de raíz. `session.ipynb` sabe leer ambos
formatos (ver `docs/DATA_BACKEND.md`), así que el CSV exportado funciona
igual sin importar si una respuesta vino de `flow.html` o de una página
individual. Las 4 páginas individuales (`module-a.html` … `module-d.html`)
siguen disponibles como alternativa/respaldo.

## Estructura

```
docs/
  index.html                 landing: link único (flow.html) + los 4 módulos por separado
  flow.html                   los 4 módulos en un solo flujo secuencial (recomendado)
  module-a.html               Módulo A (efecto dotación: vendedor/comprador)
  module-b.html               Módulo B (efecto marco: 5 dilemas ganancia/pérdida)
  module-c.html               Módulo C (tranvía + heterogeneidad)
  module-d.html                Módulo D (juego del ultimátum)
  DATA_BACKEND.md             cómo funciona el backend de datos (Web3Forms)
analysis/
  session.ipynb               notebook para correr en clase (balance, ATE, CATE)
  requirements.txt            pip install -r analysis/requirements.txt
  data/                       responses.csv (exportado de Web3Forms antes de la clase; nunca se commitea)
```

`session.ipynb` corre hoy mismo con datos simulados (`SIMULATE = True`,
marcados como tales en la salida) para probar el flujo antes de la clase
real; apenas exista `analysis/data/responses.csv` usa esos datos reales
automáticamente, y cada sección se salta con un aviso si a ese módulo
todavía le faltan respuestas.

Las páginas son HTML/CSS/JS puro (sin frameworks), pensadas para
**GitHub Pages**: públicas, sin login, sirven desde `/docs` en esta rama.
Cada envío manda un POST en JSON a Web3Forms (ver `docs/DATA_BACKEND.md`)
y, si ese envío falla, descarga una copia CSV local en el dispositivo del
estudiante como respaldo.

## Estado

- [x] Las 4 páginas (diseño, aleatorización, validaciones)
- [x] GitHub Pages activo sobre esta rama, carpeta `/docs`
- [x] Backend de datos (Web3Forms) probado de punta a punta con respuestas reales
- [x] Notebook de análisis en vivo (balance, ATE, CATE) — probado con datos simulados y con datos reales
- [x] Módulo D con su propia sección de análisis en `session.ipynb` (descriptivo, ATE, balance, heterogeneidad por género) — probado con datos simulados; todavía sin respuestas reales
- [ ] Correr con toda la clase
