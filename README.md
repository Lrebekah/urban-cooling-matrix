# Urban Cooling Decision Matrix

An adjustable-weight scoring tool and map for prioritizing urban cooling interventions along the Duke Street corridor in Alexandria, Virginia.

**Live tool:** https://lrebekah.github.io/urban-cooling-matrix

## What it does

The tool ranks parcels, buildings and bus stops in the corridor for nine kinds of cooling intervention, from street trees and cool pavement to green roofs and bus stop shelters. Each score combines a few standard GIS layers (surface temperature, vegetation, built-up extent, canopy gap and ownership) using weights derived with the Analytic Hierarchy Process (AHP). You can change the weights with sliders and watch the rankings, charts and map update right away.

It is meant to support early prioritization, not final siting decisions. Scores are relative within this corridor, and every candidate site still needs a field check.

## Using the tool

- **Sliders:** each intervention has its own weight sliders. Weights are relative, so the tool rescales them to add up to 100%, and the percentage beside each slider shows its current share. “Reset to AHP defaults” restores the original weights.
- **Map:** the map follows whichever slider you move, and the menu above it switches to any other score. Colors run from the lowest to the highest current score, and the five top sites have a black outline. Click a site for details.
- **All Priority Sites:** this map view shows every parcel and building that lands in the top five for two or more interventions, colored by how many.
- **Combined Priority Sites and Top 5 charts:** both update as you change the weights.
- **Tables:** click any column header to sort.

## Interventions scored

| Score | Interventions covered | Scored on |
|---|---|---|
| Vegetation/Shade | Street trees, natural playgrounds | Parcels |
| Surface/Pavement | Cool pavement, high-albedo walls, tracks | Parcels |
| Stormwater | Bioretention, permeable pavement | Parcels |
| Canopy-Need | Shaded play spaces, retractable sails | Parcels |
| Built-Up Need | Breeze blocks, splash pads, misting, parklets | Parcels |
| Cool Roof Coatings | Reflective roof coatings | Buildings |
| Green Roofs | Extensive and intensive green roofs | Buildings |
| Evaporative Cooling Walls | Evaporative cooling walls | Buildings |
| Bus Stop Shade Structures | Bus shelters | Bus stops |

## How the scores work

Each score is a weighted overlay: a weighted sum of inputs that have been normalized to a 0 to 1 scale. The default weights come from AHP pairwise comparisons that were checked for consistency (consistency ratio below 0.10). Ownership is included as a feasibility factor, since public sites are easier for a city to act on.

Some interventions also have eligibility rules:

- **Green roofs** need a footprint over 5,000 square feet and are discounted by roof slope.
- **Evaporative cooling walls** are discounted by a fixed factor of 0.6 when a building has no wall in a favorable solar orientation. That factor is not adjustable.
- **Bus stop shelters** are ranked by surface temperature among stops with 40 or more average daily boardings, the City of Alexandria’s eligibility standard. They have no adjustable weights.

When the page loads, it recomputes every score at the default weights and compares the result with the value exported from ArcGIS Pro. Open the browser console (F12) and you should see one “Formula check” line per score ending in `mismatches 0`.

### Relationship to thermal comfort

The sliders carry labels such as “relates to Radiant Load.” These inputs are proxies. Surface temperature, built-up extent and canopy gap relate to the thermal comfort drivers of radiant load, air temperature and humidity, but they do not measure them. Ownership is a feasibility factor, not a thermal driver. The tool favors replicability with widely available data over detailed microclimate modeling.

## Data

The `data` folder holds three GeoJSON files exported from ArcGIS Pro in WGS84: `parcels_scored.geojson`, `buildings_scored.geojson` and `busstops_scored.geojson`. Each file carries the normalized input values as well as the final scores, which is what makes the live recalculation possible. Owner names and mailing addresses were left out.

Sources:

- **Land surface temperature, NDVI and NDBI:** derived from Landsat imagery (30 m native resolution).
- **Tree canopy:** University of Vermont Spatial Analysis Laboratory. (2025). *High Resolution Land Cover Dataset for Alexandria, VA* (V1). University of Vermont Dataverse. https://doi.org/10.5072/FK2/AX6ELT
- **Parcels, building footprints and ownership:** City of Alexandria GIS data.
- **Roof slope and green roof thresholds:** slope derived from LiDAR; the slope and footprint thresholds are adapted from the Northern Virginia Regional Commission (NVRC) Green Roof Map.
- **Bus ridership:** DASH. The 40 boardings per day threshold comes from the City of Alexandria Bus Stop Accessibility & Amenities Program.
- **Mapping:** [Leaflet](https://leafletjs.com). Basemap tiles © OpenStreetMap contributors and CARTO.

## Running it yourself

The easiest way is the live link above. To run a copy on your own computer, serve the folder rather than double-clicking `index.html`, because browsers block a page from reading local data files that way. With Python installed, run this in the project folder:

```
python -m http.server
```

On Windows you can use `py -m http.server` instead. Then open http://localhost:8000 in your browser. You need an internet connection for the map library and the basemap tiles.

The map uses OpenStreetMap tiles by default. To use the CARTO basemap, get a free key at https://carto.com/basemaps/apikey and paste it between the quotes in the `CARTO_KEY` line near the top of the map code in `index.html`.

## Limitations

- **Proxies, not measurements.** The inputs approximate the thermal comfort drivers. They are not measured air temperature, humidity or radiant exposure.
- **Coarse temperature data.** Surface temperature has a native resolution of about 30 meters, so neighboring buildings can share the same value.
- **Relative scores.** Scores rank sites against each other within this corridor. They are not predicted temperature reductions.
- **Building-to-parcel links.** The tool assigns each building to the parcel that contains its center point. A building that spans two parcels is credited to only one.
- **Weights are judgments.** The defaults reflect one set of pairwise comparisons. The sliders let others test their own priorities.

## Citation and contact

[Add the paper citation or project link and contact details here.]
