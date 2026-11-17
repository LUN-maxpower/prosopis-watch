# Prosopis Watch

Satellite monitoring, change tracking and decision support for **_Prosopis juliflora_** — an invasive woody species affecting drylands across the Horn of Africa, East Africa, South Asia and the Middle East.

Runs entirely in the browser. No backend, no build step, no API keys, no installation.

**▶ [Open the live app](https://lun-maxpower.github.io/prosopis-watch/)**

---

## What it does

| Capability | What it computes |
|---|---|
| **CARE Ground Truth & `v20b` RF Model** | Displays all 426 CARE / SoilWatch ground-truth records (`n=94` field plots + `24` Marsabit points + `230` online Prosopis + `78` online Acacia/Balanites), all 57 walked ODK field polygons (`4,789` vertices, `109.95 ha`), and 466 `v20b` 100-tree Random Forest detected Prosopis stands (`13,003.4 ha`, `OA = 92.9%`, `κ = 0.858`), plus a live in-browser `v20b` RF viewport classifier |
| **Map & change** | Maps Prosopis stands from Sentinel-2, then compares two time periods to show where it has been cleared and where it is returning |
| **Regrowth vs recovery** | Uses a 12-month NDVI time series to separate harmful woody regrowth from genuine grassland recovery after clearing |
| **Re-invasion risk** | Scores spread risk across a grid from seed-source pressure, soil moisture, terrain and fire history |
| **Targeting for impact** | Ranks where clearing delivers the greatest gain for grazing, water access and erosion control |
| **MRV quality control** | Verifies reported field activity against the satellite record and produces an audit trail |

A synchronised **swipe comparison** lets you drag a divider between two dates over the same area, with each side carrying its own findings.

---

## How to use it

1. **Search a location** — try `Afar, Ethiopia` or `Baringo, Kenya`
2. **Zoom to the area you want to analyse.** The visible extent is the analysis area, so zoom in for faster and sharper results
3. **Pick a baseline period**, then choose a scene from the list — cloud cover is shown for each
4. **Run the screening.** It takes 30–60 seconds while it reads pixel data tile by tile
5. **Repeat for a second period** and run the change analysis
6. **Enable swipe** and drag the divider to compare

If the best scene in your chosen window has 5% cloud or more, the app automatically searches ±45 days and recommends clearer alternatives.

---

## Method

Prosopis is evergreen. In drylands it holds a green, moist canopy through the dry season while surrounding grass and crops senesce. That contrast is the established remote-sensing basis for mapping it in Afar and Baringo.

Screening combines two Sentinel-2 indices:

- **NDVI** `(B08−B04)/(B08+B04)` — greenness
- **NDMI** `(B08−B11)/(B08+B11)` — canopy moisture

Thresholds default to NDVI ≥ 0.35 and NDMI ≥ 0.05 and are adjustable in the interface.

The **regrowth vs recovery** module exploits the same phenology from the opposite direction: evergreen woody cover gives a high dry-season NDVI floor and low seasonal amplitude, while grassland gives a low floor and high amplitude.

---

## Important limitations

**This is a spectral indicator, not a trained classifier.** It has not been trained or validated against labelled ground truth. Other dense, moist woody species produce the same signal.

**All findings require field validation** before they are used for operational decisions.

**The risk model is a weighted composite of proxies, not a calibrated probability.** Weights are exposed so they can be tuned against local knowledge. Land use and climate are deliberately **not** included — no reliable free browser-accessible source exists at this resolution, and a fabricated proxy would look authoritative while meaning nothing.

**"Not verified" in MRV means the satellite record does not support the claim.** It is not proof that work was not done. Cloud cover, scene gaps and small plot sizes all produce false negatives. Treat it as a screening flag for follow-up, never as an accusation.

**Targeting scores are relative within the current map extent**, not absolute values comparable between areas.

---

## Data sources

- **Sentinel-2 L2A** — optical imagery, 10 m, via [Microsoft Planetary Computer](https://planetarycomputer.microsoft.com/)
- **Copernicus DEM GLO-30** — terrain and slope, via Planetary Computer
- **MODIS MCD64A1** — burned area history, via Planetary Computer
- **Esri World Imagery** — basemap
- **OpenStreetMap / Nominatim** — place name search
- **Leaflet 1.9.4** — mapping library

Planetary Computer anonymous access is **rate-limited and intended for research and non-commercial use**. Heavy or commercial use requires a subscription key and a review of their terms.

Geocoding requests are throttled to one per second in line with the [Nominatim usage policy](https://operations.osmfoundation.org/policies/nominatim/).

---

## Running it yourself

Download `index.html` and open it in any modern browser. It works directly from the file system — no server required.

To host it, drop the single file into any static host: GitHub Pages, Azure Static Web Apps, Netlify, or similar.

> **Do not share it as a OneDrive or SharePoint preview link.** That preview runs HTML in a sandbox that silently blocks network requests, so the app renders blank with no error. Share a GitHub Pages URL, or tell recipients to download the file and open it locally.

---

## Browser support

Any current Chrome, Edge, Firefox or Safari. Requires an internet connection for imagery and analysis.

---

## Licence

MIT — see `LICENSE`.

Imagery and data remain subject to the terms of their respective providers.
