# OpenAI

Cómo se ejecuta cada intención con la API de OpenAI. Todo es "así se hace en OpenAI", no una obligación del agente.

Lo de aquí sale de `developers.openai.com`, consultado el 2026-10-17. Lo que no se pudo confirmar va marcado **sin verificar**. OpenAI retira modelos seguido: antes de un trabajo largo, revisa la página de modelos y la de retiros (enlaces en Fuentes).

**En corto:** OpenAI aquí es imagen (generar y editar, muy buena con texto legible e instrucciones largas) y voz en off. **No hay video**: la API de video (Sora 2) se apagó el 2026-09-24 sin reemplazo.

## Detección

| Vía | Cómo se detecta |
|---|---|
| MCP | Herramientas que casan con `mcp__.*([Oo]pen[Aa][Ii]\|gpt[-_]?image).*__`. **No se encontró un MCP oficial de OpenAI para imágenes**; los que existen son de terceros y sus nombres de herramienta varían (`generate_image`, `edit_image`…). Si aparece uno, léele el esquema antes de usarlo |
| CLI | No hay CLI oficial de imágenes. Se usa `curl` (y `jq` para sacar la imagen) |
| Llave | `[ -n "$OPENAI_API_KEY" ] && echo sí \|\| echo no`. Es el nombre oficial |

## Llamar sin enseñar la llave

La llave va **solo** dentro del header, leída de la variable. Nunca `echo $OPENAI_API_KEY`, nunca `curl -v` ni `--trace`, nunca `set -x`, nunca en un archivo ni en `prompts.md`. Rutas absolutas siempre. **El prompt nunca va pegado en el comando**: se escribe a un archivo con heredoc entre comillas y se lee desde ahí (ver "Llamadas seguras" en el `SKILL.md` de esta skill).

**Prompt a archivo** (para las dos llamadas de abajo):

```bash
cat > /ruta/abs/trabajo/<carpeta>/prompt.txt <<'EOF'
…el prompt, tal cual…
EOF
```

**Generar** (`imagen.generar`): JSON armado con `jq`, enviado desde archivo.

```bash
jq -n --arg p "$(cat /ruta/abs/trabajo/<carpeta>/prompt.txt)" \
  '{model: "gpt-image-2.5-flare", prompt: $p, size: "1024x1536", quality: "high"}' \
  > /ruta/abs/trabajo/<carpeta>/peticion.json

curl -sS -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d @/ruta/abs/trabajo/<carpeta>/peticion.json \
  > /ruta/abs/trabajo/<carpeta>/respuesta.json
```

**Editar con referencias** (`imagen.editar`, producto real) — multipart, una línea `image[]` por referencia. El prompt va con `--form-string`, no con `-F` (que interpreta `@` y `<` dentro del valor):

```bash
curl -sS -X POST "https://api.openai.com/v1/images/edits" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F "model=gpt-image-2.5-sunburst" \
  -F "image[]=@/ruta/abs/entrada/producto.png" \
  -F "image[]=@/ruta/abs/entrada/escena-referencia.png" \
  --form-string "prompt=$(cat /ruta/abs/trabajo/<carpeta>/prompt.txt)" \
  > /ruta/abs/trabajo/<carpeta>/respuesta.json
```

**Leer la respuesta** sin hacer `cat` del JSON (trae la imagen en base64, enorme):

```bash
# ¿Falló? Solo el mensaje
jq -r '.error.message // empty' /ruta/abs/trabajo/<carpeta>/respuesta.json

# La imagen, directo a archivo
jq -r '.data[0].b64_json' /ruta/abs/trabajo/<carpeta>/respuesta.json \
  | base64 --decode > /ruta/abs/trabajo/<carpeta>/imagen-01.png
```

La alternativa es la Responses API con la herramienta integrada `{"type": "image_generation", "model": "…"}`; el resultado llega en un `image_generation_call` con la imagen en `result`. Para trabajo de estudio, la Image API de arriba es más directa.

## Modelos

| Modelo | Para qué |
|---|---|
| `gpt-image-2.5-sunburst` | El más capaz. Edición donde importa la precisión: producto real, cambios finos |
| `gpt-image-2.5-flare` | Rápido y de buena calidad para generar a diario |
| `gpt-image-2` | Sigue disponible (aparece como reemplazo recomendado, sin fecha de retiro) |

