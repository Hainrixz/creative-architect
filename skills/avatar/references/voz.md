# La voz del avatar

Una cara consistente con una voz distinta cada vez sigue pareciendo tres marcas. La voz se fija igual que la cara.

Aquí está el oficio. Qué herramienta usa cada proveedor para `voz.crear`, `voz.hablar` y `voz.doblar`, con qué parámetros y qué idiomas acepta, lo dice `proveedores`.

## Crear la voz de la persona

Intención `voz.crear`. Algunos proveedores abren su propio widget que graba desde el navegador o sube un archivo y crea la voz de punta a punta; otros reciben un audio ya subido con `media.subir`. Si el proveedor tiene widget, úsalo: cuando avisa que terminó, **ya está**, no hay que llamar a nada más.

- Si la persona prefiere usar un audio que ya tiene, que lo elija en la puerta de subida del proveedor.
- Nunca le pidas que adjunte el audio en el chat si el proveedor tiene su propia puerta: muchos servidores no leen adjuntos del anfitrión, y ese turno se pierde.

Un buen audio de origen: 30 segundos o más, en un cuarto callado, hablando normal y no leyendo como robot. Grabado con el teléfono cerca de la boca queda mejor que con un micro caro a dos metros.

## Usar una voz de la casa

Si nadie quiere prestar su voz, o hace falta una y ya, casi todos los proveedores tienen voces de fábrica además de las propias. Pide la lista: cada voz trae un identificador, casi siempre un tipo (de fábrica o propia) y una muestra para escucharla.

**Si el proveedor separa identificador y tipo, se pasan los dos juntos, siempre.** Un identificador de voz sin su tipo, en esos proveedores, no selecciona nada; `proveedores` dice cuáles son.

Cuando haya varias candidatas, pásale las muestras a la persona y que elija ella. Es su marca la que va a sonar así durante el próximo año.

## Dónde se usa después

- **Voz en off suelta:** `voz.hablar`, con el identificador de la voz y el texto. Si el proveedor permite afinar velocidad, tono o volumen, úsalo con mano ligera.
- **Videos hablados:** si el proveedor tiene flujo guiado de UGC (`video.ugc`), ese flujo ya sabe pedir la voz. Ahí no la manejes por tu cuenta: sigue su documento. Si no lo tiene, `ugc` compone el video con `voz.hablar` y el resto de piezas.
- **Cambiar la voz de un audio que ya existe:** si el proveedor lo soporta, `proveedores` dice cómo se llama.
- **Doblar un video a otro idioma, con labios sincronizados:** `voz.doblar`. Útil para reciclar un reel que funcionó a otro mercado.

Crear, cambiar y doblar voz suelen costar, y no todos los proveedores dejan consultar el precio antes. Si se puede, se dice en una línea; si no, se anota en `costo.md` lo que el proveedor informe después. No se frena el trabajo por eso.

## La regla

Se clona la voz de quien la está prestando, en ese momento, sabiendo qué se va a hacer con ella. Punto.

No la de un cliente que mandó un audio de WhatsApp. No la de un socio que "seguro no le importa": "tengo su permiso" dicho por otro no basta, consiente quien presta la voz, en la conversación. No la de nadie famoso ni de ningún político, ni "parecida a", ni para ponerle en la boca algo que no dijo. Nunca la de un menor, aunque lo pida su padre o su madre. Y nada sexual ni que finja ser noticia o comunicado oficial con una voz real. Suplantar una voz es un problema legal en la mayoría de los países donde va a correr esto, y además se nota.

Si te lo piden igual, dilo de frente y ofrece lo que sí se puede: una voz de fábrica que suene parecida en tono y ritmo, elegida por la persona con las muestras delante.

## Guárdalo

El identificador de la voz va a `marca/avatar.json`, en `voces.<proveedor>`: `id`, `tipo` si el proveedor lo usa, `esVozPropia` (true si es la de alguien que la prestó) y una `nota` de cómo suena. Una por proveedor; nunca llaves ni tokens. Una voz creada que nadie apuntó es una voz que se paga dos veces.

Y como la voz clonada no viaja de un proveedor a otro, guarda también el audio de origen en `marca/avatar/` si la persona está de acuerdo. Con él se vuelve a crear la voz en otro proveedor sin pedirle que grabe otra vez.
