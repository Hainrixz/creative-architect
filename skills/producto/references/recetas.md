# Recetas de foto de producto

Cada receta es una combinación de encuadre, luz y fondo que resuelve un uso concreto. No son estilos para elegir por gusto: son herramientas, y usar la equivocada se nota.

## Primero, la foto de origen

Lo que le pides a la persona antes de subir nada. Una foto mala de origen no la arregla ningún modelo:

- **Producto completo en el cuadro**, sin que se le corte nada y sin que se le encime otra cosa.
- **Enfocado.** Si está movida, todo lo que venga después va a estar movido.
- **Luz pareja.** La de una ventana de día es mejor que la del foco del techo. Sin flash directo.
- **Fondo simple.** Una pared blanca, una mesa lisa. Lo que hay detrás va a desaparecer, pero un fondo confuso confunde también al modelo.
- **La etiqueta legible**, si el empaque tiene texto. Es lo que el modelo tiene que respetar.
- **Dos o tres ángulos** si los tienen. Sirven de referencia cruzada y salen mejores resultados.

Si la única foto que hay está fatal, dilo antes de gastar: "con esta foto el resultado va a inventar detalles del empaque; una tomada junto a una ventana, sin flash, cambia el resultado por completo".

## Las recetas

### 1. Packshot limpio
**Para:** ficha de tienda, catálogo, comparativa, cualquier lugar donde el producto tiene que verse y ya.
**Encuadre:** producto centrado, completo, un poco de aire alrededor, a la altura del ojo o ligeramente por encima.
**Luz:** difusa y grande, envolvente, de tipo softbox. Sombra suave y corta debajo, para que no flote.
**Fondo:** blanco o gris muy claro, plano, con degradado suavísimo.
**Lo que la arruina:** reflejos duros sobre la etiqueta, sombra dura y negra, producto cortado por el borde.

### 2. Lifestyle
**Para:** anuncios, redes, todo lo que tenga que dar ganas y no solo informar.
**Encuadre:** el producto en su ambiente natural, un poco descentrado, con algo desenfocado adelante o atrás para dar profundidad.
**Luz:** natural, lateral, de mañana o de tarde. Sombras largas, algo de contraste.
**Fondo:** un lugar real y coherente con quien compra: una cocina de verdad, un escritorio usado, una mesa de café.
**Lo que la arruina:** un ambiente de revista sueca cuando tu cliente vive en Guadalajara. Que el lugar se parezca al suyo.

### 3. En la mano
**Para:** dar escala y hacerlo deseable. Es la que mejor rinde en anuncios de Meta.
**Encuadre:** una mano sosteniéndolo, cerca de la cámara, producto nítido y el resto suave.
**Luz:** suave, frontal-lateral, que se le vea la textura a la piel y al empaque.
**Fondo:** desenfocado, sin competir.
**Lo que la arruina:** las manos son lo más difícil de generar. Revisa dedos, uñas y cómo agarra. Si algo se ve raro, se descarta: nadie confía en un producto que sostiene una mano de seis dedos.

### 4. Macro de textura
**Para:** cuando lo que vendes es la calidad del material, del acabado o del ingrediente.
**Encuadre:** muy cerca, llenando el cuadro con una parte del producto.
**Luz:** rasante, casi de lado, que es la que hace visible el relieve.
**Fondo:** apenas existe.
**Lo que la arruina:** demasiado enfoque en todo. En macro, el desenfoque es lo que hace que se vea macro.

### 5. Sobre bloque de color
**Para:** cuando la pieza va a llevar texto encima, o cuando quieres que grite tu marca en el feed.
**Encuadre:** producto centrado o al tercio, con **un tercio del cuadro deliberadamente vacío** para el titular.
**Luz:** de estudio, dura y direccional, con sombra marcada y con carácter.
**Fondo:** un color plano, sacado de `marca.json` (el primario o el acento).
**Lo que la arruina:** llenar el cuadro. El vacío es el trabajo.

### 6. En contexto de uso
**Para:** explicar para qué sirve, sin decirlo.
**Encuadre:** el producto en el momento exacto en que se usa. Vertiéndose, aplicándose, abriéndose.
**Luz:** la del lugar donde eso pasa de verdad.
**Fondo:** el sitio real: el baño, la cocina, el gimnasio.
**Lo que la arruina:** un momento imposible. Si el gesto no se puede hacer con ese producto, se nota al instante.

## Proporciones

Cada proveedor acepta sus propias proporciones y lo dice su archivo en `proveedores`. Qué proporción pide cada red vive en `formatos-por-red` del plugin `publicar` (opcional, si lo tienes), que es donde se actualiza cuando Instagram cambie de opinión; no copies esa tabla aquí. Lo único que se decide en esta skill es que la imagen se genere ya en la proporción final, y que si te la piden en dos, ensanchar una con `imagen.expandir` suele salir más barato y más consistente que generar dos veces.

## El retoque de después

| Qué necesitas | Intención | Nota |
|---|---|---|
| Producto recortado sobre transparente | `imagen.quitar_fondo` | Lo que pide una ficha de tienda |
| Misma foto, otra proporción | `imagen.expandir` con la proporción destino | Rellena los bordes en vez de recortar el producto |
| Resolución para impresión | `imagen.escalar` a 2K o 4K | Algunos proveedores piden el ancho y el alto reales de la imagen de origen; `proveedores` lo dice |

Las tres son generaciones. Si el proveedor permite consultar su costo, va como nota informativa junto con el resto; si no lo permite, se sigue igual.

## La revisión final, contra la foto original

Antes de entregar, pon la foto real al lado de la generada y revisa en este orden:

1. **El color.** ¿Es el mismo? Los modelos tienden a saturar.
2. **La etiqueta.** ¿Dice lo que dice? ¿Sobra o falta una letra?
3. **La forma.** ¿La tapa, el cuello, las proporciones son las mismas?
4. **El logo.** ¿Está completo y bien escrito?
5. **Lo imposible.** Reflejos que no cuadran, sombras hacia dos lados, un producto flotando sin apoyo.

Si algo de esto falla, esa imagen no es de ese producto y no se entrega. Se dice qué salió mal, se hace el ajuste concreto del prompt (y se dice cuál) antes de repetir. Generar otra vez lo mismo no es un ajuste. Tras dos intentos fallidos de la misma pieza, para, explica qué falla y propón otro enfoque.
