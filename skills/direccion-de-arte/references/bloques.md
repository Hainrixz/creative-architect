# Los once bloques de un prompt de imagen

El orden importa: lo que va al principio pesa más. Lo innegociable arriba.

---

## 1. Concepto

Una frase que dice qué es la imagen y para qué sirve. No es decoración: es lo que mantiene coherentes a los otros diez bloques.

> Foto publicitaria de producto para anuncio de Instagram, cálida y apetecible.

Si esta frase no existe, cada bloque tira para su lado y el resultado se siente cosido.

## 2. Sujeto

Qué es lo que se ve, con la concreción de quien lo tiene delante. Material, color, tamaño, estado.

- Flojo: "un frasco de café".
- Firme: "un frasco de vidrio ámbar de 250 g, con etiqueta de papel kraft mate y tapa de metal negro, medio lleno".

Si el sujeto es un producto real, aquí va también su referencia visual (la foto real, subida al proveedor con `media.subir` y pasada como referencia en `imagen.editar`) y el ancla de fidelidad de más abajo.

## 3. Composición

Dónde está cada cosa y desde dónde se mira. El bloque que más gente olvida y el que más cambia el resultado.

- **Encuadre:** primer plano, plano medio, plano general, macro.
- **Ángulo:** a la altura del ojo, picado (desde arriba), contrapicado (desde abajo, hace que el sujeto imponga), cenital (justo desde arriba, para acomodar objetos sobre una superficie).
- **Colocación:** centrado (simétrico, formal), al tercio (dinámico), descentrado con aire a un lado (deja sitio para el texto).
- **Profundidad:** algo desenfocado adelante, algo detrás. Es lo que da sensación de espacio real.
- **El vacío:** dilo explícito. "Un tercio superior del cuadro vacío, con fondo limpio" es la frase que salva un carrusel.

## 4. Luz

El bloque donde se gana o se pierde el aire profesional. Los esquemas con nombre y cuándo usar cada uno, en `luz-y-lente.md`.

Lo mínimo que tiene que decir: **de dónde viene**, **qué tan dura es** y **de qué color es**.

> Luz lateral suave desde la izquierda, tipo ventana grande, con relleno tenue a la derecha. Sombras largas y limpias. Temperatura cálida, de última hora de la tarde.

## 5. Fondo

Qué hay detrás y cuánto se ve. Tres familias:

- **Plano:** un color liso. Limpio, comercial, deja sitio para el texto.
- **Con textura:** mármol, madera, lino, cemento. Da tacto sin robar atención.
- **Ambiente:** un lugar real. Cuenta una historia y compite por la mirada, así que se desenfoca.

Regla: el fondo nunca puede tener más contraste que el sujeto. Si compite, gana el fondo y pierdes tú.

## 6. Materiales y acabados

Cómo se siente lo que se ve. Es el bloque que separa una imagen plana de una que da ganas de tocar.

Vocabulario que sí funciona: mate, satinado, brillante, esmerilado, texturado, cepillado, granulado, aterciopelado, húmedo, empañado, con condensación, con polvo fino, con huellas de uso.

> Vidrio esmerilado con condensación en la parte baja, metal negro mate, papel kraft con textura visible.

## 7. Lente

El vocabulario de cámara le dice al modelo qué tipo de imagen imitar. Detalle en `luz-y-lente.md`.

En corto: un lente corto (24 mm) exagera y mete contexto; uno normal (50 mm) ve como el ojo; uno largo (85, 100 mm) aplana, separa del fondo y favorece. Para producto, entre 85 y 100 mm casi siempre. Para retrato, 85 mm. Para macro, 100 mm macro.

La apertura controla el desenfoque: f/1.8 desenfoca mucho, f/8 mantiene casi todo nítido, f/16 lo mantiene todo.

## 8. Paleta

Los colores, y **de dónde salen**: del bloque `visual.colores` de `marca.json`. Se escriben por nombre y relación, no solo por código.

> Paleta dominada por el coral de la marca sobre crema, con un acento de negro profundo. Sin verdes.

