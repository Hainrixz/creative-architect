---
name: avatar
description: El mismo personaje en todos tus videos, con la misma cara y la misma voz. Úsala si pides un avatar o presentador de marca, "mi gemelo digital", "un personaje que salga siempre", "que sea la misma persona en todos los videos", "clona mi voz" o "una hoja de personaje".
license: MIT
---

# Avatar: la misma cara en todos tus videos

Un avatar de marca es un personaje que sale idéntico en cada generación: misma cara, misma edad, mismo estilo, misma voz. Se arma una vez y se reutiliza para siempre, en el proveedor que tengas hoy y en el que conectes mañana.

## Por qué esto importa más de lo que parece

Sin avatar, cada generación te da una persona distinta. Publicas tres reels y parecen de tres marcas: nadie asocia una cara con tu nombre, nadie te reconoce al pasar el dedo, y el conjunto se siente exactamente como lo que es, imágenes de banco.

Con avatar, la tercera vez que alguien ve a tu personaje ya sabe de quién es el video antes de leer el texto. Eso es lo que se compra aquí: reconocimiento, que es lo único que el dinero de medios no te da.

## Cuándo se activa

"Quiero un avatar de mi marca", "un personaje que salga siempre", "que sea la misma persona en todos los videos", "clona mi voz", "un presentador para mi contenido", "mi gemelo digital", "hazme la hoja de personaje".

Si lo que quieren es un video con ese personaje hablando de un producto, el avatar es la materia prima y el video es `ugc`. Si quieren una foto de producto donde sale una persona, es `producto`, que lee este mismo avatar.

## Antes de empezar

1. **`marca.json`, si lo hay.** El personaje se deriva del negocio y del público, no del gusto del momento. Si existe, úsalo: de ahí salen el vestuario, el entorno y el tono. Si no existe, no lo exijas: haz tres preguntas rápidas (qué vende, a quién, con qué tono) y, si la persona quiere, deja un `marca.json` mínimo con la forma de `direccion-de-arte/references/marca.md`. Un avatar va a salir en todo lo que publique durante meses; derivarlo de un negocio que no conoces es empezar con la cara equivocada.
2. **Decidir a quién representa.** Dos caminos, y se eligen con la persona, nunca por ella:
   - **Es ella misma.** Necesitas de 5 a 20 fotos suyas y su permiso explícito, dado por ella en esta conversación. Si es otra persona, "tengo su permiso" no basta: consiente quien presta la cara.
   - **Es un personaje inventado.** No hace falta ninguna foto de nadie, pero una identidad entrenada sigue necesitando de 5 a 20 imágenes: salen de la hoja de personaje del paso 3. Ahí entrenar sobre imágenes generadas sí es lo correcto, porque no hay una cara real que respetar.
3. **Qué proveedor vas a usar.** Pregúntale a la skill `proveedores` qué hay conectado y cuál soporta `identidad.entrenada`, `identidad.referencia` y `voz.crear`. No todos tienen las tres. Si ninguno las tiene, el avatar sigue existiendo: es la descripción, la hoja de personaje y las imágenes de referencia, y se usan con `imagen.editar` cada vez.

## El proceso

### 1. Define al personaje

Cuatro datos y ni uno más: **quién es** (edad aproximada, rasgos, de dónde parece), **cómo viste** (que case con `marca.json` si existe), **dónde suele estar** (el fondo típico de tus videos) y **cómo suena** (rápido y cercano, pausado y serio, lo que sea que hable tu marca).

Escríbelo en tres o cuatro frases y confírmalo antes de generar nada. Cambiar esto después significa volver a entrenar.

### 2. Elige la forma: identidad entrenada o referencia guardada

No son lo mismo y la diferencia se paga. En corto: la **identidad entrenada** (`identidad.entrenada`) aprende la cara de **una** persona a partir de varias fotos, tarda y suele funcionar solo con algunos modelos del proveedor; la **referencia guardada** (`identidad.referencia`) sale de una sola imagen, está lista al momento y admite varios sujetos en la misma escena. En Higgsfield, por ejemplo, se llaman Soul y Elemento; cada proveedor tiene su nombre y sus límites, y eso lo dice `proveedores`.

La decisión en dos preguntas, qué se rompe si eliges mal y qué fotos dan buen resultado, en `references/identidad.md`.

**Pregunta cuál quiere. No elijas tú en silencio.**

### 3. La hoja de personaje

Antes de generar la primera imagen del personaje, arma la hoja: el juego de vistas consistentes que es la materia prima del avatar.

- **Si el proveedor tiene flujo guiado de hoja de personaje**, úsalo. `proveedores` dice cómo se llama y cómo se pide. Ese documento manda sobre esta skill en lo técnico.
- **Si no lo tiene**, la armas tú. Genera primero una imagen ancla con `imagen.generar` (frente, luz pareja, fondo neutro, el personaje del paso 1 escrito por bloques como dice `direccion-de-arte`). Después, con esa misma imagen como referencia en `imagen.editar`, pide cada vista por separado: **frente**, **tres cuartos**, **perfil**, **cuerpo completo** y una tanda de **expresiones** (neutra, sonrisa, hablando, sorpresa). Misma ropa, mismo fondo y misma luz en todas; lo único que cambia es la vista. Si una vista se aleja de la cara del ancla, se descarta y se repite, no se "arregla" encima.

Si el avatar es la persona real, la hoja sale de sus fotos y no se inventa: las vistas que falten se piden a la persona, no se generan.

