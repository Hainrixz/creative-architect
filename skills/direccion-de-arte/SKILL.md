---
name: direccion-de-arte
description: Escribe el prompt de imagen que sale profesional en vez de genérico, por bloques, con luz, lente y exclusiones decididas. Úsala si pides mejorar mi prompt, dirección de arte, me salió genérico, no se ve profesional, se ve feo, quiero que se vea caro, esta imagen parece hecha con IA o cómo le pido que se vea como esta referencia.
license: MIT
---

# Dirección de arte: la diferencia entre "una foto de mi producto" y una foto que vende

Dos prompts, el mismo modelo, el mismo precio. Uno devuelve una imagen de banco que ya viste mil veces. El otro devuelve algo que parece de una campaña. La diferencia no es el modelo: es cuántas decisiones tomaste tú y cuántas dejaste que las tomara la máquina.

Un prompt corto no es un prompt libre. Es un prompt donde el modelo elige por ti, y el modelo siempre elige el promedio: luz plana, encuadre centrado, fondo neutro, todo enfocado. El promedio de internet. Eso es exactamente lo que se ve genérico.

## Cuándo se activa

"Me salió genérico", "no se ve profesional", "mejora este prompt", "¿cómo le pido que se vea como esta referencia?", "dirección de arte", "quiero que se vea caro", "se ve a IA".

También la carga `producto` cuando toca escribir el prompt de una foto. En `ugc`, `avatar` y `clonar-anuncio` entra cuando el prompt de imagen queda en tus manos; si el proveedor usa un flujo guiado que dicta su propio prompt (la skill `proveedores` lo indica), ese flujo manda y esta skill solo afina lo que te deje escribir.

## Antes de empezar

1. **`marca.json`.** La paleta, las palabras de estilo y la categoría salen de ahí. Si no existe, tienes dos caminos: armar uno mínimo con la persona en cinco preguntas, o decirlo una vez y seguir con criterio sobrio. La forma del archivo y las cinco preguntas, en `references/marca.md`.
2. **Para qué es la imagen.** Ficha de tienda, anuncio, carrusel, portada. Cambia el encuadre y cambia el aire que hay que dejar libre.
3. **Si hay un producto real de por medio**, su foto. Sin ella no hay fidelidad posible. Se sube al proveedor con la intención `media.subir`; no asumas que lee lo que te adjuntan en el chat.

## El proceso

### 1. Decide la imagen antes de escribirla

Antes de teclear, contesta tres cosas en tu cabeza: **qué sensación** tiene que dar, **dónde va a mirar primero** el ojo, y **qué NO** puede aparecer. Un prompt que se escribe sin haber contestado esto sale largo y sale genérico igual.

### 2. Escribe por bloques

Once bloques, en este orden. Qué va exactamente en cada uno, con vocabulario que sí funciona y ejemplos, en `references/bloques.md`:

**concepto · sujeto · composición · luz · fondo · materiales · lente · paleta · referencia de estilo · marcadores de calidad · qué evitar**

El orden importa: los modelos pesan más lo que va al principio. Lo innegociable va arriba.

No hacen falta los once siempre. Pero si tu prompt no tiene **luz** ni **composición**, no es un prompt, es una descripción, y una descripción devuelve el promedio.

### 3. Ancla el producto real

Si en la imagen va un producto que existe, hay una frase que no puede faltar y que casi todo el mundo olvida. Cómo se escribe y por qué funciona, en `references/bloques.md`, sección "El ancla de fidelidad". Sin ella, el modelo se siente con permiso de mejorar tu etiqueta, y una etiqueta mejorada es una etiqueta que no es la tuya.

Cuando hay foto real, la intención es `imagen.editar` (referencia + texto), no `imagen.generar`. Generar desde texto un producto que existe es pedirle al modelo que se lo invente.

### 4. Luz y lente: donde se gana

Es el par que más separa una imagen profesional de una amateur, y el que menos gente escribe. Los esquemas de luz con nombre, el vocabulario de lente y cómo se combinan, en `references/luz-y-lente.md`.

### 5. Di qué evitar

El bloque de exclusiones no es un extra. Es donde se corta lo que el modelo hace por inercia. La lista base, y por qué cada cosa está en ella, en `references/antipatrones.md`.

Para cualquier imagen que vaya a llevar texto encima, ahí va **sin texto, sin letras, sin logotipos, sin marcas de agua**. La tipografía horneada por un modelo no es de fiar y no se puede corregir.

### 6. Revisa antes de generar

`references/antipatrones.md` cierra con una lista de nueve cosas que revisar en el prompt antes de generar. Cuesta veinte segundos y ahorra generaciones. Después de generar, la misma lista sirve para juzgar el resultado sin autoengañarse.

### 7. Genera

Carga la skill `proveedores`: ella detecta qué hay conectado y elige la herramienta para la intención (`imagen.generar`, o `imagen.editar` si hay referencia). Si el proveedor permite consultar el costo sin generar (`costo.consultar`), menciónalo en una línea como nota y sigue. Si las instrucciones del proyecto o de la persona piden confirmar antes de gastar, mandan ellas. Si no hay proveedor conectado, entrega el prompt por bloques listo para pegar en cualquier herramienta.

El prompt bueno no cambia el precio, cambia cuántas veces lo pagas.

### 8. Si hay que repetir, no repitas a ciegas

Cuando una imagen se descarta, di **qué** falló y **qué renglón** del prompt cambias, y genera con el prompt cambiado. Volver a mandar lo mismo esperando otro resultado es la forma más cara de usar cualquier herramienta. Tras dos intentos fallidos de la misma pieza, para, explica qué falla y propón otro enfoque.

## Qué entregas

Cuando te piden solo el prompt: el prompt completo, listo para copiar, con los bloques separados por línea y una nota de una frase por bloque que diga por qué está esa decisión ahí. Esa nota es lo que hace que la persona aprenda y no vuelva a preguntar.

Cuando además generas, en `trabajo/<fecha>-direccion-de-arte-<slug>/`:

- Las imágenes.
- `prompts.md` — el prompt exacto de cada una, y qué cambiarías para la siguiente.
- `costo.md` (opcional) — si el proveedor dio el costo, qué se generó y cuánto fue. Es informativo.

## Reglas

- **Luz y composición son obligatorias.** Sin ellas devuelves el promedio de internet.
- **El producto real manda sobre lo bonito.** Si el modelo lo "mejoró", esa imagen no sirve.
- **Sin texto horneado** cuando la pieza vaya a llevar texto encima.
- **Nada de imitar el estilo de un artista vivo por su nombre.** Se describe la técnica: la luz, el grano, la paleta, el encuadre. Se llega al mismo lugar y no te llevas a nadie por delante.
- **No repitas a ciegas.** Cada intento nuevo cambia algo del prompt, y dices qué.
- **Esta skill no nombra herramientas ni modelos.** Habla en intenciones; la traducción la hace `proveedores`.
- Un prompt que no puedes explicar renglón por renglón todavía no está terminado.
