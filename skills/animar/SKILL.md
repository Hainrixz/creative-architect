---
name: animar
description: Convierte una imagen fija en video y lo deja en el formato de la red donde va (9:16 para Reels y TikTok). Úsala si pides "anima esta foto", "ponle movimiento", "hazlo video", "que la cámara se acerque", "dale vida a esta imagen", "pásalo a vertical", "lo necesito para Reels" o "que se mueva como este otro video".
license: MIT
---

# Animar: que la foto que ya tienes se mueva

Una imagen buena que se mueve tres segundos rinde varias veces más que la misma imagen quieta. Esta skill convierte lo que ya generaste (o lo que ya tenías) en video, y lo deja en el formato que pide la red donde va.

## Cuándo se activa

"Anima esta foto", "hazlo video", "ponle movimiento", "que la cámara se acerque", "dale vida a esta imagen", "pásalo a vertical", "esto lo necesito para Reels", "que se mueva como este otro video".

Si el video hay que crearlo desde cero a partir de un brief, eso es `ugc`. Si hay que sacar variantes de un anuncio que ya existe, es `clonar-anuncio`. Si la imagen de arranque todavía no existe, primero es `producto` o `direccion-de-arte`.

## Antes de empezar

1. **La imagen de arranque.** Puede ser un trabajo que ya generaste en esta sesión (se pasa su identificador directo y no se vuelve a generar) o un archivo suyo. Lo segundo entra por el mecanismo de subida del proveedor (`media.subir`), desde un archivo local o una URL https. **No asumas que el proveedor lee lo que te adjuntan en el chat.**
2. **A dónde va.** Reels, TikTok, feed, anuncio, YouTube. Define la proporción y la duración, y las dos mueven el resultado y el precio.
3. **`marca.json`**, si el video lleva algo escrito o hablado. Si no existe, ofrece armarlo con la forma de `direccion-de-arte/references/marca.md` o sigue con criterio sobrio.
4. **`marca/avatar.json`**, si en el video sale la persona de la marca. Su identidad tiene que mantenerse de la imagen al video; si se pierde a media toma, se descarta.

## El proceso

### 1. Decide qué tipo de movimiento es

Hay caminos distintos y no se parecen. La tabla de decisión completa, por intención, está en `references/movimiento.md`. En corto:

- **La imagen cobra vida sola** (la cámara se acerca, el humo sube, el pelo se mueve): `video.desde_imagen`, con la imagen como fotograma de arranque.
- **Que se mueva como este otro video** (copiar el movimiento de un clip que te pasaron, un baile, un movimiento de cámara): `video.copiar_movimiento`, con las imágenes y exactamente un video de referencia.
- **Cambiar el producto de un video por el tuyo**: `video.reemplazar_objeto`, una intención opcional. No todos los proveedores la tienen; `proveedores` dice si el tuyo sí.

Reemplazar objeto o copiar movimiento, solo sobre un video propio o con licencia. El clip de otro no entra.
- **El video ya existe y solo hay que cambiarle la proporción o la calidad**: `video.reencuadrar` o `video.escalar` (paso 4).

Si el proveedor ofrece plantillas de movimiento con previsualización (presets, efectos), `proveedores` lo indica: cuando la persona quiere "un movimiento con estilo y no sé describirlo", enséñaselas y que elija.

### 2. Escribe el movimiento, no la escena

Este es el error caro. La escena ya está en la imagen: describirla otra vez hace que el modelo la reinvente y pierdas el encuadre que te gustó.

En el prompt va **solo lo que se mueve**: qué hace la cámara, qué hace el sujeto, qué hace el ambiente. El vocabulario completo de movimientos, en `references/movimiento.md`.

Decide la duración antes de generar, no después: es el parámetro que más mueve el precio. Pregunta para qué es antes de pedir diez segundos.

### 3. Genera

Carga `proveedores` para elegir la herramienta y el modelo de la intención que decidiste. Si el proveedor permite consultar el costo (`costo.consultar`), menciónalo en una línea como nota (con la duración y la resolución que vas a usar) y sigue. Si las instrucciones del proyecto o de la persona piden confirmar antes de gastar, mandan ellas. Si no hay proveedor conectado, entrega el prompt de movimiento listo para pegar.

Una petición, un video. Si necesitas varios planos independientes, sácalos en una tanda razonable y revísalos antes de pedir más.

### 4. Deja el formato listo

Genera directo en la proporción final cuando puedas: un video horizontal que va a Reels ya nació con un problema.

Si el video ya existe, `video.reencuadrar` cambia su proporción rellenando los bordes, sin recortar lo importante. Es lo que convierte un video horizontal en el vertical de Reels. Cada proveedor acepta sus propias proporciones y sus propios límites de duración; lo dice su archivo en `proveedores`. Si la proporción que te piden no está (pasa con `4:5` en algunos), sal de la más cercana y recorta después.

`video.escalar` sube la calidad de un video que ya está bien. No arregla uno que está mal.

### 5. Revisa antes de entregar

Los tres fallos que se repiten, y qué hacer con cada uno, en `references/movimiento.md`. El resumen: si el producto se deforma a media toma, si aparece un dedo de más o si el movimiento va y viene como un péndulo, ese video no se entrega.

Si repites, no repitas a ciegas: cambia el prompt, di qué cambiaste y por qué. Tras dos intentos fallidos de la misma pieza, para, explica qué falla y propón otro enfoque.

## Qué entregas

En `trabajo/<fecha>-animar-<slug>/`:

- El video, con su duración y su proporción en el nombre: `packshot-05s-9x16.mp4`.
- `prompts.md` — el prompt de movimiento exacto y la intención usada, para repetir el que funcionó.
- `costo.md` (opcional) — si el proveedor dio el costo, qué se generó y cuánto fue. Es informativo.

## Reglas

- **El prompt describe el movimiento, no la escena.** La escena ya está en la imagen.
- **Poco movimiento gana.** Un acercamiento lento y limpio se ve caro; tres cosas moviéndose a la vez se ve generado.
- **La duración se decide antes.** Casi nadie necesita diez segundos de un solo plano.
- **No repitas a ciegas.** Cada intento nuevo cambia algo del prompt, y dices qué. Tras dos intentos fallidos de la misma pieza, para, explica qué falla y propón otro enfoque.
- **El video de referencia es tuyo o tienes licencia** cuando copias su movimiento o le reemplazas un objeto.
- Si el resultado deforma el producto o la cara, se descarta. No se entrega "porque casi".
- **Esta skill no nombra herramientas ni modelos.** Habla en intenciones; la traducción la hace `proveedores`.
