# Cartas Geológicas SERNAGEOMIN — overlays georreferenciados

Overlays georreferenciados de **Cartas Geológicas de geología básica** publicadas por
SERNAGEOMIN (Chile), listos para consumirse como capa de referencia offline en la PWA
[GeoTerreno CDC](https://cvenegas-sernageomin.github.io/geoterreno-cdc/) (libreta de campo
geológica).

Cada mapa se sirve por CDN (`raw.githubusercontent.com`) y se descarga **on-demand** solo
cuando el geólogo activa la capa; a diferencia del patrón hermano de
[mapas-peligros-overlays](https://github.com/cvenegas-sernageomin/mapas-peligros-overlays),
GeoTerreno CDC permite además **descargar cada carta al dispositivo antes de ir a terreno**
para verla sin conexión.

## Piloto (2026-08-25): 9 mapas de la Región del Maule

Reusados tal cual (mismo WebP recortado+warpeado a plate-carrée, mismos `bounds`) desde el
repo [Geo_Repo_Maule_Sernageomin](https://github.com/cvenegas-sernageomin/sernageomin-maule),
filtrados a **solo geología básica** (se excluyen geología aplicada, geoquímica, hillshade,
académico e histórico de ese repo):

- 5 Cartas oficiales de Serie Geología Básica: Pichibelco-Cauquenes (GB-214), San
  Clemente-Melado (GB-220), Río Claro (IR-110), Hoja Laguna del Maule (Carta N°64,
  1:250.000), Tinguiririca-Teno (IR-21-89).
- 4 cuadrantes de geología de campo de Laguna del Maule (Hildreth, Godoy, Fierstein &
  Singer — Boletín 63, SERNAGEOMIN, 2010).

No se georreferenció nada nuevo para este piloto — es trabajo ya hecho y publicado que se
reusa. Ampliar el catálogo a otras regiones/Cartas es un esfuerzo manual aparte (ver el
proyecto hermano `mapas-peligros-overlays` para el detalle de por qué el georreferenciado no
escala automáticamente a cientos de hojas).

## Estructura

- `manifest.json` — índice que lee la PWA: por mapa da `id`, `titulo`, `region`, `fuente`,
  `anio`, `escala`, `informe`, `bounds` (N/S/E/W en WGS84), `overlay` (WebP), `leyenda`
  (imagen de leyenda, si existe), `opacidad`.
- `overlays/<id>.webp` — mapa recortado al neat-line, ya warpeado a plate-carrée.
- `leyendas/<id>.jpg` — leyenda/simbología, solo para las 2 Cartas que la traen aparte
  (Pichibelco-Cauquenes, Tinguiririca-Teno); el resto no tiene leyenda separada en el
  origen (se puede consultar el informe PDF completo en su Carta correspondiente).

## Formato de entrega para cartas nuevas

Al colaborador que georreferencia una carta nueva se le pide **GeoTIFF con CRS declarado**
(EPSG:4326 u otro — el pipeline de conversión reproyecta los bounds), **no** un tile archive
(MBTiles/PMTiles).

El GeoTIFF de entrada nunca se publica ni llega a la PWA: se convierte a `overlays/<id>.webp`
con [`geotiff2webp.py`](../../opencode/proyectos/Georef/tools/geotiff2webp.py) y solo ese
WebP es lo que pesa para el teléfono del geólogo. Pedir un tile archive de entrada sería peor,
no más liviano: obligaría a reensamblar un raster único antes de poder usar el pipeline
existente (recorte de margen blanco, recálculo proporcional de bounds), y sumaría una segunda
compresión con pérdida sobre tiles que ya suelen venir comprimidos — topando el detalle al
zoom máximo que haya elegido el colaborador al tilear, en vez de la resolución nativa del
escaneo.

Si algún día se sirve PMTiles a la PWA en vez de WebP, esa es una decisión de *salida*
independiente de esta — se seguiría tileando desde GeoTIFF de entrada, no desde un tile
archive que mande el colaborador.
