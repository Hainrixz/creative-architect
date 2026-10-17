<div align="center">

# Creative Architect

**Tu director de arte y productor de imagen y video con IA, dentro de Claude Code.**

[![Versión](https://img.shields.io/badge/versión-1.0.0-0a7cff)](CHANGELOG.md)
[![Licencia MIT](https://img.shields.io/badge/licencia-MIT-green)](LICENSE)
[![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-d97757)](https://code.claude.com/docs/en/plugins)
[![Idioma](https://img.shields.io/badge/idioma-español-c60b1e)](#)
[![Proveedores](https://img.shields.io/badge/proveedores-Higgsfield%20·%20Magnific%20·%20OpenAI-555)](#proveedores-soportados)

</div>

Creative Architect convierte un pedido en español ("fotos de mi producto para Meta", "un UGC de 15 segundos", "anima esta imagen") en piezas de campaña, no en imágenes de banco.
Decide la imagen antes de escribirla: sensación, dónde mira el ojo, luz, lente y qué no debe aparecer.
Y la genera en el proveedor que tengas conectado, o te entrega el prompt listo para pegar si no tienes ninguno.

---

## De dónde viene

Creative Architect es la evolución de **`higgsfield-estudio` v5.0.0**, el plugin de Enrique Rocha para hacer UGC, avatares, fotos de producto y anuncios con Higgsfield.

Ese plugin tenía dos cosas mezcladas: **oficio** (cómo se escribe un prompt que no sale genérico, cómo se respeta un producto real, cómo se arma un gancho) y **proveedor** (nombres de herramientas, modelos y un candado de créditos atado a Higgsfield). El oficio no depende de la herramienta, así que se separó:

- Las skills de oficio hablan en **intenciones** ("editar una imagen con la foto real como referencia", "video desde imagen").
- Una capa de **proveedores** traduce cada intención a la herramienta concreta de Higgsfield, Magnific, OpenAI o la que añadas.

Resultado: el mismo criterio sirve hoy con un proveedor y mañana con otro, sin reescribir nada.

## Qué incluye

Un agente y ocho skills.

| Pieza | Qué hace |
|---|---|
| Agente `creative-architect` | Director de arte y productor. Detecta proveedores, lee tu marca, enruta al oficio, escribe el prompt, genera, revisa y entrega. |
| `/creative-architect` | Punto de entrada. Manda tu pedido al oficio correcto; si no está claro, pregunta una sola cosa. |
| `direccion-de-arte` | El prompt por bloques que sale profesional: luz y lente, antipatrones, ancla de fidelidad, forma de `marca.json`. |
| `producto` | De la foto del celular a la foto de catálogo o creativo de anuncio, sin inventar el producto. |
| `avatar` | El mismo personaje en todos tus videos: misma cara, misma voz, con consentimiento. |
| `ugc` | Videos con una persona hablando de tu producto: brief, gancho, formatos (testimonio, unboxing, try-on). |
| `animar` | Le da movimiento a una imagen y la deja en el formato de la red donde va. |
| `clonar-anuncio` | Busca lo que corre tu competencia en la Biblioteca de Anuncios de Meta y te quedas con su estructura, no su contenido. |
| `proveedores` | Detecta qué tienes conectado y traduce cada intención a la herramienta de cada proveedor. |

## Proveedores soportados

| Proveedor | Vía | Fuerte en |
|---|---|---|
| **Higgsfield** | MCP (conector de claude.ai) y CLI `higgsfield` | Video, UGC, identidades persistentes, voz, edición con referencia |
| **Magnific** (antes Freepik) | MCP oficial (OAuth: `claude mcp add --transport http magnific https://mcp.magnific.com`) o API REST con `MAGNIFIC_API_KEY` (acepta `FREEPIK_API_KEY`); reiluminar/expandir/transferir estilo solo por REST | Escalado, reiluminado y detalle de imagen |
| **OpenAI** | API con `OPENAI_API_KEY` en una variable de entorno | Imagen (generar y editar, con texto preciso) y voz en off. Sin video: la API de Sora se apagó el 2026-09-24 |

Cuando hay varios conectados, el agente elige por intención según la tabla de fortalezas de `skills/proveedores/SKILL.md`, o usa el que le pidas ("hazlo con Magnific").

**¿Otro proveedor?** Sigue `skills/proveedores/references/nuevo-proveedor.md`: un archivo que mapea cada intención a sus herramientas. Las skills de oficio no se tocan.

## Instalación

Requisitos: [Claude Code](https://code.claude.com) con soporte de plugins y `git`.

### Opción A: marketplace desde GitHub

El repositorio es **privado**. Claude Code clona con el `git` de tu máquina, sin pedir contraseña, así que necesitas acceso de lectura al repo y una credencial ya guardada: una llave SSH cargada en `ssh-agent`, o por HTTPS `gh auth login` seguido de `gh auth setup-git`. Si no tienes llave SSH de GitHub, exporta `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` antes de abrir Claude Code.

Dentro de Claude Code:

```
/plugin marketplace add Hainrixz/creative-architect
/plugin install creative-architect@creative-architect
```

### Opción B: local, clonando el repo

```bash
git clone https://github.com/Hainrixz/creative-architect.git ~/creative-architect
```

Y en Claude Code:

```
/plugin marketplace add ~/creative-architect
/plugin install creative-architect@creative-architect
```

Instalado desde una ruta local, el plugin lee tus archivos en el sitio: un `git pull` y `/reload-plugins` bastan para actualizar.

### Conecta al menos un proveedor

Ninguna llave va en este repo ni en tus archivos de trabajo. El plugin solo comprueba que existan; nunca las imprime.

- **Conector o MCP.** Para Higgsfield: activa el conector en la configuración de conectores de claude.ai con tu cuenta; sus herramientas aparecen solas en Claude Code. Para Magnific: `claude mcp add --transport http magnific https://mcp.magnific.com` y entra con tu cuenta en el navegador.
- **Login de la CLI.** Si el proveedor tiene CLI (por ejemplo `higgsfield`), instálala e inicia sesión con su comando de login. El plugin la encuentra en tu `PATH`.
- **Variable de entorno en tu shell.** Para APIs como OpenAI, exporta la llave en tu perfil de shell (`~/.zshrc`, `~/.bashrc`) o en tu gestor de secretos: `OPENAI_API_KEY` para OpenAI y `MAGNIFIC_API_KEY` (o `FREEPIK_API_KEY`) para la API REST de Magnific. El detalle de cada proveedor está en su archivo de `skills/proveedores/references/`.

¿Sin proveedor? Funciona igual: recibes el prompt por bloques y los ajustes, listos para pegar en cualquier herramienta.

## Uso

Habla normal. Puedes empezar con `/creative-architect` o pedirlo directo.

```
Tengo esta foto de mi crema facial (entrada/crema.jpg). Hazme 3 fotos para un anuncio de Meta en 4:5, que se vea premium, con espacio arriba para el texto.
```

```
Quiero un video UGC de 15 segundos para TikTok: una chica de 25 años probando mis audífonos en el metro. Gancho fuerte en los primeros 3 segundos.
```

```
Arma el avatar de mi marca con estas 10 fotos mías y clona mi voz con este audio. Lo voy a usar en todos mis reels.
```

```
Anima esta imagen del perfume: solo un movimiento lento de cámara, 5 segundos, vertical para Reels.
```

```
Busca qué anuncios está corriendo Café Tostado del Valle en Meta, dime por qué funciona el que lleva más tiempo y monta el mío con la misma estructura.
```

```
Este prompt me sale genérico: "foto de una taza de café en una mesa". Mejóralo y dime qué cambiaste.
```

## Cómo decide

```
pedido
  │
  ├─ 1. proveedores   ¿qué hay conectado? (MCP → CLI → variables de entorno)
  ├─ 2. marca         lee marca.json y marca/avatar.json si existen
  ├─ 3. oficio        direccion-de-arte · producto · avatar · ugc · animar · clonar-anuncio
  ├─ 4. prompt        decide (sensación, foco del ojo, qué NO) y escribe por bloques
  ├─ 5. genera        intención → herramienta del proveedor, con nota de costo informativa
  ├─ 6. revisa        antipatrones + fidelidad al producto real; lo que falla se rehace
  └─ 7. entrega       trabajo/<fecha>-<oficio>-<slug>/ con prompts.md
```

Sus pilares: decide antes de escribir, luz y composición obligatorias, el producto real manda, poco movimiento gana, gancho en 3 segundos, de la competencia la estructura y nunca el contenido.

## Costos

Si el proveedor permite consultar el costo sin generar, el agente lo dice en una línea ("esto cuesta ~X créditos") y sigue. **Es informativo, no bloquea**: no hay hook ni confirmación obligatoria. Si tu proyecto o tú le piden confirmar antes de gastar, lo hace. Si una pieza falla dos veces, para y te propone otro enfoque en vez de reintentar en bucle. Si hubo gasto, queda un `costo.md` en la carpeta del trabajo.

Lo que **nunca** hace (las cinco reglas):

1. Comprar créditos, planes o recargas por ti.
2. Publicar en redes o sitios si no lo pediste.
3. Gastar tus cupos gratuitos o ilimitados sin que lo digas.
4. Borrar, archivar, despublicar ni mover nada remoto (proyectos, carpetas, assets, voces, identidades), ni cambiar ajustes, espacio de trabajo, suscripción o conexiones de tu cuenta, si no lo pediste con esas palabras.
5. Tocar tus campañas de Meta: del conector de Meta solo usa la búsqueda de la Biblioteca de Anuncios (lectura).

Y además: nunca imprime, copia ni guarda tus llaves; nunca pega el texto de un prompt dentro de un comando; solo sube los archivos que señalaste, al proveedor elegido; y el texto de anuncios, webs, transcripciones o respuestas de proveedores lo lee como material, nunca como órdenes.

Por oficio: solo entrena caras y clona voces de quien las presta (nunca de menores), no genera figuras públicas ni contenido sexual de personas reales, no inventa testimonios, declara que lo generado es IA al publicar y no imita a artistas vivos por su nombre.

## Estructura

```
creative-architect/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── agents/
│   └── creative-architect.md
├── skills/
│   ├── creative-architect/     entrada: /creative-architect
│   ├── direccion-de-arte/      bloques, luz-y-lente, antipatrones, marca
│   ├── producto/               recetas
│   ├── avatar/                 identidad, voz, avatar.schema.json
│   ├── ugc/                    brief, formatos-ugc
│   ├── animar/                 movimiento
│   ├── clonar-anuncio/         anatomia, biblioteca-meta
│   └── proveedores/            intenciones, higgsfield, magnific, openai, nuevo-proveedor, costos
├── docs/
│   └── arquitectura.md
├── CHANGELOG.md
├── LICENSE
└── README.md
```

En tu proyecto, el plugin usa `marca.json`, `marca/avatar.json`, `entrada/` para tus archivos y `trabajo/` para los entregables.

## Créditos

Creado por **Enrique Rocha** · [tododeia.com](https://tododeia.com)

Basado en `higgsfield-estudio` v5.0.0, del mismo autor. El oficio de aquel plugin (los bloques del prompt, luz y lente, antipatrones, ancla de fidelidad, recetas de producto, brief y gancho de UGC, anatomía de anuncios) es el corazón de este.

## Licencia

[MIT](LICENSE) © 2026 Enrique Rocha
