# marca.json: lo mínimo para que las imágenes sean tuyas

Sin marca, las imágenes salen bonitas y ajenas. Con cinco datos bien puestos, la paleta, el tono y lo que no se dice dejan de ser una apuesta del modelo y pasan a ser una decisión tuya.

`marca.json` es **opcional**. Si existe, se usa. Si no, el agente puede armar uno mínimo con la persona o seguir con criterio sobrio. No es requisito para trabajar.

## Dónde vive

En la raíz del proyecto de trabajo: `marca.json`. El personaje persistente, si lo hay, va aparte en `marca/avatar.json` (eso es cosa de la skill `avatar`).

## La forma mínima

```json
{
  "nombre": "Café Ámbar",
  "categoria": "café de especialidad, venta en línea",
  "tono": "cálido, directo, sin pretensión; habla de tú",
  "palabrasProhibidas": ["gourmet", "premium", "el mejor café del mundo"],
  "idioma": "es-MX",
  "visual": {
    "colores": [
      { "nombre": "ámbar",        "hex": "#C8782A", "rol": "primario" },
      { "nombre": "crema",        "hex": "#F3EAD9", "rol": "fondo" },
      { "nombre": "negro profundo","hex": "#1B1712", "rol": "acento" }
    ],
    "estilo": "fotografía editorial de gastronomía, luz natural lateral, grano fino, colores algo desaturados, superficies reales con uso"
  }
}
```

| Campo | Para qué lo usa la dirección de arte |
|---|---|
| `nombre` | Nombrar los entregables y no confundir marcas. Nunca se hornea en la imagen |
| `categoria` | Elegir receta y esquema de luz (comida pide luz suave; tecnología aguanta dura) |
| `tono` | El bloque de concepto y el ambiente: cercano, serio, lujoso, juguetón |
| `palabrasProhibidas` | Lo que no puede aparecer en copys ni en ningún texto que acompañe la pieza |
| `idioma` | El idioma de cualquier texto que se ponga encima después |
| `visual.colores` | El bloque de paleta. Se escriben **por nombre y relación** ("ámbar sobre crema, acento negro"), no solo el hex. El `rol` dice cuál domina: `primario`, `fondo`, `acento`, `texto` |
| `visual.estilo` | El bloque de referencia de estilo, ya descrito como técnica y no como nombre propio |

Se le pueden añadir más campos (tipografías, público, competencia). Las skills de oficio ignoran lo que no usan.

## Si no existe: las cinco preguntas

Cuando la pieza importa (va a un anuncio, a la tienda, a una serie de carruseles), vale la pena armarlo antes de generar. Cinco preguntas, en este orden, y con las respuestas escribes el archivo:

1. **¿Cómo se llama la marca y qué vende?** → `nombre`, `categoria`.
2. **Si tu marca fuera una persona, ¿cómo hablaría?** Tres adjetivos bastan. → `tono`.
3. **¿Qué palabras o promesas no quieres ver nunca junto a tu marca?** → `palabrasProhibidas`.
4. **¿Cuáles son tus colores?** Si tiene logo o web, saca de ahí los hex; si no, pide dos o tres colores con nombre y decide tú el rol. → `visual.colores`.
5. **Enséñame una imagen que sientas tuya y una que odies.** De la primera describes la técnica (luz, grano, paleta, encuadre); de la segunda sacas exclusiones. → `visual.estilo`.

El `idioma` se deduce de cómo te escribe la persona; confírmalo en una línea.

Si la persona no quiere contestar o es una pieza de prueba, no insistas: dilo una vez ("sin marca, voy con paleta sobria y luz natural") y sigue con criterio sobrio.

## Si tienes el plugin marketing-base

Si en la sesión está disponible `/marketing-base:marca`, puedes usarlo para crear o completar `marca.json` con su propio cuestionario. No es dependencia: todo lo de arriba funciona sin él.
