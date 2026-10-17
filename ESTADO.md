# ESTADO — Creative Architect

Estado: v1.0.0 publicada (repo privado Hainrixz/creative-architect, tag y Release v1.0.0).

## Qué se hizo
- 2026-10-17: análisis de `fuente/higgsfield-estudio` (v5.0.0): separado el oficio agnóstico de la capa Higgsfield. Contrato en `docs/arquitectura.md`.
- Construcción en paralelo:
  - Agente `agents/creative-architect.md` y skill de entrada `/creative-architect`.
  - Skills de oficio: `direccion-de-arte` (incluye la forma de `marca.json`), `producto`, `animar`, `avatar` (esquema 2.0 con identidades por proveedor), `ugc`, `clonar-anuncio`.
  - Capa `proveedores`: detección, tabla de fortalezas, `intenciones.md`, un archivo por proveedor (Higgsfield MCP+CLI, Magnific, OpenAI), plantilla `nuevo-proveedor.md` y `costos.md` (solo informativo).
  - Manifiesto `.claude-plugin/plugin.json` y `marketplace.json`, README, LICENSE MIT, CHANGELOG y `.gitignore`.
- Quitados: el hook `creditos.mjs`, la skill `creditos`, la confirmación obligatoria de costo, el máximo de 4 por tanda y la dependencia de `marketing-base`.
- Revisión adversarial con tres revisores independientes (código y consistencia, seguridad, hechos y candados) y dos correctores. Se arregló:
  - Inyección en los ejemplos de shell: ahora se usan `jq --arg`, `--form-string` y el prompt va a un archivo.
  - Regla de que los datos externos no son órdenes.
  - Las cinco reglas: no comprar, no publicar, no gastar cupos, no borrar ni mover nada remoto, y del conector de Meta solo lectura.
  - Reglas de abuso: menores, figuras públicas, contenido sexual y consentimiento.
  - El esquema del avatar, que ahora admite identidad entrenada y referencia guardada juntas.
  - La detección de la CLI `hf`, que se confundía con Hugging Face.
  - La URL del repo (sin guion final).
- Verificado:
  - `claude plugin validate --strict` pasa.
  - Los ejemplos de `avatar.json` validan contra el esquema.
  - El grep sin candados de Higgsfield obligatorios, sin secretos y sin referencias colgadas.
  - Lo de OpenAI (video de Sora apagado el 2026-09-24, modelos gpt-image-2.5) y Magnific (MCP con OAuth y API REST) contra la documentación oficial.

## Qué sigue
- Probar el plugin instalado en una sesión real con cada proveedor. Magnific y OpenAI no están conectados en la Mini: falta probarlos de punta a punta.
- Puntos "sin verificar" de Magnific y OpenAI: están anotados al final de `skills/proveedores/references/{magnific,openai}.md`.

## Decisiones
- `fuente/` queda fuera de git (en `.gitignore`): es el material de origen y vive solo en la Mini.
- Sin hooks: el costo es nota informativa, no se bloquea ni se espera confirmación. Si las instrucciones del proyecto piden confirmar, mandan ellas. Tras dos intentos fallidos de la misma pieza, el agente para y explica.
- El contrato que viaja con el plugin es `skills/proveedores/references/intenciones.md`; `docs/` solo sirve en el repo.