Retirados o por retirar: `dall-e-2` y `dall-e-3` (apagados 2026-05-12), `gpt-image-1` (2026-10-23), `gpt-image-1-mini`, `gpt-image-1.5` y `chatgpt-image-latest` (2026-12-01). No los uses en trabajo nuevo.

Parámetros que importan:

- `size`: `1024x1024`, `1536x1024`, `1024x1536`, o `ANCHOxALTO` personalizado con lados múltiplos de 16. Límites: lados ≤ 3840 px, proporción entre 1:3 y 3:1, total entre 655 360 y 8 294 400 píxeles; por encima de 2560x1440 es experimental.
- `quality`: `low`, `medium`, `high`, `xhigh`, `max` o `auto`. `xhigh` y `max` solo en los 2.5.
- `background: "transparent"` con `output_format` `png` o `webp`.
- `output_format`: `png` (defecto), `jpeg`, `webp`; `output_compression` 0-100 para jpeg/webp.
- `n`: cuántas imágenes; por defecto 1.
- `mask` (solo en edits): PNG con canal alfa, mismo formato y tamaño que la imagen, archivo < 50 MB. Se aplica a la **primera** imagen de `image[]`. El enmascarado es guiado por prompt: no sigue la forma exacta.
- `input_fidelity`: para `gpt-image-2` se omite.
- `moderation`: `auto` (defecto) o `low`. Por defecto `auto`. `low` no se usa para saltarse las reglas del agente (consentimiento, menores, contenido sexual con personas reales, figuras públicas): esas no cambian con el filtro.
- Puede tardar hasta 2 minutos en prompts complejos.

## Tabla intención → herramienta

| Intención | Cómo | Notas |
|---|---|---|
| `media.subir` | No hay subida previa: el archivo va en la misma llamada (`image[]=@ruta`) | Rutas absolutas |
| `imagen.generar` | `POST /v1/images/generations` | Fuerte en texto legible dentro de la imagen y en seguir instrucciones largas |
| `imagen.editar` | `POST /v1/images/edits` con `image[]` (varias) | `gpt-image-2.5-sunburst`. Número máximo de referencias: sin verificar (la doc enseña 4). Compara contra el producto real |
| `imagen.quitar_fondo` | Aproximado: edits con `background: "transparent"` y prompt "mismo objeto, sin fondo" | **Regenera** el objeto, no lo recorta: puede cambiar etiqueta o forma. Para producto real, deriva a Magnific o Higgsfield |
| `imagen.expandir` | Aproximado: edits con la imagen en un lienzo más grande y `mask` sobre lo nuevo | Sin verificar como flujo documentado. Mejor Higgsfield o Magnific |
| `imagen.escalar` | **No soportado** | Deriva a Magnific o Higgsfield |
| `imagen.reiluminar` | Aproximado: edits con la luz nueva en el prompt | Regenera; riesgo para la fidelidad del producto. Mejor Magnific |
| `video.desde_imagen`, `video.desde_texto`, `video.copiar_movimiento`, `video.reemplazar_objeto`, `video.reencuadrar`, `video.escalar` | **No soportado.** La API de video (Sora 2) se apagó el 2026-09-24 | Deriva a Higgsfield (o Magnific para generar) |
| `identidad.entrenada` | **No soportado** | Deriva a Higgsfield |
| `identidad.referencia` | No hay referencia guardada: se pasa la imagen del personaje en `image[]` en cada llamada | La propia doc avisa que los personajes recurrentes pueden no salir consistentes. Para serie larga, Higgsfield |
| `voz.crear` | Voces personalizadas con grabación de consentimiento del hablante + muestra | Requisitos y acceso: sin verificar (guía "Custom voices") |
| `voz.hablar` | `POST /v1/audio/speech` con `gpt-4o-mini-tts` y `voice` | Voces: `alloy`, `ash`, `ballad`, `coral`, `echo`, `fable`, `nova`, `onyx`, `sage`, `shimmer`, `verse`, `marin`, `cedar` (recomendadas `marin` y `cedar`). Optimizadas para inglés: prueba el español antes de una serie. Formatos mp3, opus, aac, flac, wav, pcm |
| `voz.doblar` | **No soportado** | Deriva a Higgsfield |
| `video.ugc`, `analisis.video`, `analisis.prediccion` | **No soportado** | Deriva a Higgsfield |
| `costo.consultar` | **No hay** endpoint de costo previo | Ver Costo informativo |

