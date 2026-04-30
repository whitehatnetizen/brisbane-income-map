# Greater Brisbane Income Map

Interactive choropleth of Greater Brisbane (GCCSA) with two data lenses: **Census & ATO** (who lives where, 2021/2022–23 snapshot) and **RTA** (current rental market on new private leases, latest quarter plus growth since Sep 2021). Click any suburb or postcode for a side-panel breakdown; switch lens, switch geography, switch metric, compare against a salary or rent figure.

> **Best viewed on desktop.** The interface is dense by design — multiple toggles, a side panel, and a floating legend. Mobile is functional but cramped.

## Two lenses

The map deliberately separates two questions you might ask about a Brisbane suburb. They use different data sources, measure different populations, and aren't directly comparable — so they live in different lenses to avoid invitation to bad division.

| Lens | What it shows | Source(s) |
|---|---|---|
| **Census & ATO** | Median household / family / personal income, weekly rent, monthly mortgage, housing-cost burden (suburb view); median taxable / salary / total income per taxpayer (postcode view) | ABS Census 2021 G02 at SAL · ATO Taxation Statistics 2022–23 Individuals Table 25 |
| **RTA** | Median weekly rent for new private leases — latest quarter, Sep 2021 baseline, and growth between them. CPI Brisbane shown as a benchmark on the growth card | RTA bond statistics, quarterly |

### Why these don't mix

Census rent is **self-reported** (a household member fills in question 56) and pools **all tenancies** including long-tenured tenants on legacy rents. RTA rent is **administrative** data drawn from the bond lodgement form on **new private-market tenancies** that quarter — different population, different methodology. Even at the same point in time RTA tends to run higher than Census because it captures the marginal tenant paying market rate. The growth metric inside the RTA lens is RTA-to-RTA (Sep 2021 vs latest), apples-to-apples — never RTA-vs-Census.

## Two geographies

Either lens can be viewed at suburb (ABS SAL) or postcode (ABS POA) granularity. RTA does not publish a postcode "all dwellings" rollup, so postcode rent values are computed as a bonds-weighted average across the published per-dwelling categories.

## Features

- **Search** — type-ahead suburb/postcode finder at the top of the side panel
- **Distribution mode** — choropleth shaded by quantile across the chosen metric, with weekly / monthly / annual conversions in the side panel
- **Compare $ mode** — enter an amount and the map splits into "at or above" vs "below" the threshold (indigo / mustard — colour-blind safe)
- **Housing-burden mode** — each suburb shaded by housing cost as a percentage of income, with three bands at 30% (stress) and 50% (severe stress) per Australia's 30/40 rule. Toggle the denominator between household income and personal income to see the difference between a couple/share-house view and a single-earner view.
- **RTA growth scale** — the rent-growth metric uses fixed thresholds at 0% / 20% / 40% / 60% / 80% / 100% (rather than data-driven quantiles) so the darkest band is always reserved for suburbs where rent has at least doubled since Sep 2021. Breakpoint values are shown as ticks under the legend bar.
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
- **Housing burden** is computed at display time: median weekly rent (or median monthly mortgage × 12 ÷ 52) divided by the chosen median income figure. The 30% and 50% thresholds follow established Australian housing-policy convention. Suppressed cells (small-population suburbs) appear in grey on the map and as "n/a" in the side panel. Burden lives in the Census & ATO lens only — it pairs Census rent with Census income (matched vintage). Pairing RTA rent with Census income would mix vintages and silently change what "stress" means, so it isn't offered.
- **ATO data** comes from Table 25 of the Taxation Statistics, which publishes count, average, and median directly per postcode. The map shows medians for like-for-like comparability with the Census-side medians. Annual figures are converted to weekly for display consistency.
- **RTA data** is the quarterly bond statistics file, suburb sheet (`All dwellings` rollup) and postcode sheet (no rollup published — computed as a bonds-weighted average across the per-dwelling category medians). The Sep 2021 quarter is the baseline because it's the first full quarter after the 2021 Census reference date and is well within the suburb-level series (which starts Sep 2017). Coverage at the suburb level is roughly 46% of GCCSA SALs — the rest sit permanently below the RTA suppression threshold (small bond counts) and render with an explicit suppressed message rather than a value. Postcode coverage is ~80%.
- **CPI benchmark** on the rent-growth card uses ABS All-groups CPI Brisbane, ~22% over Sep 2021 → Mar 2026. Single labelled constant; no suburb-level wage imputation is done, by design.

## Built with

- [Leaflet](https://leafletjs.com/) for map rendering
- [CARTO](https://carto.com/) basemap tiles
- ABS Digital Atlas ArcGIS FeatureServer for boundaries and Census attributes
- [Shapely](https://github.com/shapely/shapely) for polygon simplification (Python)

## Data licences

- **ABS** data — [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/)
- **ATO** data — [Creative Commons Attribution 2.5 Australia](https://creativecommons.org/licenses/by/2.5/au/)
- **RTA** bond statistics — [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/) (© State of Queensland)
- **ABS ASGS Edition 3** boundary files — CC-BY 4.0
- **CARTO** basemap tiles — CC-BY (attribution shown on the map)

## Disclaimer

This map presents publicly available statistics for general information. Figures are from 2021 (Census), 2022–23 (ATO), and the latest published RTA quarter (refreshed quarterly on the 1st of Apr / Jul / Oct / Jan). Suppressed values exist where ABS, ATO, or RTA privacy thresholds were not met. Burden bands are illustrative — individual circumstances will vary.
