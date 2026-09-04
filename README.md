# Experimentos de inferencia causal en vivo

Tres demos para una clase de inferencia causal, pensadas para correrse en
vivo: los estudiantes responden desde su celular (una URL/QR por módulo) y
los datos se analizan en clase, en el momento.

| Módulo | Qué muestra | Diseño |
|---|---|---|
| **A — El precio arbitrario** | Réplica del "coherent arbitrariness" de Ariely et al. (2003) con los dos últimos dígitos del RUT en vez del SSN. Correlación (no asignación aleatoria) entre el dígito y la disposición a pagar. | Observacional |
| **B — El ancla numérica** | Réplica del anclaje de Strack & Mussweiler (1997, "edad de Gandhi"), con 5 personajes, ancla alta/baja **asignada al azar** por ítem y orden de presentación aleatorio, para ver cómo decae el efecto con la repetición. | Experimento aleatorizado, panel 5×N |
| **C — El dilema y tú** | Variante del tranvía (palanca vs. empujar) asignada al azar; heterogeneidad del efecto por género, religiosidad y personalidad (TIPI-10). | Experimento aleatorizado + heterogeneidad (CATE) |

El contraste pedagógico central: A es una variable "tan buena como
aleatoria" pero no diseñada por nadie; B y C son asignación aleatoria real.
Comparar el balance de covariables entre A y B en clase es el punto de
partida para hablar de qué garantiza — y qué no — cada uno.

## Estructura

```
docs/
  index.html                 landing con los 3 módulos
  module-a.html               Módulo A (RUT / anclaje observacional)
  module-b.html               Módulo B (ancla numérica, 5 ítems)
  module-c.html               Módulo C (tranvía + heterogeneidad)
  GOOGLE_FORMS_SETUP.md       cómo cablear el backend de datos
analysis/
  session.ipynb               notebook para correr en clase (balance, ATE, CATE)
  requirements.txt            pip install -r analysis/requirements.txt
  data/                       module_a.csv, module_b.csv, module_c.csv (exportar de Sheets antes de la clase)
```

`session.ipynb` corre hoy mismo con datos simulados (`SIMULATE = True`,
marcados como tales en la salida) para probar el flujo antes de la clase
real; apenas existan los 3 CSV en `analysis/data/` usa esos automáticamente.

Las páginas son HTML/CSS/JS puro (sin frameworks), pensadas para
**GitHub Pages**: públicas, sin login, sirven desde `/docs` en esta rama.
Cada envío hace un POST silencioso a un Google Form (ver
`docs/GOOGLE_FORMS_SETUP.md`), que registra las respuestas en una Hoja de
cálculo para leer en vivo durante la clase.

## Estado

- [x] Las 3 páginas (diseño, aleatorización, validaciones)
- [x] Activar GitHub Pages sobre esta rama, carpeta `/docs`
- [x] Notebook de análisis en vivo (balance de covariables, ATE, CATE) — probado con datos simulados
- [ ] Crear los 3 Google Forms y cablear `FORM_CONFIG` en cada página (bloqueante: sin esto no se guarda ninguna respuesta real)
