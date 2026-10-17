# Identidad entrenada o referencia guardada: la decisión que define tu avatar

Casi todos los proveedores que sirven para avatares tienen dos formas de guardar una identidad reutilizable. Se llaman distinto en cada uno (en Higgsfield, por ejemplo, son **Soul** y **Elemento**), pero la lógica es la misma. No son intercambiables, y elegir mal se descubre tarde, cuando ya entrenaste.

Los nombres concretos, las herramientas, los modelos compatibles y los límites de cada proveedor viven en `proveedores/references/<proveedor>.md`. Aquí está el criterio, que no cambia de un proveedor a otro.

## La tabla

| | **Identidad entrenada** (`identidad.entrenada`) | **Referencia guardada** (`identidad.referencia`) |
|---|---|---|
| Qué es | Un modelo entrenado con la cara de una persona | Una imagen de referencia guardada con nombre |
| Qué necesita | 5 a 20 fotos de la **misma** persona | Una imagen basta |
| Cuánto tarda | Minutos. No bloquea: se consulta el estado | Al momento |
| Cuántos sujetos por escena | **Uno solo**, y no se negocia | Varios en la misma imagen |
| Sirve para | Personas | Personas, objetos, lugares, props |
| Fidelidad | Más alta: aprendió esa cara | Buena, pero es parecido, no identidad entrenada |
| Con qué modelos | Normalmente **pocos**, los que el proveedor diseñó para eso | Normalmente más: varios de imagen y algunos de video |
| Cómo se usa | Su identificador en la llamada de generación | Depende del proveedor: un identificador aparte o una marca dentro del texto del prompt |

Si el proveedor que tienes conectado no soporta alguna de las dos, `proveedores` lo dice. Y si no soporta ninguna, el avatar se sostiene a mano: la imagen ancla de la hoja de personaje va como referencia en cada `imagen.editar`, y la descripción escrita del personaje va en cada prompt.

## Cómo se decide en dos preguntas

**¿Es la cara de una persona concreta, sola en el cuadro, y tienes 5 o más fotos suyas (o una hoja de personaje completa)?**
Sí a todo, es **identidad entrenada**. Es el gemelo digital: te va a salir esa persona, no alguien parecido.

**¿Cualquier otra cosa?**
Es **referencia guardada**. Y en particular lo es, sin discutir, si:
- Hay más de una persona en la escena. La identidad entrenada no puede.
- El sujeto no es una persona: tu botella, tu empaque, tu local.
- Solo tienes una imagen.
- Quieres el resultado ahora.
- Vas a usar un modelo que no está en la lista corta de los que aceptan la identidad entrenada de ese proveedor.

## Lo que se rompe si eliges mal

- Entrenas una identidad y luego la quieres usar con un modelo que no la acepta: no se puede, y el entrenamiento ya se pagó. Antes de entrenar, mira en `proveedores` con qué modelos funciona y confirma que ahí está el que vas a usar.
- Quieres a tu avatar dándole la mano a un cliente en la misma foto: con identidad entrenada no se puede, hay que rehacerlo como referencias guardadas.
- Guardas al personaje como referencia y esperas fidelidad de gemelo digital: te va a dar un aire, no la cara.
- Entrenas en un proveedor y mañana cambias a otro: el identificador no viaja. Lo que sí viaja es la hoja de personaje y las fotos, por eso se guardan en `marca/avatar/` y no solo en la nube del proveedor.

## No es una u otra: pueden convivir

En el mismo proveedor puedes tener las dos para el mismo personaje: la entrenada para los planos donde sale sola y la referencia para las escenas con más gente o con modelos que no aceptan la entrenada. En `marca/avatar.json` cada proveedor tiene dos casillas opcionales, `identidades.<proveedor>.entrenada` e `identidades.<proveedor>.referencia`, y se llenan las que existan. Cada una guarda:

- `id`, el único obligatorio.
- `nombreEnProveedor` (p. ej. Soul, Elemento) y `variante` si el proveedor la distingue (p. ej. `soul_2`, `soul_cinematic`).
- `url`, solo si el proveedor la pide junto al id (en Higgsfield, la referencia guardada).
- `modelosCompatibles`, `estado` y `creadoEn`.

Nunca llaves ni tokens: solo identificadores. La forma exacta está en `avatar.schema.json`.

## Cómo se suben las imágenes

Con `media.subir`, igual para las dos. Lo que cambia de un proveedor a otro es la puerta (un widget, una URL https, el identificador de un trabajo que ya generaste) y la forma exacta en que cada intención pide la media. Eso está en `proveedores`, y es justo donde se cae la llamada si se hace de memoria: hay proveedores que piden para la referencia guardada algo más que el identificador pelado (en Higgsfield, el identificador **y** la URL https que devolvió la subida). Guarda en cuanto subas lo que la identidad va a necesitar después (el id y, si el proveedor la pide, la URL), en la casilla `url` de esa identidad.

Nunca pidas que adjunten el archivo en el chat si el proveedor tiene su propia puerta de subida: muchos servidores no leen adjuntos del anfitrión y ese turno se pierde.

## Las fotos que dan buen resultado

Si son de una persona real, tienen que ser de quien está hablando contigo y dio su permiso en la conversación; "tengo su permiso" de parte de otro no basta. Si parecen de un famoso o de alguien distinto, no se entrena. Y nunca de un menor, aunque lo pida su padre o su madre. Con eso claro, pídelas así:
- Entre 8 y 15, mejor que 5.
- Luz distinta en varias: día, interior, sombra.
- Ángulos distintos: de frente, de tres cuartos, de perfil.
- Expresiones distintas, con y sin sonrisa.
- Sin lentes de sol, sin filtros de belleza, sin nadie más en el cuadro.
- Nada de fotos ya generadas por IA **cuando el avatar es una persona real**: entrenar sobre generado degrada el parecido. Para un personaje inventado es al revés, y la hoja de personaje generada es la fuente correcta, porque no hay ninguna cara real que respetar.

Un juego de 12 fotos variadas da un avatar notablemente mejor que 20 selfies iguales del mismo día.

## Y para la referencia guardada

Una sola imagen, así que tiene que ser la buena: de frente o tres cuartos, luz pareja, fondo limpio, la cara grande en el cuadro, expresión neutra. Lo que salga en esa imagen (el peinado, la ropa, la sombra en media cara) va a querer repetirse en todo lo demás.
