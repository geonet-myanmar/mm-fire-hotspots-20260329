# 🔥 Myanmar Fire Hotspots — Satellite Monitoring Dashboard

[![GitHub Pages](https://img.shields.io/badge/Live_Demo-GitHub_Pages-ff4d1c?style=for-the-badge&logo=github)](https://geonet-myanmar.github.io/mm-fire-hotspots-20260329/)
[![License: MIT](https://img.shields.io/badge/License-MIT-ffab00?style=for-the-badge)](LICENSE)
[![Data: NASA FIRMS](https://img.shields.io/badge/Data-NASA_FIRMS-00bcd4?style=for-the-badge)](https://firms.modaps.eosdis.nasa.gov/)

An interactive web-based dashboard for visualizing active fire hotspots across Myanmar using VIIRS (Visible Infrared Imaging Radiometer Suite) satellite data from the Suomi NPP and NOAA-20 missions. Built as a single-page HTML application deployable via GitHub Pages with zero server-side dependencies.
---

## Table of Contents

- [Background](#background)
- [Features](#features)
- [Live Demo](#live-demo)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Data Sources](#data-sources)
- [Connecting to NASA FIRMS Live API](#connecting-to-nasa-firms-live-api)
- [Map Controls & Interface](#map-controls--interface)
- [Technical Details](#technical-details)
- [Customization](#customization)
- [Deployment to GitHub Pages](#deployment-to-github-pages)
- [Related Resources](#related-resources)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

---

## Background

During the dry season (January–April), mainland Southeast Asia experiences extensive biomass burning from agricultural clearing, forest fires, and land management practices. Myanmar consistently records some of the highest fire hotspot counts in the region.

On **29 March 2026**, the Suomi NPP satellite's VIIRS sensor detected **6,863 fire hotspots** across Myanmar — the highest among all Southeast Asian nations on that date. This dashboard was created to visualize and explore those hotspots spatially, providing an interactive tool for researchers, disaster response teams, and the public.

### Regional Context (29 March 2026)

| Country | Hotspots |
|---------|----------|
| **Myanmar** | **6,863** |
| Thailand | 4,327 |
| Laos | 3,046 |
| Cambodia | 1,010 |
| Vietnam | 802 |
| Malaysia | 37 |

*Source: GISTDA Thailand via Suomi NPP VIIRS*

---

## Features

### Core Visualization
- **6,863 fire hotspot markers** plotted on an interactive Leaflet.js light-themed map
- **Color-coded by Fire Radiative Power (FRP)**: yellow (low), amber (medium), orange-red (high), red (extreme)
- **Scaled marker radius** proportional to fire intensity
- **leaflet.heat heatmap overlay** with multi-stop gradient (yellow–amber–orange–red–crimson) showing fire density and clustering patterns
- **Admin Level 1 boundaries** — 15 state/region/union territory polygons from MIMU loaded via `admin1.geojson` with hover tooltips

### Interactive Controls
- **Heatmap toggle** — enable/disable the thermal density overlay
- **Point layer toggle** — show/hide individual fire markers
- **Click-to-inspect** — click any hotspot to see detailed attributes in a styled popup:
  - Geographic coordinates (lat/lng)
  - State/Region/Union Territory name and type
  - Fire Radiative Power (MW)
  - Brightness temperature (K)
  - Detection confidence (%)
  - Satellite (Suomi NPP / NOAA-20)
  - Acquisition date and time (UTC)

### Dashboard Panels
- **Statistics panel** — total hotspot count, high-confidence detections (≥80%), average brightness temperature
- **Regional breakdown** — clickable chips showing hotspot counts by state/region with fly-to navigation
- **Legend** — FRP classification with color reference
- **Info panel** — data source attribution and methodology notes

### Design & UX
- **Light basemap (CartoDB Positron)** ensuring fire hotspots and heatmap patterns are clearly visible against a neutral background
- **Responsive layout** adapting to desktop and mobile screens
- **Animated loading screen** with satellite data branding
- **Glassmorphism UI panels** with backdrop blur and subtle glow effects
- **Custom popup styling** matching the dark dashboard aesthetic
- **JetBrains Mono + DM Sans** typography pairing

---

## Live Demo

👉 **[View the live dashboard →](https://geonet-myanmar.github.io/mm-fire-hotspots-20260329/)**


---

## Getting Started

### Prerequisites

This is a **zero-dependency static HTML application**. You only need:
- A modern web browser (Chrome, Firefox, Edge, Safari)
- A text editor (for modifications)
- Git (for version control and deployment)

No build tools, package managers, or server-side runtime required.

### Quick Start

```bash
# Clone the repository
git clone https://github.com/geonet-myanmar/mm-fire-hotspots-20260329.git
cd mm-fire-hotspots-20260329

# Open directly in browser
open index.html
# or on Linux:
xdg-open index.html
# or on Windows:
start index.html
```

### Local Development Server (Optional)

For a local server with live reload:

```bash
# Using Python
python -m http.server 8000

# Using Node.js
npx serve .

# Then open http://localhost:8000
```

---

## Project Structure

```
mm-fire-hotspots-20260329/
├── index.html          # Main application (single-file, self-contained)
├── admin1.geojson      # MIMU Myanmar state/region boundaries (Admin Level 1)
├── README.md           # This documentation
├── LICENSE             # MIT License
└── CONTRIBUTING.md     # Contribution guidelines
```

### Architecture

The entire application is contained in a single `index.html` file for maximum portability and zero-configuration deployment. This includes:

- **HTML** — semantic structure with overlay panels, map container, and UI controls
- **CSS** — complete styling with CSS custom properties for theming (~200 lines)
- **JavaScript** — async GeoJSON loading, boundary-constrained data generation with point-in-polygon validation, Leaflet.js map initialization, interaction handlers, and statistics computation (~330 lines)

External dependencies loaded via CDN:
- [Leaflet.js v1.9.4](https://leafletjs.com/) — interactive map engine
- [leaflet.heat v0.2.0](https://github.com/Leaflet/Leaflet.heat) — heatmap visualization plugin
- [CartoDB Positron](https://carto.com/basemaps/) — light base map tiles
- [Google Fonts](https://fonts.google.com/) — JetBrains Mono & DM Sans typefaces

Local data files:
- `admin1.geojson` — Myanmar Admin Level 1 boundaries (15 states/regions/union territory, ~724 KB), sourced from [MIMU](https://themimu.info/) (Myanmar Information Management Unit). Properties: `ST` (English name), `ST_PCODE` (place code), `ST_RG` (State/Region/Union Territory), `ST_MMR` (Myanmar-language name)

---

## Data Sources

### Current Implementation

The dashboard currently uses **synthetically generated data** that accurately models the reported 6,863 hotspots with:

- **Boundary-constrained placement** — every hotspot is generated within actual state/region polygons from `admin1.geojson` using ray-casting point-in-polygon testing (rejection sampling ensures no points fall outside Myanmar's borders)
- **Realistic spatial distribution** weighted by Myanmar's 15 states/regions/union territory based on known fire patterns (Sagaing 27%, Shan 22%, Mandalay 10%, Kachin/Magway 8% each)
- **Clustered point generation** simulating natural fire clustering behavior (40% of points cluster near existing points within the same region)
- **Log-normal FRP distribution** matching typical VIIRS fire detections
- **Realistic attribute ranges** for brightness temperature (300–410 K), confidence (40–100%), and acquisition times across multiple satellite overpasses
- **Single source of truth** — all region metadata (centroids, bounding boxes, polygon boundaries) is derived at runtime from `admin1.geojson`, eliminating hardcoded duplicates

### Primary Data Sources

| Source | Description | URL |
|--------|-------------|-----|
| **NASA FIRMS** | Fire Information for Resource Management System | https://firms.modaps.eosdis.nasa.gov/ |
| **GISTDA Thailand** | Geo-Informatics and Space Technology Development Agency | https://disaster.gistda.or.th/fire |
| **VIIRS Active Fire** | 375m resolution fire product (VNP14IMG) | https://earthdata.nasa.gov/earth-observation-data/near-real-time/firms/viirs-i-band-375-m-active-fire-data |
| **MIMU** | Myanmar Information Management Unit — Admin Level 1 boundaries | https://themimu.info/ |

### Satellite Sensors

| Satellite | Sensor | Resolution | Equator Crossing |
|-----------|--------|------------|-----------------|
| Suomi NPP | VIIRS | 375 m | 01:30 / 13:30 local time |
| NOAA-20 | VIIRS | 375 m | 00:50 / 12:50 local time |

---

## Connecting to NASA FIRMS Live API

To replace the synthetic data with real-time FIRMS data, follow these steps:

### 1. Obtain a FIRMS API Key

Register for a free MAP KEY at: https://firms.modaps.eosdis.nasa.gov/api/area/

### 2. Modify the Data Fetching Function

Replace the `generateMyanmarHotspots(regionMeta)` call in the `init()` function with a FIRMS API fetch. The `classifyPoint()` function already available in the codebase will assign each FIRMS detection to its correct state/region using the actual admin1 polygon boundaries:

```javascript
async function fetchFIRMSData(regionMeta) {
  const API_KEY = 'YOUR_FIRMS_MAP_KEY';
  const url = `https://firms.modaps.eosdis.nasa.gov/api/country/csv/${API_KEY}/VIIRS_SNPP_NRT/MMR/1`;

  try {
    const response = await fetch(url);
    const csvText = await response.text();
    const lines = csvText.trim().split('\n');
    const headers = lines[0].split(',');

    return lines.slice(1).map(line => {
      const values = line.split(',');
      const row = {};
      headers.forEach((h, i) => row[h.trim()] = values[i]?.trim());

      const lat = parseFloat(row.latitude);
      const lng = parseFloat(row.longitude);

      return {
        lat, lng,
        frp: parseFloat(row.frp) || 0,
        bright: parseFloat(row.bright_ti4) || 0,
        confidence: row.confidence === 'h' ? 90 : row.confidence === 'n' ? 60 : 30,
        satellite: row.satellite || 'N/A',
        acqTime: row.acq_time ?
          `${row.acq_time.slice(0,2)}:${row.acq_time.slice(2,4)} UTC` : '',
        acqDate: row.acq_date || '',
        // Classify using actual admin1 polygon boundaries
        region: classifyPoint(lat, lng, regionMeta) || 'Unknown'
      };
    });
  } catch (error) {
    console.error('FIRMS API error:', error);
    return generateMyanmarHotspots(regionMeta); // Fallback to synthetic data
  }
}
```

### 3. Update the Init Function

Replace the `generateMyanmarHotspots(regionMeta)` call with `fetchFIRMSData(regionMeta)`:

```javascript
// In the init() function, change this line:
allData = generateMyanmarHotspots(regionMeta);
// To:
allData = await fetchFIRMSData(regionMeta);
```

The existing `classifyPoint()` function uses ray-casting point-in-polygon testing against the actual admin1.geojson boundaries — no approximate coordinate-based classification needed.

### API Parameters Reference

| Parameter | Description | Example |
|-----------|-------------|---------|
| `source` | Satellite source | `VIIRS_SNPP_NRT`, `VIIRS_NOAA20_NRT`, `MODIS_NRT` |
| `country` | ISO 3166-1 alpha-3 code | `MMR` (Myanmar), `THA` (Thailand) |
| `day_range` | Number of days (1–10) | `1` for last 24h, `7` for last week |
| `format` | Output format | `csv`, `json`, `kml`, `shp` |

### FIRMS API URL Patterns

```
# Country-level (CSV)
https://firms.modaps.eosdis.nasa.gov/api/country/csv/{KEY}/{SOURCE}/{COUNTRY}/{DAYS}

# Area-based (bounding box)
https://firms.modaps.eosdis.nasa.gov/api/area/csv/{KEY}/{SOURCE}/{W},{S},{E},{N}/{DAYS}

# Myanmar bounding box
https://firms.modaps.eosdis.nasa.gov/api/area/csv/{KEY}/VIIRS_SNPP_NRT/92,9.5,101,28.5/1
```

---

## Map Controls & Interface

### Header
Displays the project title, satellite source identifier, and a pulsing fire indicator showing the dashboard is active.

### Statistics Panel (Right Side)
| Card | Description |
|------|-------------|
| **Total Hotspots** | Complete count of fire detections displayed |
| **High Confidence** | Detections with ≥80% confidence level |
| **Avg Brightness** | Mean brightness temperature in Kelvin |

### Regional Panel (Left Side)
Clickable chips showing the top 8 states/regions by hotspot count. Clicking any chip flies the map to that region's centroid (computed from `admin1.geojson` geometry) at zoom level 8.

### Bottom Controls
| Button | Function |
|--------|----------|
| **Heatmap** | Toggle thermal density overlay on/off |
| **Cluster** | Toggle individual point markers on/off |
| **ⓘ About** | Show/hide data source information panel |

### Map Interactions
- **Scroll/pinch to zoom** — standard Leaflet zoom behavior
- **Click + drag** — pan the map
- **Click a hotspot** — open detailed popup with all attributes
- **Click a region chip** — animated fly-to with zoom

---

## Technical Details

### Fire Radiative Power (FRP) Classification

| Class | FRP Range | Color | Marker Radius |
|-------|-----------|-------|---------------|
| Low | < 10 MW | `#ffe066` (Yellow) | 3 px |
| Medium | 10–50 MW | `#ffab00` (Amber) | 4 px |
| High | 50–100 MW | `#ff4d1c` (Orange-Red) | 5 px |
| Extreme | > 100 MW | `#ff1744` (Red) | 6 px |

### Heatmap Generation

The heatmap overlay is generated using the [leaflet.heat](https://github.com/Leaflet/Leaflet.heat) plugin:
- Each hotspot contributes an intensity value derived from log-scaled FRP (`log1p(frp) / log1p(maxFrp)`)
- Heatmap parameters: radius 18 px, blur 25 px, maxZoom 10, minOpacity 0.25
- Multi-stop color gradient: `#ffffb2` (0.2) → `#fecc5c` (0.4) → `#fd8d3c` (0.55) → `#f03b20` (0.7) → `#e31a1c` (0.85) → `#bd0026` (1.0)
- Dynamically re-renders on zoom and pan for consistent visual analysis at all scales

### Administrative Boundaries

`admin1.geojson` serves as the **single source of truth** for all geographic operations:
- **Source**: MIMU (Myanmar Information Management Unit)
- **Features**: 15 admin units — 7 States (Chin, Kachin, Kayah, Kayin, Mon, Rakhine, Shan), 7 Regions (Ayeyarwady, Bago, Magway, Mandalay, Sagaing, Tanintharyi, Yangon), 1 Union Territory (Nay Pyi Taw)
- **Properties**: `ST` (English name), `ST_PCODE` (MIMU place code), `ST_RG` (administrative type), `ST_MMR` (Myanmar-language name)
- **Styling**: `#4a5568` stroke (1.5 px), light fill at 15% opacity
- **Interaction**: Hover tooltip displays `"{name} {type}"` (e.g., "Sagaing Region", "Chin State", "Nay Pyi Taw Union Territory")
- **Derived data** (computed at runtime from GeoJSON, not hardcoded):
  - Region centroids (for fly-to navigation)
  - Bounding boxes (for rejection-sampling hotspot placement)
  - Polygon geometry (for point-in-polygon boundary validation)
  - Region classification (for assigning hotspots to admin units)

### Spatial Data Distribution

Hotspot generation weights by state/region (based on historical fire patterns):

| Admin Unit | Type | Weight | Typical Count |
|------------|------|--------|---------------|
| Sagaing | Region | 27% | ~1,853 |
| Shan | State | 22% | ~1,510 |
| Mandalay | Region | 10% | ~686 |
| Kachin | State | 8% | ~549 |
| Magway | Region | 8% | ~549 |
| Bago | Region | 6% | ~412 |
| Chin | State | 5% | ~343 |
| Kayin | State | 3% | ~206 |
| Rakhine | State | 3% | ~206 |
| Kayah | State | 2% | ~137 |
| Tanintharyi | Region | 2% | ~137 |
| Nay Pyi Taw | Union Territory | 1% | ~69 |
| Mon | State | 1% | ~69 |
| Yangon | Region | 1% | ~69 |
| Ayeyarwady | Region | 1% | ~69 |

### Performance

- **Initial load**: ~2 seconds (GeoJSON fetch → metadata extraction → boundary-validated hotspot generation → rendering)
- **6,863 circle markers**: rendered via Leaflet's Canvas renderer for optimal performance
- **Point-in-polygon validation**: ray-casting algorithm with bounding-box pre-rejection for fast spatial testing
- **leaflet.heat heatmap**: canvas-based rendering with dynamic re-rendering on zoom
- **Admin boundaries**: ~724 KB MIMU GeoJSON fetched once, then reused for boundaries, generation, and classification
- **Total file size**: ~27 KB HTML + ~724 KB admin1.geojson

---

## Customization

### Change the Color Theme

Edit the CSS custom properties in the `:root` block:

```css
:root {
  --bg-dark: #0a0e17;        /* Main background */
  --accent-fire: #ff4d1c;     /* Primary fire color */
  --accent-amber: #ffab00;    /* Secondary warm color */
  --accent-cool: #00bcd4;     /* Cool accent for contrast */
}
```

### Adjust Hotspot Count

Modify the `n` value in `generateMyanmarHotspots()`:

```javascript
const n = 6863;  // Change this to any desired count
```

### Change Region Weights

Update the `REGION_WEIGHTS` object to model different fire distribution scenarios:

```javascript
const REGION_WEIGHTS = {
  'Sagaing': 0.27, 'Shan': 0.22, 'Mandalay': 0.10, 'Kachin': 0.08,
  'Nay Pyi Taw': 0.01,
  // Adjust weights (must sum to ~1.0)
  // Bounding boxes and polygons are derived automatically from admin1.geojson
};
```

### Switch Base Map

Replace the tile layer URL:

```javascript
// Light theme (default)
L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {...})

// Dark theme
L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {...})

// Satellite imagery
L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {...})

// OpenStreetMap
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {...})
```

### Extend to Other Countries

Adjust the map center, zoom, and coordinate bounds:

```javascript
// For Thailand
const map = L.map('map', { center: [15.0, 100.0], zoom: 6 });

// For all of mainland Southeast Asia
const map = L.map('map', { center: [17.0, 100.0], zoom: 5 });
```

---

## Deployment to GitHub Pages

### Step 1: Create a GitHub Repository

```bash
# Initialize git in your project folder
cd mm-fire-hotspots-20260329
git init

# Rename the main HTML file to index.html (if not already)
mv myanmar_hotspots.html index.html

# Add all files
git add .
git commit -m "Initial commit: Myanmar Fire Hotspots Dashboard"

# Create repo on GitHub and push
git remote add origin https://github.com/geonet-myanmar/mm-fire-hotspots-20260329.git
git branch -M main
git push -u origin main
```

### Step 2: Enable GitHub Pages

1. Go to your repository on GitHub
2. Navigate to **Settings** → **Pages**
3. Under **Source**, select **Deploy from a branch**
4. Choose **main** branch and **/ (root)** folder
5. Click **Save**

### Step 3: Access Your Site

Your dashboard will be live at:

```
https://geonet-myanmar.github.io/mm-fire-hotspots-20260329/
```

GitHub Pages typically deploys within 1–2 minutes. Check the **Actions** tab for deployment status.

### Custom Domain (Optional)

To use a custom domain:

1. Add a `CNAME` file to the repo root containing your domain name
2. Configure DNS records with your domain registrar
3. Enable HTTPS in the GitHub Pages settings

---

## Related Resources

### Fire Monitoring Platforms
- [NASA FIRMS](https://firms.modaps.eosdis.nasa.gov/) — Global fire monitoring
- [GISTDA Fire Monitoring](https://disaster.gistda.or.th/fire) — Thailand & ASEAN fire data
- [Global Forest Watch Fires](https://fires.globalforestwatch.org/) — WRI fire dashboard
- [ASEAN Haze Action Online](http://haze.asean.org/) — Regional transboundary haze monitoring

### Technical References
- [VIIRS Active Fire Product User Guide](https://viirsland.gsfc.nasa.gov/PDF/VIIRS_activefire_User_Guide.pdf)
- [Leaflet.js Documentation](https://leafletjs.com/reference.html)
- [FIRMS API Documentation](https://firms.modaps.eosdis.nasa.gov/api/)

### Research Context
- Sagaing Fault seismicity and environmental monitoring
- Southeast Asian transboundary haze from biomass burning
- Dry season fire patterns in Myanmar's forest conservation areas

---

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Areas where contributions would be particularly valuable:
- Integration with live NASA FIRMS API
- Time-series animation showing fire progression over multiple days
- Smoke/haze dispersion modeling overlay
- Multi-country support for ASEAN fire monitoring
- Accessibility improvements (screen reader support, keyboard navigation)

---

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- **NASA FIRMS** for providing open access to near real-time fire data
- **GISTDA Thailand** for regional fire hotspot monitoring and reporting
- **Leaflet.js** community for the excellent open-source mapping library
- **leaflet.heat** plugin for heatmap visualization
- **CartoDB** for Positron light-themed base map tiles
- **MIMU** (Myanmar Information Management Unit) for official Admin Level 1 boundary data
- Fire hotspot report data referenced from GISTDA bulletin, 29 March 2026

---

<p align="center">
  <strong>Built for open science and disaster awareness 🌏🔥</strong>
</p>
