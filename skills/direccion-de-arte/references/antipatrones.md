# Lo que hace que una imagen se vea a IA

Nadie sabe explicar por qué una imagen "se ve generada", pero todo el mundo lo detecta en medio segundo. Son unos pocos rasgos, y casi todos vienen de lo mismo: el modelo tomó por ti las decisiones que no tomaste.

## Los ocho delatores

**1. Todo está perfecto.** Ni una arruga, ni una mota de polvo, ni una huella. El mundo real tiene desgaste. Pide imperfección explícita: "una huella tenue en el vidrio", "el mantel con arrugas de uso", "un poco de polvo de café sobre la mesa".

**2. Simetría de más.** Todo centrado, todo equilibrado, todo mirando al frente. Pide asimetría: el sujeto al tercio, algo saliéndose del cuadro, un objeto de apoyo que rompa el eje.

**3. Luz de ninguna parte.** El sujeto iluminado por igual desde todos lados, sin una sombra que diga de dónde viene la luz. **Es el delator número uno.** Se arregla escribiendo el bloque de luz.

**4. Todo enfocado.** Ninguna cámara real deja nítido el primer término, el sujeto y el fondo a la vez. Elige un plano de enfoque y dilo: "enfoque nítido en la etiqueta, fondo suave".

**5. Saturación de más.** Colores un punto más vivos de lo que existe. Pide "colores algo desaturados, contraste medio" y el resultado gana realismo de golpe.

**6. Texto que no es texto.** Letras que se parecen a letras pero no dicen nada. No se puede corregir: está pintado. Por eso las imágenes que llevan texto encima se piden **sin texto**.

**7. Manos y dedos.** El clásico. Cuenta los dedos, mira cómo agarra el objeto, mira las uñas. Si hay algo raro, se descarta la imagen.

**8. La cara de nadie.** Simétrica de más, piel de anuncio de crema, ojos vidriosos. Para retratos, pide "piel real con textura y poros visibles, sin retoque, iluminación natural".

## La lista base de exclusiones

Se copia al bloque "qué evitar" y se le añade lo específico de cada imagen.

> Sin texto, sin letras, sin palabras, sin logotipos ajenos, sin marcas de agua, sin firmas.
> Sin manos ni dedos deformes, sin extremidades de más.
> Sin luz plana ni sin dirección, sin sombras que se contradigan, sin reflejos imposibles.
> Sin colores sobresaturados, sin ese aspecto de render 3D pulido.
> Sin fondo desordenado, sin objetos que compitan con el sujeto.
> Sin marcos ni bordes ni collages.

## Añade según el caso

| Si estás haciendo | Añade a las exclusiones |
|---|---|
| Imagen para carrusel o pieza con texto encima | Sin texto de ningún tipo, y deja un tercio del cuadro limpio |
| Foto de producto | Sin rediseñar la etiqueta, sin cambiar el tipo de letra, sin alterar el color del empaque |
| Retrato o avatar | Sin piel de plástico, sin ojos vidriosos, sin simetría perfecta de la cara |
| Comida | Sin brillos de laca, sin porciones imposibles, sin colores de caricatura |
| Lifestyle | Sin gente sonriendo a la cámara sin motivo, sin ambiente de catálogo sueco |

## Lo que NO sirve de nada

Folclore que circula en foros y que solo diluye lo importante:

- Apilar "8k, ultra HD, hiperrealista, obra maestra, premiada, trending". Los modelos actuales no lo necesitan y quita peso a lo que sí decidiste.
- Nombrar a un fotógrafo o ilustrador vivo. Además de ser un problema con su trabajo, funciona peor que describir la técnica: "grano fino de película, contraste medio, colores desaturados" es más preciso que un apellido.
- Escribir párrafos larguísimos. Pasado cierto punto, lo del final ya no pesa. Once bloques bien escritos ganan a trescientas palabras.
- Repetir la misma idea con tres sinónimos. No la refuerza, la enturbia.

## La revisión de los veinte segundos

Antes de generar, pasa el prompt por esta lista. Cuesta menos que una generación de más.

1. ¿Dice de dónde viene la luz?
2. ¿Dice qué tan dura es?
3. ¿Dice dónde está el sujeto en el cuadro?
4. ¿Dice qué está enfocado y qué no?
5. Si hay un producto real, ¿está el ancla de fidelidad?
6. Si la pieza lleva texto encima, ¿está pedido sin texto y con el aire libre?
7. ¿Los colores salen de `marca.json`? (Si no hay marca, ¿los decidiste tú en vez de dejárselos al modelo?)
8. ¿Hay bloque de exclusiones?
9. ¿Puedes explicar cada renglón? Si hay uno que está "porque suena bien", quítalo.

## Y después de generar

La misma lista, mirando el resultado y sin autoengañarse. Se descarta, no se entrega "porque casi", si:

- La luz no viene de ningún lado.
- El producto cambió de color, de forma, de etiqueta o de logo.
- Hay letras inventadas.
- Hay manos raras.
- Se ve más limpio que la realidad.

Cuando descartes, di **qué** falló y **qué renglón** del prompt vas a cambiar, cámbialo y entonces generas. Repetir lo mismo esperando otro resultado es la forma más cara de usar cualquier herramienta.
