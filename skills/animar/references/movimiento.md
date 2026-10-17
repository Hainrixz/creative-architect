# Movimiento: qué ruta tomar y cómo describirlo

## La tabla de decisión

| Lo que te piden | Intención | Cómo se arma |
|---|---|---|
| "Anima esta foto", "que la cámara se acerque", "que se mueva un poco" | `video.desde_imagen` | La imagen va como fotograma de arranque; el prompt describe **solo** el movimiento |
| "Que se mueva igual que este video", "cópiale el baile", "el mismo movimiento de cámara" | `video.copiar_movimiento` | Las imágenes de referencia y **exactamente un** video de origen, propio o con licencia |
| "Cambia el producto de este video por el mío" | `video.reemplazar_objeto` (opcional) | Igual: imágenes de referencia más un solo video de origen, propio o con licencia. No todos los proveedores lo tienen; `proveedores` dice si el tuyo sí |
| "Un personaje quieto que siga este clip" | `video.copiar_movimiento` | Una imagen del personaje y el clip. Si el proveedor tiene más de una ruta para esto, `proveedores` dice cuál está mantenida |
| "Quiero un movimiento con estilo, no sé describirlo" | `video.desde_imagen` con plantilla del proveedor | Si el proveedor tiene presets con previsualización, enséñaselos y que elija |
| "Ponlo vertical", "que llene la pantalla del teléfono" | `video.reencuadrar` | Un video, una proporción destino |
| "Súbelo de calidad", "que se vea nítido" | `video.escalar` | Algunos proveedores cobran bastante más según los fps o la resolución destino; si se puede consultar, dilo como nota |

Reemplazar objeto o copiar movimiento se hace solo sobre un video propio o con licencia. Sobre el clip de otro, no: eso ya es reeditar su pieza.

## Cómo se escribe un prompt de movimiento

La escena ya está en la imagen. Si la vuelves a describir, el modelo la reinterpreta y pierdes el encuadre que te costó conseguir. En el prompt van tres cosas y en este orden:

**1. La cámara.** Qué hace el punto de vista:

- Acercamiento lento (*slow push in*) — el que mejor funciona casi siempre.
- Alejamiento que revela (*pull back*) — para descubrir el contexto.
- Órbita alrededor del sujeto — muy vistoso, muy propenso a deformar.
- Paneo lateral — para recorrer algo largo.
- Cámara fija — sí, es una opción, y a veces la mejor.

**2. El sujeto.** Qué hace lo que está en el cuadro. Un gesto, uno solo: gira la cabeza, levanta el producto, parpadea, camina hacia la cámara.

**3. El ambiente.** Lo que se mueve por su cuenta: el vapor que sube, el humo, el polvo en el rayo de luz, la tela que ondea, el líquido que se mueve en el vaso.

Ejemplo de lo que sí:

> Acercamiento muy lento hacia el frasco. El vapor del café sube y se disuelve al salir del cuadro. La luz de la ventana no cambia. Todo lo demás, quieto.

Ejemplo de lo que no:

> Un frasco de café de especialidad sobre una mesa de madera, con luz de mañana, en una cocina moderna, mientras la cámara se acerca.

El segundo vuelve a describir la escena y el modelo se siente autorizado a rehacerla. El primero solo pide movimiento.

## La regla que más resultados salva: menos es más

Un acercamiento lento y limpio se ve caro. Tres cosas moviéndose a la vez se ve generado por IA, y se ve a un metro de distancia.

Cuando dudes: **una cosa de la cámara, una cosa del sujeto, nada más**. Si el resultado se siente muerto, siempre puedes pedir más en la siguiente. Al revés no funciona.

## Duración

La duración es el parámetro que más mueve el precio, así que se decide antes, no después:

| Para qué | Cuánto |
|---|---|
| Un plano de producto en un carrusel o en un anuncio | 3 a 5 s |
| Un gancho de Reel | 5 s |
| Un plano suelto dentro de un montaje más largo | 5 s |
| Una escena que tiene que contar algo | 8 a 10 s |

Casi nadie necesita diez segundos de un solo plano. Pregunta para qué es antes de pedirlos.

## Formato: pídelo con el destino claro

Genera directo en la proporción final cuando puedas. Si el modelo acepta proporción, pásala explícita: varios modelos salen en horizontal por defecto, y un video horizontal que va a Reels ya nació con un problema.

`video.reencuadrar` es el arreglo cuando el video ya existe: rellena los bordes en vez de recortar, así que no te come el sujeto. Cada proveedor acepta sus propias proporciones y sus propios límites (duración máxima, parámetros extra para videos largos); lo dice su archivo en `proveedores`.

Qué proporción pide cada red, si tienes el plugin `publicar`, vive ahí. No la copies aquí.

## Los tres fallos que se repiten

**1. El producto se deforma a media toma.** El modelo "recuerda" mal la forma cuando algo se mueve mucho. Se arregla bajando el movimiento y volviendo a anclar el producto en el prompt: "el frasco no cambia de forma ni de etiqueta en ningún momento". Se descarta la toma mala, no se entrega.

**2. Manos y caras.** Mira los dedos y mira los ojos. Si hay un dedo de más o la mirada se va a ninguna parte, no se entrega: se nota más en video que en foto, porque el ojo tiene medio segundo para descubrirlo.

**3. El movimiento de péndulo.** Va y regresa al punto de partida, y se ve falso. Pasa cuando el prompt pide un movimiento sin dirección clara. Se arregla diciendo hacia dónde y que no vuelva: "la cámara se acerca de forma continua, sin regresar".

Ninguno de los tres se arregla generando otra vez lo mismo y cruzando los dedos. Cambia el prompt, di lo que cambiaste y por qué, y entonces vuelve a generar. Tras dos intentos fallidos de la misma pieza, para, explica qué falla y propón otro enfoque.
