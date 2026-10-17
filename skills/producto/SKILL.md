---
name: producto
description: Convierte la foto real de tu producto en foto de catálogo o creativo de anuncio, sin sesión de fotos y sin inventarte el producto. Úsala si pides fotos de mi producto, un packshot, una foto de catálogo, fondo blanco, quítale el fondo, ponlo sobre un fondo bonito, la foto para el anuncio o fotos para la tienda.
license: MIT
---

# Producto: de la foto del celular a la foto de catálogo

Esta skill toma la foto real de tu producto, la que tomaste sobre la mesa de la cocina, y la convierte en la imagen que se ve en un anuncio, en una ficha de tienda o en un carrusel.

La palabra importante es **real**. No se inventa el producto: se le cambia el escenario, la luz y el encuadre. Un producto inventado se ve precioso hasta que llega el paquete y no se parece.

## Cuándo se activa

"Fotos de mi producto", "un packshot", "una foto de catálogo", "que se vea profesional", "la foto para el anuncio", "quítale el fondo", "ponlo sobre un fondo bonito", "fotos para la tienda".

Si lo que quieren es que el producto se mueva, es `animar`. Si quieren a alguien usándolo y hablando, es `ugc`. Si quieren variantes de un anuncio que ya existe, es `clonar-anuncio`.

## Antes de empezar

1. **`marca.json`.** De ahí salen los colores, el estilo y la categoría. Sin eso las fotos salen bonitas y ajenas. Si no existe, dilo en una línea y ofrece armarlo en cinco preguntas, con la forma de `direccion-de-arte/references/marca.md`. Aquí vale insistir una vez, porque sin los colores y el estilo las fotos salen ajenas y volver a generarlas es trabajo (y gasto) repetido. Si dice que siga igual, sigue con criterio sobrio.
2. **`marca/avatar.json`, si en la foto sale una persona.** Las recetas *Lifestyle* y *En la mano* llevan gente. Si la marca ya tiene avatar, se usa su identidad (`identidad.entrenada` o `identidad.referencia`, con el identificador que el archivo guarda para el proveedor en uso) para que sea la misma persona de siempre. Si no existe, se dice en una línea y se sigue sin él; crearlo es cosa de `avatar`.
3. **La foto real del producto.** Es el insumo, no un extra. Pídela por el mecanismo de subida del proveedor (`media.subir`): un archivo de la computadora o una URL https (la ficha de la tienda, por ejemplo). **No asumas que el proveedor lee lo que te adjuntan en el chat**; la skill `proveedores` dice cómo entra un archivo en cada uno.
   Qué pedirles para que la foto sirva, en `references/recetas.md`.
4. **Para qué es cada foto.** Una foto de catálogo, una de anuncio y una de carrusel no se parecen en nada. Pregúntalo antes.

## El proceso

1. **Sube la foto real** (`media.subir`) y confirma que se ve el producto completo, enfocado y sin que se le encime nada.
2. **Mira si hay flujo guiado.** Si el proveedor tiene un flujo propio para fotografía de producto, la skill `proveedores` lo indica y se puede usar. Si lo usas, sus instrucciones mandan en lo técnico; las recetas, el ancla de fidelidad y la revisión contra el producto real de esta skill siguen valiendo.
3. **Elige la receta.** Packshot limpio, lifestyle, en la mano, macro de textura, sobre bloque de color, en su contexto de uso. Las recetas completas, con luz, fondo y encuadre de cada una, en `references/recetas.md`.
4. **Escribe el prompt por bloques.** No lo improvises: la estructura de un prompt que sale profesional en vez de genérico es la skill `direccion-de-arte`. Cárgala y úsala. Ahí también está el ancla de fidelidad, que es la frase que obliga al modelo a respetar el producto real.
5. **Genera.** Carga `proveedores` para elegir la herramienta. La intención es `imagen.editar`: la foto real como referencia más el prompt. Nunca `imagen.generar` a secas para un producto que existe. Si sale una persona y hay avatar, su identidad va en la misma llamada como el proveedor la pida. Si el proveedor permite consultar el costo (`costo.consultar`), menciónalo en una línea como nota y sigue. Si las instrucciones del proyecto o de la persona piden confirmar antes de gastar, mandan ellas. Si vas a sacar varias recetas, sácalas en una tanda razonable y revisa antes de pedir más.
6. **Limpia lo que haga falta.**
   - `imagen.quitar_fondo` deja el producto recortado sobre transparente, que es lo que pide una ficha de tienda.
   - `imagen.expandir` ensancha la imagen si te la piden en otra proporción, sin recortar el producto.
   - `imagen.escalar` la sube a 2K o 4K para impresión.
   También son generaciones: si el proveedor da su costo, va en la misma nota informativa.
7. **Revisa contra el producto real.** Pon la foto original al lado. Si cambió el color, la etiqueta, la forma de la tapa o el logo, se descarta. Esa foto no es de tu producto. Si repites, cambia el renglón del prompt que falló y di cuál; no vuelvas a mandar lo mismo. Tras dos intentos fallidos de la misma pieza, para, explica qué falla y propón otro enfoque.

## Para carruseles: pide las imágenes SIN texto

La tipografía que hornea un modelo de imagen no es de fiar. A veces sale perfecta y a veces sale con una letra de más, un acento donde no va, o una palabra que no existe. Y no lo puedes corregir: está pintada en los píxeles.

Así que cuando la imagen vaya a un carrusel, o a cualquier pieza donde el texto importe:

- **Pide la imagen limpia.** En el bloque de "qué evitar" del prompt va explícito: sin texto, sin letras, sin logotipos, sin marcas de agua.
- **Deja el aire donde va el texto.** Un tercio libre arriba o abajo, o un fondo plano en la zona donde después irá el titular.
- **El texto lo pone quien sabe ponerlo**, encima, en una herramienta de diseño donde es editable, se puede corregir y respeta la tipografía de tu marca. Si tienes el plugin `carruseles`, ese es su trabajo.

Es media línea en el prompt y te ahorra tirar cuatro generaciones porque una decía "OFERTTA".

## Qué entregas

En `trabajo/<fecha>-producto-<slug>/`:

- Las imágenes, nombradas por receta: `packshot-01.png`, `lifestyle-01.png`.
- `prompts.md` — el prompt exacto de cada una, y la receta que usó. Es lo que permite repetir la buena y no la mala.
- `costo.md` (opcional) — si el proveedor dio el costo, qué se generó y cuánto fue. Es informativo.

## Reglas

- **La foto real es el punto de partida.** Si no hay foto, se dice que sin ella el resultado no es tu producto y se pregunta si aun así lo quieren como imagen de concepto.
- **El producto no cambia.** Ni el color, ni la etiqueta, ni la forma. Si cambió, se descarta la generación.
- **Sin texto horneado** cuando la pieza lleve texto encima.
- **No repitas a ciegas.** Cada intento nuevo cambia algo del prompt, y dices qué. Tras dos intentos fallidos de la misma pieza, para, explica qué falla y propón otro enfoque.
- **No asumas que el proveedor lee adjuntos del chat.** La foto entra por `media.subir`.
- **Esta skill no nombra herramientas ni modelos.** Habla en intenciones; la traducción la hace `proveedores`.