Decir qué colores **no** quieres es tan útil como decir cuáles sí. Los modelos meten azul y teal por defecto en cuanto los dejas.

## 9. Referencia de estilo

Cómo se ve, en términos de técnica y no de nombres propios.

- Lo que sí: "estilo de fotografía editorial de revista de gastronomía, grano fino de película, contraste medio, colores algo desaturados".
- Lo que no: el nombre de un fotógrafo o de un ilustrador vivo. Se describe lo que hace, no a quién se lo copias. Se llega al mismo sitio sin llevarse a nadie por delante.

Si tienen una imagen de referencia, mejor que cualquier frase: se sube y se pasa como referencia visual.

## 10. Marcadores de calidad

Van al final y son cortos. Una lista larga de superlativos no mejora nada, solo diluye lo anterior.

> Fotografía profesional de producto, iluminación de estudio, alto detalle, enfoque nítido en la etiqueta.

Lo que **no** funciona: apilar "8k, ultra HD, obra maestra, hiperrealista, trending". Eso es folclore de foro. Lo que sí funciona es decir **dónde** tiene que estar el detalle.

## 11. Qué evitar

El bloque de exclusiones. La lista base y su razón, en `antipatrones.md`.

El mínimo, siempre:

> Sin texto, sin letras, sin logotipos, sin marcas de agua, sin manos deformes, sin reflejos imposibles, sin fondo desordenado.

---

## El ancla de fidelidad al producto real

Esta es la frase que separa una foto **de tu producto** de una foto de un producto parecido al tuyo. Va junto al bloque de sujeto, y va siempre que haya una foto real de por medio.

> El producto de la imagen de referencia se reproduce **exactamente**: misma forma, mismo color, misma etiqueta, mismo texto de la etiqueta, mismas proporciones, mismo logotipo. No se rediseña, no se mejora, no se estiliza, no se le cambia el tipo de letra. Lo único que cambia es el entorno, la luz y el encuadre.

Por qué hace falta: el modelo entiende "foto bonita de este frasco" como permiso para hacer el frasco más bonito. Y lo hace: le limpia la etiqueta, le cambia la tipografía por una más elegante, le corrige el color. El resultado es precioso y no es tu producto, y eso se descubre cuando al cliente le llega la caja.

Después de generar, la comparación con la foto original es obligatoria: color, etiqueta, forma, logo. Si cambió algo, esa imagen se descarta.

---

## Ejemplo completo

Mismo producto, dos prompts.

**El genérico:**

> Una foto profesional de un frasco de café sobre una mesa.

Devuelve: frasco centrado, luz plana de frente, fondo de madera anónima, todo enfocado. La foto de banco que ya viste.

**Por bloques:**

> Foto publicitaria de producto para anuncio de Instagram, cálida y apetecible.
> Un frasco de vidrio ámbar de 250 g con etiqueta de papel kraft mate y tapa de metal negro. El producto de la referencia se reproduce exactamente: misma forma, mismo color, misma etiqueta y mismo logotipo. Solo cambian el entorno, la luz y el encuadre.
> Plano medio, ligeramente por encima de la altura del ojo, frasco al tercio derecho, tercio izquierdo vacío para el titular. Granos de café desenfocados en primer término.
> Luz lateral suave desde la izquierda, tipo ventana grande, relleno tenue a la derecha. Sombra larga y limpia hacia la derecha. Temperatura cálida de última hora de la tarde.
> Fondo de cemento pulido claro, desenfocado, sin objetos.
> Vidrio con brillo suave, metal negro mate, papel kraft con textura visible, un poco de polvo de café sobre la superficie.
> Lente de 85 mm a f/2.8. Enfoque nítido en la etiqueta, fondo suave.
> Paleta cálida: ámbar, crema y negro profundo. Sin azules ni verdes.
> Estilo de fotografía editorial de gastronomía, grano fino, contraste medio.
> Fotografía profesional de producto, iluminación de estudio, alto detalle en la etiqueta.
> Sin texto añadido, sin letras, sin logotipos ajenos, sin marcas de agua, sin reflejos imposibles.

Mismo modelo, mismo costo. La diferencia son las decisiones.
