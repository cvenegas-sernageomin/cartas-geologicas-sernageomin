# Cartas geológicas georreferenciadas — notas para Claude

Overlays de cartas (lámina → WebP + bounds) que consumen Geonotas y GeoChile vía `manifest.json`.
Repo `cvenegas-sernageomin/cartas-geologicas-sernageomin` (rama `main`).
Ficha en memoria: `proyecto-cartas-geologicas-georref`.

## Reglas
- Insertar entradas en `manifest.json` **como texto**, no con `json.dump` (reformatea floats y ensucia el diff).
- Al cambiar un overlay ya publicado, subir su campo `rev` (Geonotas re-descarga por `rev`).
- Al enlazar por regex en el manifest, acotar a la propia entrada (`(?:(?!"id": ).)*?`) y verificar que cada `leyenda` contenga su id.
- El datum se **lee de la hoja**, no se deduce. Validar contra la grilla UTM (memoria `reference-validar-georref-con-grilla-utm`).
- No tocar sin el usuario: serie antigua CGCH_01–28, GB-127/129/161, cartas sin PDF fuente.

## Carpetas locales (gitignored, no se publican)
`_auditoria-georref/` (herramientas de revisión), `_mapas_por_ingresar/` (2,6 GB de láminas pendientes), `_para-cartografo/`.

## Verificar
Servidor: config `cartas-geologicas` (puerto 8793). Tras push, comparar md5 contra `raw.githubusercontent.com/.../main/...`.
