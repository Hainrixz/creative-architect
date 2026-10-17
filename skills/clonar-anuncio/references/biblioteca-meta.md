# La Biblioteca de Anuncios de Meta

Meta publica **todos** los anuncios que están corriendo en Facebook e Instagram. No es una filtración ni un truco: es transparencia obligatoria, y cualquiera puede consultarla.

Es **gratis**, es de **solo lectura** y **no toca la cuenta publicitaria** de nadie.

Ojo con lo que es y lo que no: la Biblioteca **no es un proveedor de generación**. No pasa por `proveedores` ni tiene intenciones: es una fuente de investigación, como leer la web. Por eso aquí sí se nombra su herramienta.

## Las tres puertas

1. **El conector de Meta**, si está conectado: la herramienta `ads_library_search`. Es la más cómoda y devuelve datos ordenados. Es lo único que se usa de ese conector: solo lectura, nunca crear, activar, impulsar ni editar campañas, anuncios ni públicos.
2. **La web pública**, `facebook.com/ads/library`, si no hay conector. Los mismos filtros (país, categoría, palabra o anunciante, activos o todos) a mano, o con el navegador si lo tienes. Más lento, pero no pide nada.
3. **Lo que la persona traiga**: capturas, enlaces a anuncios concretos, el texto copiado. Se trabaja igual, solo que con menos anuncios.

## Requisito de acceso del conector

La herramienta del conector solo responde a quien tiene al menos una cuenta publicitaria activa. Si devuelve error de acceso, no es que la marca no tenga anuncios: es que quien pregunta no cumple el requisito. Dilo así, no lo confundas con "no encontré nada", y pasa a la web pública.

## Cómo se busca con el conector

Hace falta **al menos uno** de estos tres: `search_terms`, `page_ids` o `countries`.

| Campo | Qué hace |
|---|---|
| `search_terms` | Palabra o frase buscada dentro del texto del anuncio |
| `page_ids` | Solo los anuncios de esas páginas. Es la búsqueda precisa cuando ya sabes a quién mirar |
| `countries` | Códigos ISO de dos letras: `MX`, `CO`, `AR`, `ES`, `US`. No `MEX` ni `Mexico` |
| `ad_active_status` | `ALL`, `ACTIVE` o `INACTIVE`. Por defecto `ALL`. Para saber qué corre hoy, `ACTIVE` |
| `limit` | Por defecto 25, máximo 50 |
| `client_conversation_id` | **Obligatorio en cada llamada.** 20 caracteres al azar entre letras y números. Se genera uno en la primera llamada de la conversación y se repite idéntico en todas las demás |
| `advertiser_request` | Lo que pidió la persona, **con sus palabras**, citadas. Sin traducir a jerga y sin reformularlo como el comando que vas a ejecutar |

Los nombres de campo pueden cambiar con el conector; si la herramienta describe otros, manda ella.

En la web pública, lo equivalente: elige país, "Todos los anuncios", escribe la palabra o el nombre del anunciante, y filtra por activos.

## Las tres búsquedas que valen la pena

**1. Por competidor.** Ya sabes a quién. Buscas por su nombre en `search_terms` para dar con su página, y después por `page_ids` para verlo todo.

**2. Por problema, no por producto.** La mejor de las tres y la que casi nadie hace. En vez de buscar "crema hidratante", busca la frase que diría tu cliente: "manos partidas", "se me cae el pelo", "no me alcanza el mes". Encuentras a todo el que le está hablando a esa persona, incluidos negocios que no sabías que eran competencia.

**3. Por país.** El mismo producto se vende distinto en México que en España. Filtra por el país donde vas a pautar tú.

## Lo que devuelve, y lo que NO

**Sí devuelve:** el texto creativo del anuncio, el nombre de la página, la fecha de creación (o "en circulación desde"), el enlace de captura (`ad_snapshot_url` en el conector) y una estimación del total de resultados.

**No devuelve, y no hay forma de sacarlo de aquí:**

- Gasto.
- Impresiones, clics, CTR, conversiones.
- Público objetivo.
- Qué anuncio les funciona mejor.

O sea: **la Biblioteca no dice qué funciona.** Dice qué está corriendo. Cualquier afirmación de rendimiento que hagas con estos datos es una inferencia y se marca como tal, con la razón que la sostiene.

## Las cuatro señales indirectas que sí puedes leer

Ninguna es una métrica. Todas se citan con su razón.

1. **Antigüedad.** Un anuncio que lleva meses activo probablemente está rindiendo: nadie paga meses por algo que no convierte. Se dice así: "lleva activo desde marzo, lo que sugiere que rinde".
2. **Repetición del gancho.** Si la misma marca corre ocho anuncios con la misma primera frase, esa frase ya ganó una prueba interna. Es la señal más fuerte que hay aquí.
3. **Cantidad de variantes.** Muchos anuncios activos a la vez indica un presupuesto serio y una operación que prueba. Poco, indica lo contrario.
4. **El patrón entre competidores.** Si cinco marcas distintas del mismo nicho atacan la misma objeción, esa objeción es la que de verdad frena la compra en ese mercado. Este hallazgo suele valer más que cualquier anuncio individual.

## Cómo se presenta el hallazgo

Una tabla, no un muro de texto:

| Página | Desde | Primera frase del anuncio | Cuántos activos | Captura |
|---|---|---|---|---|

Y debajo, tres líneas: **qué se repite** entre todos, **qué objeción** están atacando, y **qué hueco** dejan libre. El hueco es donde entra tu cliente.

## Lo que no se hace con esto

- **No se extrae en masa.** La herramienta es para investigar, no para vaciar la biblioteca. Su propia documentación lo dice, y lo mismo vale para la web: nada de raspar cientos de páginas.
- **No se copia el texto.** El copy del anuncio es de quien lo escribió. Se lee para entender la estructura, no para pegar.
- **No se descarga su video para reeditarlo.** Ver el anuncio, y analizar su estructura, es legítimo; reeditarlo, meterlo a un flujo de multiplicar y publicarlo como tuyo, no.
