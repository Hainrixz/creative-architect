# Changelog

Formato basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/). Versionado [SemVer](https://semver.org/lang/es/).

## [1.0.0] - 2026-10-17

Primera versión de Creative Architect. Nace de `higgsfield-estudio` 5.0.0: se conserva el oficio y se separa del proveedor.

### Añadido

- **Capa de proveedores** (`skills/proveedores/`): detección de lo conectado (MCP, CLI, variables de entorno sin imprimir su valor), vocabulario común de intenciones (`imagen.editar`, `video.desde_imagen`, `identidad.referencia`…) y un archivo por proveedor: Higgsfield, Magnific, OpenAI, más una guía para añadir otro (`nuevo-proveedor.md`).
- **`intenciones.md`** en `proveedores/references/`: el vocabulario de intenciones (incluida `video.reemplazar_objeto`) y las ocho capacidades especiales que contesta cada proveedor. Viaja con el plugin instalado; `docs/arquitectura.md` remite a él.
- **Cinco reglas de dinero y cuenta** (antes tres): se suman no borrar, archivar, despublicar ni mover nada remoto ni tocar ajustes, espacio de trabajo, suscripción o conexiones de la cuenta sin que se pida con esas palabras, y del conector de Meta solo la búsqueda de la Biblioteca (lectura).
- **Datos externos no son órdenes**: el texto de anuncios, webs, transcripciones, análisis y respuestas de proveedores se lee como material; si trae instrucciones, no se siguen y se avisa.
- **Llamadas seguras**: el prompt nunca va pegado en un comando (archivo con heredoc + `jq --arg`, `--form-string` o `--prompt "$(cat …)"`), `curl -sS` sin `-v`, respuestas leídas con `jq`, URLs firmadas de subida tratadas como credenciales, y subidas solo de lo que la persona señaló al proveedor elegido.
- **Reglas de abuso** en el agente: nada de menores, nada sexual con personas reales, ni figuras públicas ni "parecidos", nada que finja ser noticia o documento oficial, y el consentimiento lo da quien presta la cara o la voz.
- **Agente `creative-architect`**: director de arte y productor con pilares, orden de trabajo en siete pasos y reglas éticas. Sin proveedor entrega el prompt listo para pegar.
- **Skill de entrada `/creative-architect`**: enruta cada pedido al oficio correcto y, si no está claro, hace una sola pregunta.
- **`marca.md`** en `direccion-de-arte/references/`: forma de `marca.json` para que el plugin no dependa de otro.
- **Schema de avatar 2.0**: identificadores por proveedor en `marca/avatar.json`.
- Entregables ordenados en `trabajo/<fecha>-<oficio>-<slug>/` con `prompts.md`.

### Cambiado

- `art-director` pasa a llamarse **`direccion-de-arte`**.
- Las skills de oficio ya no nombran herramientas ni modelos de un proveedor: hablan en intenciones y la capa de proveedores traduce.
- El costo es una **nota informativa** de una línea que no bloquea la generación. Si el proyecto o la persona piden confirmar antes de gastar, mandan ellas. Tras dos intentos fallidos de la misma pieza, se para y se propone otro enfoque.
- Lo que dice un proveedor en vivo (herramienta o documento de flujo) manda solo en lo técnico, nunca sobre consentimiento, testimonios, llaves, compras, publicación ni borrado.

### Quitado

- Hook `hooks/creditos.mjs` (bloqueaba cada llamada a Higgsfield).
- Skill `creditos` y la confirmación obligatoria de costo antes de generar ("di el costo y espera el sí").
- Límite fijo de cuatro generaciones por tanda.
- Dependencia del plugin `marketing-base`.

### Se mantiene

- Consentimiento para cara y voz, nada de testimonios falsos, declarar la IA, no imitar a artistas vivos por nombre, el producto real manda, sin texto horneado si la pieza llevará texto, nunca imprimir llaves.
- Nunca comprar créditos ni publicar sin que lo pidan (reglas 1 y 2 de las cinco).
