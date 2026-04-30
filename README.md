# Greater Brisbane Income Map

Interactive map of Greater Brisbane (GCCSA) showing income data per suburb and postcode. Click any area for a side-panel breakdown; toggle between two data lenses, switch metrics, compare to your own income.

**Live site:** https://whitehatnetizen.github.io/brisbane-income-map/

## What's shown

| Toggle | Data | Source |
|---|---|---|
| **Suburbs · Census** | Median household, family, and personal weekly income; rent; mortgage; population; average household size — at SAL (Suburb and Locality) level | ABS Census 2021 General Community Profile (G02) |
| **Postcodes · ATO** | Mean taxable income, salary/wages, total income — per postcode | ATO Taxation Statistics 2022–23, Individuals Table 6 |

Two views, two perspectives. Census reports *medians of households*; ATO reports *means of individual taxpayers*. They will rarely agree, and the gap between them is itself informative.

## Features

- Suburb and postcode search (top of right panel)
- Distribution choropleth across 5 metrics, with weekly / monthly / annual conversions
- "Compare $" mode — enter a salary, see which suburbs sit above or below
- Light and dark themes (Quiet Slate)

## Running locally

Browsers block `fetch()` from `file://` URLs, so the page needs an HTTP server:

```sh
python -m http.server 8000
```

Then open http://localhost:8000 in a browser.

## Built with

- [Leaflet](https://leafletjs.com/) for the map
- [CARTO](https://carto.com/) basemap tiles (CC-BY)
- [shapely](https://github.com/shapely/shapely) for polygon simplification (10 m tolerance)
- ABS Digital Atlas FeatureServer for boundary data + Census attributes

## Data licences

- ABS data: [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/)
- ATO data: [Creative Commons Attribution 2.5 Australia](https://creativecommons.org/licenses/by/2.5/au/)
- Boundary data (ABS ASGS Edition 3): CC-BY 4.0
