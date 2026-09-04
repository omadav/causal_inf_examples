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
analysis/                     notebooks para la sesión en vivo (pendiente)
```

Las páginas son HTML/CSS/JS puro (sin frameworks), pensadas para
**GitHub Pages**: públicas, sin login, sirven desde `/docs` en esta rama.
Cada envío hace un POST silencioso a un Google Form (ver
`docs/GOOGLE_FORMS_SETUP.md`), que registra las respuestas en una Hoja de
cálculo para leer en vivo durante la clase.

## Estado

- [x] Las 3 páginas (diseño, aleatorización, validaciones)
- [ ] Crear los 3 Google Forms y cablear `FORM_CONFIG` en cada página
- [ ] Activar GitHub Pages sobre esta rama, carpeta `/docs`
- [ ] Notebook de análisis en vivo (balance de covariables, ATE, CATE)
