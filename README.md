# Greater Brisbane Income Map

Interactive choropleth of Greater Brisbane (GCCSA) showing income, housing-cost, and housing-burden data per suburb and per postcode. Click any area for a side-panel breakdown; toggle between data lenses, switch metrics, compare against a salary or a rent figure.

> **Best viewed on desktop.** The interface is dense by design — multiple toggles, a side panel, and a floating legend. Mobile is functional but cramped.

## Two views

The map can show either suburbs or postcodes. The two come from different data sources and measure income differently — they aren't directly comparable but they're complementary.

| View | Geography | Data | Source |
|---|---|---|---|
| **Suburbs · Census** | ABS SAL (Suburbs and Localities) | Median household, family, and personal weekly income; weekly rent; monthly mortgage; housing cost burden; population; average household size | ABS Census 2021 General Community Profile, table G02 |
| **Postcodes · ATO** | ABS POA (Postal Areas) | Median taxable income, median salary/wages, median total income (per taxpayer) | ATO Taxation Statistics 2022–23, Individuals Table 25 |

Census reports medians per *household*; ATO reports means per *individual taxpayer*. A postcode where the ATO mean is well above the Census median typically has a long tail of high earners pulling the average up.

## Features

- **Search** — type-ahead suburb/postcode finder at the top of the side panel
- **Distribution mode** — choropleth shaded by quantile across the chosen metric, with weekly / monthly / annual conversions in the side panel
- **Compare $ mode** — enter an amount and the map splits into "at or above" vs "below" the threshold (indigo / mustard — colour-blind safe)
- **Housing-burden mode** — each suburb shaded by housing cost as a percentage of income, with three bands at 30% (stress) and 50% (severe stress) per Australia's 30/40 rule. Toggle the denominator between household income and personal income to see the difference between a couple/share-house view and a single-earner view.
- **Information popovers** explain Household / Family / Personal income definitions and housing-burden methodology
- **Light and dark themes**

## Running locally

The page loads its data files via `fetch()`, which browsers block from `file://` URLs. Run any static HTTP server from the project root, for example:

```sh
python -m http.server 8000
```

Then visit `http://localhost:8000`. The deployed site (GitHub Pages) is served over HTTPS and works directly with no setup.

## Methodology notes

- **Greater Brisbane GCCSA** is defined by the ABS as Brisbane LGA plus Ipswich, Logan, Moreton Bay, and Redland (and a portion of Scenic Rim and Somerset). Suburb polygons are SALs whose centroid falls inside the GCCSA boundary; postcode polygons are POAs whose centroid falls inside.
- **Polygon simplification** uses Visvalingam-Whyatt at a 10 m tolerance, reducing GeoJSON size by roughly two-thirds with no visible difference at the map's zoom range (9–15).
- **Housing burden** is computed at display time: median weekly rent (or median monthly mortgage × 12 ÷ 52) divided by the chosen median income figure. The 30% and 50% thresholds follow established Australian housing-policy convention. Suppressed cells (small-population suburbs) appear in grey on the map and as "n/a" in the side panel.
- **ATO data** comes from Table 25 of the Taxation Statistics, which publishes count, average, and median directly per postcode. The map shows medians for like-for-like comparability with the Census-side medians. Annual figures are converted to weekly for display consistency.

## Built with

- [Leaflet](https://leafletjs.com/) for map rendering
- [CARTO](https://carto.com/) basemap tiles
- ABS Digital Atlas ArcGIS FeatureServer for boundaries and Census attributes
- [Shapely](https://github.com/shapely/shapely) for polygon simplification (Python)

## Data licences

- **ABS** data — [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/)
- **ATO** data — [Creative Commons Attribution 2.5 Australia](https://creativecommons.org/licenses/by/2.5/au/)
- **ABS ASGS Edition 3** boundary files — CC-BY 4.0
- **CARTO** basemap tiles — CC-BY (attribution shown on the map)

## Disclaimer

This map presents publicly available statistics for general information. Figures are from 2021 (Census) and 2022–23 (ATO) and may not reflect current conditions. Suppressed values exist where ABS or ATO privacy thresholds were not met. Burden bands are illustrative — individual circumstances will vary.