Si una vista sale mal dos veces seguidas, para, explica qué falla y propón otro enfoque (otra ancla, otra luz, otro proveedor) en lugar de un tercer intento igual.

### 4. Crea la identidad

Con la hoja (o las fotos reales) en la mano:

- **Identidad entrenada:** intención `identidad.entrenada` con de 5 a 20 imágenes y un nombre. Tarda y no bloquea: sigue con la voz mientras termina.
- **Referencia guardada:** intención `identidad.referencia` con la imagen ancla. Queda lista al momento.

Antes, cada imagen se sube con `media.subir`. Nunca pidas que adjunten archivos en el chat si el proveedor tiene su propia puerta de subida: `proveedores` dice cuál es.

Si el proveedor permite consultar el costo del entrenamiento sin generar, menciónalo en una línea y sigue. Si las instrucciones del proyecto o de la persona piden confirmar antes de gastar, mandan ellas.

Las dos pueden convivir en el mismo proveedor: un Soul para los primeros planos donde sale sola y un Elemento para las escenas con más gente, por ejemplo. Se guardan juntas (paso 6).

### 5. La voz

Intención `voz.crear`, con un audio de quien presta la voz. Cómo grabarlo bien, cómo usar una voz de fábrica si nadie quiere prestar la suya y dónde se usa después, en `references/voz.md`.

Y una raya que no se cruza: **solo se clona la voz de quien la está prestando**, en ese momento y a sabiendas. La voz de un tercero, de un famoso o de un cliente, no.

### 6. Guarda los identificadores

Todo esto no vale nada si mañana no encuentras el identificador. Escribe `marca/avatar.json` con la forma exacta de `references/avatar.schema.json`: la descripción una vez, y las identidades y voces **por proveedor**. Si mañana conectas otro proveedor, se agrega su entrada y lo demás se queda.

Dentro de cada proveedor, `identidades.<proveedor>` tiene dos casillas opcionales, `entrenada` y `referencia`, y pueden estar llenas las dos. Cada una guarda `id` (lo único obligatorio) y, si aplica, `nombreEnProveedor`, `variante` (p. ej. `soul_2`), `url` (cuando el proveedor la pide junto al id), `modelosCompatibles`, `estado` y `creadoEn`. La voz va en `voces.<proveedor>` con su `id` y su `tipo` si el proveedor lo usa:

```json
{
  "version": "2.0",
  "nombre": "Sofi",
  "identidades": {
    "higgsfield": {
      "entrenada": { "id": "…", "nombreEnProveedor": "Soul", "variante": "soul_2", "estado": "lista" },
      "referencia": { "id": "…", "nombreEnProveedor": "Elemento", "url": "https://…" }
    }
  },
  "voces": { "higgsfield": { "id": "…", "tipo": "element", "esVozPropia": true } }
}
```

Ese archivo es el que lee `ugc` para que salga siempre el mismo personaje, y el que lee `producto` cuando en la foto sale una persona.

Si ya existía, no lo pises: agrega o actualiza solo la entrada del proveedor que usaste, y avisa qué cambió. Si lo que cambia es la descripción del personaje, eso sí se confirma antes, porque es otro personaje.

## Qué entregas

En `trabajo/<fecha>-avatar-<slug>/`:

- `prompts.md`: el prompt del ancla y el de cada vista de la hoja, por bloques, listos para repetir en otro proveedor.
- `costo.md`, solo si hubo gasto y el proveedor lo informa: qué se generó y cuánto costó, como nota.

Y en la marca, que es lo que se reutiliza:

- `marca/avatar.json` con la descripción y los identificadores por proveedor.
- Las imágenes de la hoja de personaje, en `marca/avatar/`.

`marca/avatar/` tiene caras y voces: no lo subas a un repo público.

En el chat, tres líneas: cómo se llama, en qué proveedor y con qué modelos funciona, y qué falta (p. ej. "la voz todavía es de fábrica").

Si no hay ningún proveedor conectado, entregas igual la descripción, la hoja escrita como prompts por bloques y el `avatar.json` sin identidades, listo para llenarse el día que se conecte uno.

## Reglas

- **Una identidad entrenada por escena.** Si van dos personas en el cuadro, la identidad entrenada no sirve: eso son referencias guardadas.
- **Las fotos son de quien da permiso.** Nunca entrenes con la cara de un tercero, y menos con la de alguien famoso. Si las fotos parecen de un famoso o de otra persona distinta de quien habla contigo, no se entrena.
- **La voz también es de quien da permiso.** Sin excepciones.
- **Consiente quien presta la cara o la voz, en la conversación.** "Tengo su permiso" dicho por un tercero no basta.
- **Menores, nunca.** No se entrena ni se clona la cara o la voz de un menor, aunque lo pida su padre o su madre. Los personajes inventados son adultos.
- **Nada sexual ni desnudos** con una persona real o con alguien que se le parezca.
- **Figuras públicas y políticos, no.** Ni ellos ni "parecidos", ni poniéndoles dichos o respaldos que no hicieron.
- **Nada que finja ser noticia o documento oficial.**
- **Lo generado se declara** cuando el avatar aparece en algo publicado.
- **Nada de llaves ni tokens en `avatar.json`.** Solo identificadores, que son públicos dentro de la cuenta.
- **El costo es nota, no candado.** Si se puede consultar, se dice en una línea y se sigue. Nunca compres créditos ni planes por la persona.
- Si el personaje cambia de estilo cada mes, no es un avatar. Elige uno y sostenlo.
