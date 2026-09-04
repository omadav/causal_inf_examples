# Configurar el Google Form (backend de datos)

Las páginas en `docs/module-a.html`, `module-b.html` y `module-c.html` son
autónomas (HTML/CSS/JS puro, sin librerías) y quedan públicas al activar
GitHub Pages — ningún estudiante necesita iniciar sesión en nada. Al enviar
sus respuestas, cada página hace un POST silencioso (`fetch` en modo
`no-cors`) directamente al endpoint `formResponse` de **un único Google
Form compartido por los 3 módulos**, que las guarda en una Hoja de cálculo.
Tú lees esa hoja para el análisis en vivo.

Cada módulo empaqueta todas sus respuestas en un solo campo de texto
(`payload_json`), así que el formulario entero tiene **solo 2 preguntas**
— nada de crear 8 campos por módulo.

## Pasos (una sola vez, ~2 minutos)

1. Ve a [forms.google.com](https://forms.google.com) → formulario en blanco.
2. Título: `Respuestas — experimentos de inferencia causal`.
3. Desactiva "Recopilar direcciones de correo electrónico" (⚙️ arriba a la
   derecha → Respuestas).
4. Agrega **2 preguntas**, ambas de tipo "Respuesta corta", ninguna
   obligatoria, en este orden:
   1. `modulo`
   2. `payload_json`
5. Menú ⋮ (arriba a la derecha) → **"Obtener enlace con respuestas
   prellenadas"**.
6. En la vista previa, escribe cualquier valor de prueba en ambos campos
   (por ejemplo `A` y `test`) y pulsa **"Obtener enlace"**. Copia esa URL
   larga.
7. Pégamela — extraigo de ahí la URL de acción (`.../formResponse`) y los
   dos `entry.NNNNNNNN`, y actualizo `FORM_CONFIG` en las 3 páginas.
8. Pestaña **Respuestas** → ícono verde de Sheets → "Crear hoja de cálculo
   nueva". Ahí llegará cada envío en vivo, con columnas `Marca temporal`,
   `modulo`, `payload_json`.

## Probar antes de la clase

Abre cada una de las 3 páginas, complétala como si fueras un estudiante y
revisa que aparezca una fila nueva en la Hoja de cálculo (con `modulo` =
A, B o C según la página, y `payload_json` con el resto de las respuestas
codificadas). Si `FORM_CONFIG.action` todavía dice
`REEMPLAZA_CON_TU_FORM_ACTION_URL`, la página funciona pero **no envía
nada** — solo lo avisa por consola (`console.warn`), útil para probar el
flujo visual antes de tener el formulario listo.

## Activar GitHub Pages (ya hecho ✅)

Ya está activo en `https://omadav.github.io/causal_inf_examples/`, sirviendo
desde la rama `claude/causal-inference-experiments-pwnp54`, carpeta `/docs`.
