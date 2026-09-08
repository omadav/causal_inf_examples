# Experimentos de inferencia causal en vivo

Tres demos para una clase de inferencia causal, pensadas para correrse en
vivo: los estudiantes responden desde su celular (una URL/QR por módulo) y
los datos se analizan en clase, en el momento.

| Módulo | Qué muestra | Diseño |
|---|---|---|
| **A — Vender o comprar** | Efecto dotación (Kahneman, Knetsch & Thaler, 1990): cada estudiante es asignado al azar a "vendedor" (dueño, WTA) o "comprador" (no dueño, WTP) del mismo objeto. ATE = brecha WTA−WTP; heterogeneidad por **género**. | Experimento aleatorizado + heterogeneidad (CATE) |
| **B — El ancla numérica** | Réplica del anclaje de Strack & Mussweiler (1997, "edad de Gandhi"), con 5 personajes, ancla alta/baja **asignada al azar** por ítem y orden de presentación aleatorio, para ver cómo decae el efecto con la repetición. | Experimento aleatorizado, panel 5×N |
| **C — El dilema y tú** | Variante del tranvía (palanca vs. empujar) asignada al azar; heterogeneidad del efecto por género, religiosidad y personalidad (TIPI-10). | Experimento aleatorizado + heterogeneidad (CATE) |

Los 3 módulos son experimentos aleatorizados de verdad (cada uno con su
propia moneda al aire hecha por el sitio), pensados para reforzarse entre
sí más que para contrastar observacional-vs-experimental: mismo mecanismo
de asignación en los 3, aplicado a preguntas de investigación distintas —
y con A y C mostrando explícitamente que un mismo tratamiento puede tener
efectos heterogéneos según el subgrupo (género en A, género/religiosidad/
personalidad en C).

## Estructura

```
docs/
  index.html                 landing con los 3 módulos
  module-a.html               Módulo A (efecto dotación: vendedor/comprador)
  module-b.html               Módulo B (ancla numérica, 5 ítems)
  module-c.html               Módulo C (tranvía + heterogeneidad)
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
y, en paralelo, descarga una copia CSV local en el dispositivo del
estudiante como respaldo si el envío falla.

## Estado

- [x] Las 3 páginas (diseño, aleatorización, validaciones)
- [x] GitHub Pages activo sobre esta rama, carpeta `/docs`
- [x] Backend de datos (Web3Forms) probado de punta a punta con respuestas reales
- [x] Notebook de análisis en vivo (balance, ATE, CATE) — probado con datos simulados y con datos reales
- [ ] Correr con toda la clase (hasta ahora solo pruebas del profesor)
