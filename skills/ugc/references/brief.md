# El brief de un UGC

Todo de una sola vez, no a cuentagotas. Lo que no te contesten, lo asumes razonablemente, lo dices en una línea y lo dejas anotado en los supuestos del entregable.

## Las siete preguntas

1. **¿Qué producto o servicio es, y qué hace por quien lo compra?** No la ficha técnica: el resultado. "Una crema" no sirve; "una crema para manos partidas de tanto lavarlas" sí.
2. **¿Qué se puede afirmar de él, y qué no?** Esta es la pregunta que nadie hace y la que salva la cuenta. Si el producto no está certificado para curar nada, el video no dice que cura nada. Anota lo permitido y lo prohibido palabra por palabra.
3. **¿Dónde se publica?** TikTok, Reels, Shorts, feed de Instagram, anuncio de Meta. Cambia la proporción, la duración y el ritmo del gancho.
4. **¿Cuánto dura?** Si no lo saben: 15-20 segundos para orgánico, 15-30 para anuncio. Una duración sin decidir es un hueco del brief, no un detalle.
5. **¿Quién sale?** El avatar de marca que ya existe (`marca/avatar.json`, lo arma `avatar`), un creador generado nuevo (adulto), o nadie en cámara. Si es una persona real, consiente ella en la conversación; "tengo su permiso" de otro no basta. Si es un creador nuevo cada vez, dilo: el contenido va a parecer de tres marcas distintas.
6. **¿Hay material propio?** Foto del producto, video de origen, logo. Todo lo que exista se sube con `media.subir` y se usa como referencia. Una foto real del producto vale más que cualquier prompt.
7. **¿Qué tiene que pasar al final?** Un solo objetivo: que guarden, que comenten, que entren al link, que compren. Uno, no tres.

## Lo que decides tú si no te lo dicen

| Hueco | Qué asumes | Y lo anotas |
|---|---|---|
| Duración | 15-20 s | "asumí 15 s, dime si lo quieres más largo" |
| Proporción | 9:16 vertical | "vertical, que es lo que pide Reels y TikTok" |
| Idioma | español neutro de LatAm | según `marca.json`, si existe |
| Tono | el de `marca.json` | si no hay marca, sobrio y directo |
| Gancho | el problema de quien mira, no el nombre de la marca | "empecé por el dolor, no por el producto" |

## El gancho, que es lo único que importa de verdad

Los primeros 3 segundos deciden el video entero. Lo demás es ejecución.

Lo que sirve como primera frase:
- El problema dicho como lo diría la persona, no como lo diría el departamento de marketing.
- Un resultado concreto y raro. "Dejé de gastar 4 mil pesos al mes en esto" retiene más que "ahorra dinero".
- Una creencia común atacada de frente. "Lavarte las manos más veces no es el problema."
- Media historia. Empiezas a mitad y el cerebro se queda a cerrarla.

Lo que no sirve nunca:
- El nombre de la marca en la primera frase.
- "¿Sabías que...?"
- Una pregunta que se contesta con sí o no y a nadie le importa.
- Cualquier cosa que empiece con "en la era digital" o parientes.

Escribe tres ganchos, quédate con uno y deja los otros dos en el entregable. Si los tres suenan igual de tibios, el problema es el ángulo, no la redacción.

## La honestidad no es un trámite

Un creador generado por IA puede **mostrar** un producto, **describirlo** y **leer** un texto que la marca escribió y respalda. No puede:

- Decir que lo lleva usando X tiempo.
- Contar un resultado que no le pasó a nadie.
- Presentarse como cliente cuando no existe.
- Aparentar ser una persona real identificable que no dio permiso.
- Ser o parecerse a una figura pública o a un político, ni ponerle dichos o respaldos que no hizo.
- Ser un menor, ni usar la cara o la voz de uno (aunque lo pida su padre o su madre): los creadores inventados son adultos.
- Salir en algo sexual o desnudo si es una persona real o se le parece.
- Fingir que el video es una noticia o un comunicado oficial.

No es escrúpulo de más: un testimonio inventado es publicidad engañosa en casi cualquier jurisdicción, y en Meta y TikTok tumba la cuenta publicitaria. Si lo que la persona quiere es exactamente eso, díselo de frente y ofrécele la versión que sí se puede: demostración del producto, la promesa de la marca en boca de un presentador, y el testimonio real de un cliente real si lo tiene.
