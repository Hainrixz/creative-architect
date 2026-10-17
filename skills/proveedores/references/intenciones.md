# Vocabulario de intenciones

Este es el contrato entre las skills de oficio y la capa de proveedores. Las skills de oficio hablan solo con estas palabras; cada archivo de proveedor (`higgsfield.md`, `magnific.md`, `openai.md`, o uno nuevo hecho con `nuevo-proveedor.md`) dice, por intención, qué herramienta usa, con qué parámetros clave, o que no la soporta.

Si un archivo de proveedor o una skill contradicen esta lista, manda esta lista.

## Intenciones

| Intención | Qué hace |
|---|---|
| `media.subir` | Meter un archivo local o una URL al proveedor y obtener un identificador |
| `imagen.generar` | Texto → imagen |
| `imagen.editar` | Imagen(es) de referencia + texto → imagen (producto real, cambio de escena) |
| `imagen.quitar_fondo` | Recorte sobre transparente |
| `imagen.expandir` | Outpaint / cambiar proporción sin recortar |
| `imagen.escalar` | Upscale a 2K/4K |
| `imagen.reiluminar` | Cambiar la luz de una imagen existente |
| `video.desde_imagen` | Imagen de arranque + prompt de movimiento → video |
| `video.desde_texto` | Texto → video |
| `video.copiar_movimiento` | Imagen(es) + video de referencia → mismo movimiento |
| `video.reemplazar_objeto` | Video propio + imagen de tu producto → mismo video con tu producto. El video de origen es de la persona, nunca el de la competencia |
| `video.reencuadrar` | Cambiar la proporción de un video existente |
| `video.escalar` | Upscale de video |
| `identidad.entrenada` | Identidad entrenada de una persona (varias fotos, tarda) |
| `identidad.referencia` | Referencia guardada reutilizable (una imagen, al momento, varios sujetos) |
| `voz.crear` | Clonar la voz de quien la presta |
| `voz.hablar` | Texto → audio con una voz |
| `voz.doblar` | Doblar un video a otro idioma |
| `video.ugc` | Flujo guiado de video tipo UGC, si el proveedor lo tiene |
| `analisis.video` | Desglose por escenas de un video |
| `analisis.prediccion` | Predicción de rendimiento/viralidad |
| `costo.consultar` | Estimar el costo sin generar (solo informativo) |

## Capacidades especiales

No son intenciones: son preguntas que cambian **cómo** se ejecuta una intención en un proveedor. Cada archivo de proveedor las contesta en su sección "Capacidades especiales", con este número y en este orden, para que una skill de oficio pueda preguntar "¿el proveedor tiene la capacidad 4?" sin leer el archivo entero.

| # | Pregunta | Quién la usa |
|---|---|---|
| 1 | ¿Hay flujo guiado de fotografía de producto? | `producto` |
| 2 | ¿Hay reemplazo de objeto en video (`video.reemplazar_objeto`)? | `producto`, `clonar-anuncio` |
| 3 | ¿Hay plantillas o presets de movimiento? | `animar` |
| 4 | ¿Qué proporciones y límites tienen `video.reencuadrar` e `imagen.expandir`? (p. ej. si existe 4:5, duración máxima) | `animar`, `producto` |
| 5 | ¿`imagen.escalar` pide ancho y alto de la imagen de origen? | `producto`, `direccion-de-arte` |
| 6 | ¿Qué ruta de `video.copiar_movimiento` está mantenida y cuál es antigua? | `animar` |
| 7 | ¿Hay flujo guiado de UGC y de hoja de personaje? (con sus nombres) | `ugc`, `avatar` |
| 8 | ¿Hay flujo para multiplicar un video propio en versiones? (con su nombre) | `ugc`, `clonar-anuncio` |

Si la respuesta es "no", el archivo dice a quién derivar o cómo se aproxima con otras intenciones.
