# Costo: cómo se informa en cada proveedor

El costo es una nota, no una puerta. No hay hook, no hay bloqueo y no se espera un "sí" para generar. Si el proveedor deja saber el precio sin generar, se dice en una línea y se sigue. Si no deja, se genera igual.

**Aquí no hay tabla de precios, y es a propósito.** Los precios por modelo cambian sin avisar y un número escrito en este archivo sería mentira dentro de un mes. Lo estable es *cómo* preguntar.

## Cómo se dice

Una línea, con lo que se sepa:

> Esto cuesta ~40 créditos en Higgsfield.

> OpenAI no deja consultar el precio antes; se cobra a tu cuenta de API según tamaño y calidad.

Y nada más. Sin pregunta al final, sin esperar respuesta.

**Nunca inventes un número** ni lo deduzcas de una generación parecida. Si no se puede consultar, se dice que no se puede.

## Por proveedor

### Higgsfield

- **Consultar sin generar:** `get_cost: true` dentro de `params` en `generate_image`, `generate_video`, `generate_audio`, `reframe` (con `duration_seconds` y `resolution`), `upscale_image` y `outpaint_image`. No lanza nada ni cobra.
- **CLI:** `higgsfield generate cost <modelo> --param valor …` y `higgsfield generate cost workflow <nombre> …`. `higgsfield workflow get <nombre>` dice qué parámetros cuentan para el costo.
- **No dejan consultar:** las tandas (`generate_*_batch`), `upscale_video`, `remove_background`, `motion_control`, `dubbing`, `voice_change`, `video_analysis_create`, `virality_predictor`, entrenar un Soul, crear un Elemento y `create_voice`.
- **Tanda:** si quieres dar una cifra, consulta un solo ítem con `get_cost` y multiplica. Di que es estimación.
- **Saldo:** `balance` o `higgsfield account status`. Movimientos: `transactions` o `higgsfield account transactions`.
- **Qué mueve el precio:** duración y resolución en video; `count` (cada variante se cobra); `fps` por encima de 30 en `upscale_video` con `bytedance` duplica; `motion_control` en `1080p` cuesta más que en `720p`.
- **Cupos ilimitados o de prueba:** omite `use_unlim` y `use_free_gens`. Si la persona tiene cupo que cubre el modelo, el servidor devuelve `unlim_choice` sin lanzar nada: es la pregunta para ella. Ese es el único caso en que se espera su respuesta, porque el cupo es suyo.

### Magnific

- **Consultar sin generar:** no documentado (sin verificar). Si el esquema de una herramienta del MCP trae algo así, úsalo como nota.
- **Saldo:** `account_balance` (MCP). Uso por proyecto: `project_report`.
- **Qué mueve el precio:** modelo y resolución, según su documentación.
- El MCP y la app comparten el mismo saldo.

### OpenAI

- **Consultar sin generar:** no existe.
- **Saldo:** no hay endpoint en este flujo; la persona lo ve en su panel de uso.
- **Qué mueve el precio:** `quality` (`xhigh` y `max` son las caras), `size` y `n` en imagen; largo del texto en voz.

## Saldo antes y después

Es **opcional y solo informativo**. Sirve si la persona pregunta "¿cuánto se fue?" o si quieres dejarlo en `costo.md`. Nunca es un paso previo a generar.

## `costo.md` en la carpeta de trabajo

Si hubo gasto, una nota corta en `trabajo/<fecha>-<oficio>-<slug>/costo.md`:

- Qué se generó (pieza, modelo, proveedor).
- Lo que dijo el proveedor del costo, si lo dijo, o "sin consulta previa".
- Saldo antes y después, si se miró.

Sin precios de memoria.

## Lo que sí no se hace

1. **No comprar créditos, planes ni recargas por la persona.** Si el saldo se acaba a media tarea, para, dilo y entrega el enlace de compra del proveedor tal cual (en Higgsfield, el panel de `show_plans_and_credits`). La compra la confirma ella.
2. **No publicar en abierto si no lo pidió.**
3. **No gastar cupos gratuitos o ilimitados sin que lo diga.**
4. **No borrar, archivar, despublicar ni mover nada remoto, ni cambiar ajustes, espacio de trabajo, suscripción o conexiones de la cuenta**, si no lo pidió con esas palabras.
5. **Del conector de Meta, solo la búsqueda de la Biblioteca (lectura).**

Las cinco completas están en el `SKILL.md` de esta skill.

Si las instrucciones del proyecto o de la persona piden confirmar antes de gastar, mandan ellas.

Si una generación sale mal, no la repitas a ciegas: cada reintento vuelve a cobrar. Cambia el prompt, di qué cambiaste y genera otra vez. Tras dos intentos fallidos de la misma pieza, para, explica qué falla y propone otro enfoque; no reintentes en bucle.
