# Configurar los 3 Google Forms (backend de datos)

Las páginas en `docs/module-a.html`, `module-b.html` y `module-c.html` son
autónomas (HTML/CSS/JS puro, sin librerías) y quedan públicas al activar
GitHub Pages — ningún estudiante necesita iniciar sesión en nada. Al enviar
sus respuestas, cada página hace un POST silencioso (`fetch` en modo
`no-cors`) directamente al endpoint `formResponse` de un Google Form, que
las guarda en una Hoja de cálculo vinculada. Tú lees esa hoja para el
análisis en vivo.

Como el estudiante nunca ve el formulario, **todas las preguntas pueden ser
de tipo "Respuesta corta"** — el tipo de campo no importa, solo el orden y
la cantidad. Esto mantiene la creación de cada formulario a menos de 10
preguntas.

## Pasos generales (repetir para los 3 módulos)

1. Ve a [forms.google.com](https://forms.google.com) → formulario en blanco.
2. Título: `Módulo A — respuestas` (o B / C). Desactiva "Recopilar
   direcciones de correo electrónico" (Configuración → Respuestas).
3. Agrega las preguntas de la tabla correspondiente abajo, **en ese orden**,
   todas como "Respuesta corta", ninguna obligatoria.
4. En el menú ⋮ (arriba a la derecha) → **"Obtener enlace con respuestas
   prellenadas"**.
5. Se abre una vista previa del formulario: escribe cualquier valor de
   prueba en cada campo (por ejemplo `test1`, `test2`...) y pulsa
   **"Obtener enlace"**. Copia esa URL larga — trae todos los
   `entry.NNNNNNNN` codificados.
6. Pégame esa URL (las 3, una por módulo) y yo mismo extraigo los
   `entry.NNNNNNNN` y la URL de acción, y actualizo `FORM_CONFIG` en cada
   página. (O hazlo tú mismo: la URL de acción es
   `https://docs.google.com/forms/d/e/<ID_DEL_FORM>/formResponse`, visible
   si cambias `viewform` por `formResponse` en la URL del formulario, y cada
   parámetro `entry.NNNNNNNN=valor_de_prueba` de la URL prellenada te dice
   qué entry.ID corresponde a qué pregunta, por el orden en que las
   creaste.)
7. En el formulario, pestaña **Respuestas** → ícono verde de Sheets →
   "Crear hoja de cálculo nueva". Ahí llegará cada envío en vivo.

## Módulo A — preguntas (en este orden)

1. rut_digitos
2. wtp_vino
3. wtp_audifonos
4. edad_rango
5. genero
6. religiosidad
7. practica_religion
8. submission_id
9. modulo

## Módulo B — preguntas (en este orden)

1. items_json — *(texto largo: usa "Respuesta corta" igual, Forms no
   trunca; si prefieres, usa "Párrafo" para más margen visual — es
   irrelevante ya que nunca se ve)*
2. edad_rango
3. genero
4. religiosidad
5. practica_religion
6. submission_id
7. modulo

## Módulo C — preguntas (en este orden)

1. variante
2. aceptabilidad
3. genero
4. religiosidad
5. practica_religion
6. tipi_json
7. submission_id
8. modulo

## Activar GitHub Pages (una sola vez)

1. En GitHub, entra al repo → **Settings → Pages**.
2. En "Build and deployment" → Source: **Deploy from a branch**.
3. Branch: `claude/causal-inference-experiments-pwnp54` (o la rama donde
   vivan estos archivos), carpeta **/docs**.
4. Guarda. En 1–2 minutos las páginas quedan disponibles en:
   - `https://<tu-usuario-github>.github.io/causal_inf_examples/module-a.html`
   - `https://<tu-usuario-github>.github.io/causal_inf_examples/module-b.html`
   - `https://<tu-usuario-github>.github.io/causal_inf_examples/module-c.html`

## Probar antes de la clase

Abre cada página, complétala como si fueras un estudiante y revisa que la
fila aparezca en la Hoja de cálculo del formulario correspondiente. Si
`FORM_CONFIG.action` todavía dice `REEMPLAZA_CON_TU_FORM_ACTION_URL`, la
página funciona pero **no envía nada** — solo lo avisa por consola
(`console.warn`), útil para probar el flujo visual sin ensuciar datos
reales antes de tener los formularios listos.