## En `marca/avatar.json`

Clave de proveedor: `"openai"`, en minúsculas. No hay identidad guardada en el servidor ni entrenamiento: solo cabe `referencia`, y su `id` es la ruta (relativa a la carpeta de trabajo) de la imagen del personaje que se manda en cada `image[]`.

```json
{
  "identidades": {
    "openai": {
      "referencia": {
        "id": "marca/avatar/ancla.png",
        "nombreEnProveedor": "imagen de referencia en image[]",
        "modelosCompatibles": ["gpt-image-2.5-sunburst", "gpt-image-2.5-flare", "gpt-image-2"],
        "estado": "lista",
        "creadoEn": "2026-10-17T12:00:00Z"
      }
    }
  },
  "voces": {
    "openai": { "id": "marin", "tipo": "preset" }
  }
}
```

- `identidades.openai.entrenada`: no existe (no hay `identidad.entrenada`).
- `voces.openai.id`: el nombre de la voz (`marin`, por ejemplo) o el id de la voz personalizada si se creó. OpenAI solo pide un campo `voice`; `tipo` es nota del plugin (`preset` o `propia`), no se manda. Formato exacto para voces personalizadas: sin verificar.

## Capacidades especiales

| # | Pregunta | En OpenAI |
|---|---|---|
| 1 | ¿Flujo guiado de fotografía de producto? | **No.** Se hace con edits + las recetas de `producto` |
| 2 | ¿Reemplazo de objeto en video? | **No** (no hay video) |
| 3 | ¿Plantillas o presets de movimiento? | **No** |
| 4 | Proporciones y límites | No hay `video.reencuadrar`. Imagen: tamaños fijos o `ANCHOxALTO` múltiplos de 16, así que 4:5 se pide directo (p. ej. `1024x1280`). `imagen.expandir` dedicado no hay |
| 5 | ¿`imagen.escalar` pide ancho/alto de origen? | No aplica: no hay escalado |
| 6 | ¿Ruta de copiar movimiento mantenida? | Ninguna |
| 7 | ¿Flujo guiado de UGC y de hoja de personaje? | **No.** La hoja de personaje se puede pedir como una imagen con varias vistas, sin flujo guiado |
| 8 | ¿Flujo para multiplicar un video propio? | **No** |

## Costo informativo

- Se cobra por uso a la cuenta de la API (tokens de imagen según tamaño y calidad). No hay consulta de costo sin generar.
- No inventes un precio. Si la persona quiere el número, que lo vea en su panel de uso de OpenAI.
- Lo que más mueve el costo: `quality` (`xhigh` y `max` son las caras), `size` y `n`.

## Particularidades

- La respuesta trae la imagen en base64 (`data[0].b64_json`), no una URL. Guárdala a archivo directo, sin imprimirla en la pantalla.
- Varias imágenes de golpe: `n` en una llamada, o llamadas en paralelo. Consejo de eficiencia: pocas, revisa, y pide más de la que funcionó.
- Texto dentro de la imagen: es de los mejores, pero aun así la doc avisa que la colocación precisa falla. Si la pieza lleva texto encima, la regla de oficio sigue: imagen limpia.

## Fuentes

- https://developers.openai.com/api/docs/guides/image-generation — modelos 2.5, endpoints, parámetros, ejemplos curl, limitaciones. Consultado 2026-10-17.
- https://developers.openai.com/api/docs/models — modelos de imagen listados. Consultado 2026-10-17.
- https://developers.openai.com/api/docs/deprecations — retiros de DALL·E, gpt-image-1, 1.5, 1-mini, chatgpt-image-latest y Sora 2 / Videos API. Consultado 2026-10-17.
- https://developers.openai.com/api/docs/guides/video-generation — aviso de apagado de Sora 2 (2026-09-24). Consultado 2026-10-17.
- https://developers.openai.com/api/docs/guides/text-to-speech — modelos y voces de TTS, voces personalizadas. Consultado 2026-10-17.
- Búsqueda de MCP oficial (resultados solo de terceros en glama.ai y GitHub) — consultado 2026-10-17.

**Sin verificar:** MCP oficial (no encontrado); máximo de imágenes en edits; expandir y quitar fondo como flujos documentados; requisitos de voces personalizadas y cómo se referencian.
