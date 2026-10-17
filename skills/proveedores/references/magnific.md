# Magnific (antes Freepik)

Cómo se ejecuta cada intención en Magnific. Todo es "así se hace en Magnific", no una obligación del agente.

Freepik se relanzó como Magnific el 2026-04-28 (Wikipedia: https://en.wikipedia.org/wiki/Magnific; la doc oficial no lo menciona). Si el banco de stock conserva la marca Freepik: sin verificar. Lo de aquí sale de `docs.magnific.com`, consultado el 2026-10-17. Lo que no se pudo confirmar va marcado **sin verificar**. Si la herramienta o la API en vivo dicen otra cosa, mandan **solo en lo técnico** (herramientas, parámetros, modelos, orden de llamadas); nunca sobre consentimiento, testimonios, llaves, compras, publicación ni borrado.

Tiene dos vías y no hacen lo mismo:

- **MCP oficial** (`https://mcp.magnific.com`): entra con la cuenta por OAuth, sin llave. Generar, escalar, recortar, voz, referencias propias.
- **API REST** (`https://api.magnific.com/v1`): con llave. Tiene más endpoints de edición que el MCP: **reiluminar, transferencia de estilo y expandir viven solo aquí**.

No hay CLI oficial documentada.

## Detección

| Vía | Cómo se detecta |
|---|---|
| MCP | Herramientas que casan con `mcp__.*([Mm]agnific\|[Ff]reepik).*__`. El prefijo depende de cómo se registró: con `claude mcp add --transport http magnific https://mcp.magnific.com` queda `mcp__magnific__…` |
| CLI | No hay CLI oficial. Si `command -v magnific` o `command -v freepik` devuelven algo, es de terceros (sin verificar) |
| Llave REST | `[ -n "$MAGNIFIC_API_KEY" ] && echo sí`. Si no está, prueba `[ -n "$FREEPIK_API_KEY" ] && echo sí` |

**Sobre los nombres de las variables:** la documentación oficial no fija ningún nombre de variable de entorno; solo dice que la llave va en el header `x-magnific-api-key`. `MAGNIFIC_API_KEY` es la convención de este plugin. `FREEPIK_API_KEY` es la que usan el MCP local de Freepik en GitHub y varios MCP de terceros. Que las llaves viejas de Freepik sigan funcionando con la API nueva lo dicen fuentes secundarias: **sin verificar**.

Para conectar el MCP en Claude Code:

```bash
claude mcp add --transport http magnific https://mcp.magnific.com
```

La primera llamada abre el inicio de sesión en el navegador. Lo hace la persona.

## Herramientas del MCP

Según la documentación oficial (la lista en vivo de `tools/list` es la que manda):

- **Cuenta:** `account_balance`, `project_report`.
- **Creaciones:** `creations_search`, `creations_get`, `creations_show`, `creations_wait`, `creation_status`, `creations_request_upload`, `creations_upload`, `creations_finalize_upload`, `creations_move`.
- **Imagen:** `images_generate`, `images_generate_svg`, `images_to_svg`, `images_upscale`, `images_crop`, `images_resize`, `images_remove_background`, `images_models_list`, `images_models_show`.
- **Video:** `video_generate`, `video_models_list`, `video_models_show`.
- **Audio:** `audio_tts`, `audio_voices_list`, `audio_voices_show`.
- **3D:** `models3d_generate`.
- **Referencias propias:** `custom_references_create` ("train a Soul character or style", según la doc), `custom_references_list`.
- **Carpetas y espacios:** `folders_list`, `folders_get`, `folders_create`, `folders_rename`, `folders_delete`, `spaces_list`, `spaces_view`.
- **Descubrir:** `tools_show`.

Los parámetros de cada una no están en la documentación pública: léelos del esquema al cargar la herramienta.

## Llamar la API REST sin enseñar la llave

La llave va **solo** dentro del header, leída de la variable (`MAGNIFIC_API_KEY`, o `FREEPIK_API_KEY` si es la que hay). Nunca `echo`, nunca `-v` ni `--trace`, nunca `set -x`, nunca en un archivo. El prompt nunca va pegado en el comando: se escribe a un archivo y el JSON se arma con `jq` (ver "Llamadas seguras" en el `SKILL.md` de esta skill).

```bash
# 1. Prompt a archivo (heredoc entre comillas: la shell no expande nada)
cat > /ruta/abs/trabajo/<carpeta>/prompt.txt <<'EOF'
…la luz nueva, descrita…
EOF

# 2. Petición armada con jq (los nombres de campo, del esquema del endpoint)
jq -n --arg p "$(cat /ruta/abs/trabajo/<carpeta>/prompt.txt)" \
  --arg img "$(base64 < /ruta/abs/entrada/producto.png | tr -d '\n')" \
  '{image: $img, prompt: $p}' > /ruta/abs/trabajo/<carpeta>/peticion.json

# 3. Llamada
curl -sS -X POST "https://api.magnific.com/v1/ai/image-relight" \
  -H "x-magnific-api-key: ${MAGNIFIC_API_KEY:-$FREEPIK_API_KEY}" \
  -H "Content-Type: application/json" \
  -d @/ruta/abs/trabajo/<carpeta>/peticion.json \
  > /ruta/abs/trabajo/<carpeta>/respuesta.json

# 4. Solo lo que hace falta, nunca el JSON entero
jq -r '.data.task_id // .message // .error' /ruta/abs/trabajo/<carpeta>/respuesta.json
```

Para consultar la tarea, el mismo header: `curl -sS "https://api.magnific.com/v1/ai/image-relight/<task_id>" -H "x-magnific-api-key: ${MAGNIFIC_API_KEY:-$FREEPIK_API_KEY}" > …/estado.json` y `jq -r '.data.status, (.data.generated[]? // empty)' …/estado.json`.

Casi todo es asíncrono: el POST devuelve `data.task_id` y se consulta `GET /v1/ai/<endpoint>/{task-id}` hasta que `data.status` sea `COMPLETED` (los otros: `CREATED`, `IN_PROGRESS`, `FAILED`). El resultado viene en `data.generated[]` como URLs: descárgalas pronto, algunas caducan (las de quitar fondo, en 5 minutos). También acepta `webhook_url` en varios endpoints.

Las imágenes de entrada van como URL pública o en base64, según el endpoint. Solo se suben los archivos que la persona señaló para ese trabajo, y solo a Magnific. Nunca a hosts públicos o de terceros para sacar una URL; si el endpoint pide URL y no hay, usa base64 si lo admite o deriva a otro proveedor.

## Tabla intención → herramienta

| Intención | MCP | REST | Notas |
|---|---|---|---|
| `media.subir` | `creations_request_upload` → `creations_upload` → `creations_finalize_upload` | No hay subida: se manda base64 en cada llamada, o la URL pública que la persona ya tenga | Orden del MCP según los nombres; parámetros sin verificar |
| `imagen.generar` | `images_generate` | `POST /v1/ai/mystic` (Mystic) y endpoints por modelo: Flux 2 Pro, Flux Kontext Pro, Seedream 4 / 4.5, Z-Image Turbo, Hyperflux, Runway | Catálogo con `images_models_list` |
| `imagen.editar` | `images_generate` con referencias | Flux Kontext Pro, Seedream 4.5 Edit | Fidelidad de producto real: compárala contra la foto original |
| `imagen.quitar_fondo` | `images_remove_background` | `POST /v1/ai/beta/remove-background` (form-urlencoded, `image_url`) | Responde directo con las URLs (la doc no dice "síncrono"): `high_resolution`, `preview`, `url`; caducan en 5 min. Pide `image_url`: si no hay URL que la persona ya tenga, usa el MCP o deriva |
| `imagen.expandir` | No está en el MCP | `POST /v1/ai/image-expand/flux-pro` | Por píxeles por lado (`left`, `right`, `top`, `bottom`, 0-2048), no por proporción. `image` en base64, mín. 256 px por lado, máx. 20 MP. Salida tope ~1,6 MP |
| `imagen.escalar` | `images_upscale` | `POST /v1/ai/image-upscaler` (creativo); upscaler de precisión aparte | Factores 2x, 4x, 8x y 16x. El creativo inventa detalle; el de precisión es fiel. Para producto, precisión |
| `imagen.reiluminar` | No está en el MCP | `POST /v1/ai/image-relight`; estado en `GET /v1/ai/image-relight/{task-id}` | La luz sale de un prompt, una imagen de referencia o un mapa de luz; puede cambiar el fondo. Nombres de campos: sin verificar, léelos de la referencia |
| `video.desde_imagen` | `video_generate` | Kling 2.1 / 2.5 / 2.6 Pro, Kling O1, Hailuo 02 / 2.3, WAN 2.5 / 2.6, Runway Gen4 Turbo, Seedance Pro, PixVerse V5 | Catálogo con `video_models_list` |
| `video.desde_texto` | `video_generate` | WAN 2.5 T2V, LTX 2.0 Pro | |
| `video.copiar_movimiento` | Sin verificar si `video_generate` lo expone | Kling 2.6 Motion Control (existe en el índice de la doc; ruta exacta sin verificar) | |
| `video.reemplazar_objeto` | No encontrado | No encontrado | Deriva a Higgsfield |
| `video.reencuadrar` | No encontrado | No encontrado | Deriva a Higgsfield |
| `video.escalar` | No encontrado | No encontrado | Deriva a Higgsfield |
| `identidad.entrenada` | `custom_references_create` | — | La doc dice que entrena un personaje o un estilo. Fotos necesarias, tiempos y con qué modelos se usa: sin verificar |
| `identidad.referencia` | `custom_references_create` (sin verificar si cubre referencia de una sola imagen) | — | Si hace falta referencia instantánea con varios sujetos, Higgsfield (Elementos) |
| `voz.crear` | No encontrado | No encontrado | Deriva a Higgsfield |
| `voz.hablar` | `audio_tts` + `audio_voices_list` | — | Cómo se pasa la voz (id, tipo): sin verificar, léelo del esquema |
| `voz.doblar` | No encontrado | No encontrado | Deriva a Higgsfield |
| `video.ugc` | No | No | Sin flujo guiado; se compone a mano o se deriva a Higgsfield |
| `analisis.video` | No | No | Deriva a Higgsfield |
| `analisis.prediccion` | No | No | Deriva a Higgsfield |
| `costo.consultar` | `account_balance` (saldo); costo previo por llamada: sin verificar | No documentado | Ver Costo informativo |

Extras que no son intención del contrato pero sirven: transferencia de estilo (`POST /v1/ai/image-style-transfer`), vectorizar (`images_to_svg`), generar SVG, música y efectos de sonido por REST, aislar audio, OmniHuman 1.5 (persona hablando desde imagen + audio) y Runway Act Two.

## En `marca/avatar.json`

Clave de proveedor: `"magnific"`, en minúsculas. Misma forma que en los demás proveedores (esquema de avatar 2.0):

```json
{
  "identidades": {
    "magnific": {
      "entrenada": {
        "id": "<id que devuelve custom_references_create>",
        "nombreEnProveedor": "Custom reference",
        "variante": "<tipo que pida el esquema, p. ej. personaje>",
        "modelosCompatibles": [],
        "estado": "lista",
        "creadoEn": "2026-10-17T12:00:00Z"
      }
    }
  },
  "voces": {
    "magnific": { "id": "<id de audio_voices_list>", "tipo": "<si el esquema lo pide>" }
  }
}
```

- `identidades.magnific.entrenada`: lo que devuelve `custom_references_create` cuando entrena un personaje. Si resulta servir como referencia de una sola imagen (sin verificar), va en `.referencia` con la misma forma.
- `modelosCompatibles`: apúntalos al crearla, de lo que diga el esquema o `images_models_show`; sin verificar con qué modelos funciona.
- `voces.magnific`: el identificador de `audio_voices_list`; `tipo` solo si el esquema de `audio_tts` lo pide (sin verificar si hay par id + tipo como en Higgsfield).
- Cómo se pasa media a la referencia guardada: **sin verificar**; léelo del esquema de `custom_references_create`.

Los identificadores de Higgsfield (Soul, Elemento, voz) no sirven aquí.

## Capacidades especiales

| # | Pregunta | En Magnific |
|---|---|---|
| 1 | ¿Flujo guiado de fotografía de producto? | **No** documentado. Se hace con `imagen.editar` + las recetas de `producto` |
| 2 | ¿Reemplazo de objeto en video? | **No** encontrado |
| 3 | ¿Plantillas o presets de movimiento? | API REST: "VFX Video Effects" existe en el índice (detalle sin verificar). MCP: no documentado |
| 4 | Proporciones y límites | `video.reencuadrar`: no existe. `imagen.expandir` (REST): por píxeles por lado (0-2048 cada uno), no por proporción; para llegar a 4:5 o 9:16, calcula los píxeles que faltan. Salida máx. ~1,6 MP, lados múltiplos de 16 |
| 5 | ¿`imagen.escalar` pide ancho/alto de origen? | No según lo leído: se pide el factor (2x a 16x). Parámetros exactos sin verificar |
| 6 | ¿Ruta de copiar movimiento mantenida? | Kling 2.6 Motion Control por REST; por MCP sin verificar |
| 7 | ¿Flujo guiado de UGC y de hoja de personaje? | **No** |
| 8 | ¿Flujo para multiplicar un video propio? | **No** |

## Costo informativo

- Todo sale de los créditos de la cuenta Magnific; el MCP comparte saldo con la app.
- La doc dice que cada generación cuesta según modelo y resolución. **No** se encontró un parámetro tipo "consultar costo sin generar": **sin verificar**. Si el esquema de alguna herramienta lo trae, úsalo como nota.
- Saldo: `account_balance` (MCP). Uso por proyecto: `project_report`.
- Si no hay número previo, se genera igual. Mirar el saldo antes y después es opcional.

## Particularidades

- **Lo que borra o mueve** (regla 4): `folders_delete`, `folders_rename` y `creations_move` solo si la persona lo pidió con esas palabras. Crear una carpeta para el trabajo sí se puede.
- El MCP no tiene reiluminar ni expandir: si la persona solo tiene el MCP conectado y pide esas dos, o configura una llave REST o se deriva.
- Las URLs de resultado caducan. Descarga a `trabajo/…` en cuanto estén.
- Los recursos de stock gratuitos de Freepik piden atribución si la cuenta no es premium (fuente secundaria, sin verificar). Esto no aplica a lo generado.
- La API tiene límites de uso por minuto: `docs.magnific.com/ratelimits`.

## Fuentes

- https://docs.magnific.com — portada de la doc. Consultado 2026-10-17.
- https://docs.magnific.com/llms.txt — índice de endpoints. Consultado 2026-10-17.
- https://docs.magnific.com/modelcontextprotocol — MCP oficial, endpoint, OAuth, lista de herramientas, nota de créditos. Consultado 2026-10-17.
- https://docs.magnific.com/authentication — base URL y header `x-magnific-api-key`. Consultado 2026-10-17.
- https://docs.magnific.com/api-reference/image-relight/image-relight — relight. Consultado 2026-10-17.
- https://docs.magnific.com/api-reference/image-style-transfer/image-styletransfer — transferencia de estilo. Consultado 2026-10-17.
- https://docs.magnific.com/api-reference/remove-background/post-beta-remove-background — quitar fondo. Consultado 2026-10-17.
- https://docs.magnific.com/api-reference/image-expand/post-flux-pro — expandir. Consultado 2026-10-17.
- https://docs.magnific.com/api-reference/image-upscaler-creative/image-upscaler — upscaler. Consultado 2026-10-17.
- https://docs.magnific.com/api-reference/mystic/get-mystic-task — patrón de tareas. Consultado 2026-10-17.
- https://en.wikipedia.org/wiki/Magnific — fecha del relanzamiento de Freepik como Magnific (2026-04-28). Consultado 2026-10-17. No oficial.
- https://mcp.directory/blog/freepik-mcp-complete-guide-2026 y https://anythingmcp.com/es/guides/connect-freepik-to-cursor — fuentes secundarias sobre el cambio de nombre, `FREEPIK_API_KEY` y llaves viejas. Consultado 2026-10-17. No oficiales.

**Sin verificar:** si el banco de stock conserva la marca Freepik; nombre oficial de variable de entorno; validez de llaves viejas de Freepik; parámetros de las herramientas del MCP; consulta de costo previa; ruta exacta de Kling Motion Control; campos de relight y style transfer; si `custom_references_create` sirve como referencia instantánea; par id + tipo de voz; presets de movimiento.
