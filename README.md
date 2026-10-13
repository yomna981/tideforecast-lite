# Dahab Tide & Marine Forecast

A zero-dependency, self-contained oceanographic and marine meteorological dashboard designed specifically for **Dahab, South Sinai, Egypt** (`28.5097° N, 34.5136° E`) on the Gulf of Aqaba (Red Sea).

Delivers real-time marine weather, live wind telemetry, wave conditions, and a client-side harmonic astronomical tide engine capable of running completely offline by simply double-clicking `index.html`.

---

## Highlights

- **Single Self-Contained File (`index.html`)**: Zero build steps (no Node.js, Webpack, Vite, or compilers), zero external dependencies, and zero CDN links. Runs directly via the `file://` protocol.
- **Offline Astronomical Tidal Engine**: Client-side harmonic constituent model calibrated specifically for the Gulf of Aqaba micro-tidal regime (~0.50m to 1.05m range). Computes water height and High/Low turning points without requiring an internet connection.
- **Live Marine Meteorological Telemetry**: Connects to free, open-access [Open-Meteo APIs](https://open-meteo.com/) (curated from [public-apis](https://github.com/public-apis/public-apis)) for live air temperature, sea surface temperature, wind speed/direction/gusts, humidity, dew point, and wave metrics.
- **Three Interactive Tide Visualizations**:
  - **48-Hour Continuous Harmonic Curve**: Canvas/SVG wave spanning past 24h to future 24h with real-time "NOW" marker, High/Low extrema badges, and interactive scrub/hover cursor.
  - **7-Day Multi-Day Outlook**: Daily forecast cards showing upcoming high and low tide peaks, timing, and daily tidal ranges.
  - **12-Hour Circular Dial**: Nautical tide clock illustrating current water stage, flood/ebb direction, and countdown to next peak or trough.
- **Dahab Marine Spots Guidance**: Practical recommendations for Dahab's famous watersport locations (Dahab Lagoon, Lighthouse Reef, Eel Garden, Blue Hole, The Canyon).
- **Dual Visual Themes**: Specialized **Dark Marine** (deep midnight navy with cyan accents) and **Coastal Light** (high-contrast sunlight readability) themes with instant toggle and `localStorage` persistence.

---

## Quick Start

### Direct Execution (No Setup Required)
Simply double-click [`index.html`](file:///D:/--VSCode--/tideforecast-lite/app/index.html) or open it in any modern web browser:

```bash
# On Windows (PowerShell)
Start-Process index.html

# On macOS
open index.html

# On Linux
xdg-open index.html
```

### Optional Local Web Server
While the dashboard is engineered to run seamlessly over `file://`, you can also serve it with any local static HTTP server:

```bash
# Using Python 3
python -m http.server 8080

# Using Node.js npx
npx serve .
```
Then navigate to `http://localhost:8080`.

---

## Features Breakdown

### 1. Astronomical Tidal Prediction Model
Because official physical tide gauge feeds in Dahab are often proprietary or intermittent, this application integrates a client-side harmonic astronomical tide prediction model calibrated to Dahab's micro-tidal conditions:

- **Harmonic Constituents**:
  - `M2` (Principal lunar semidiurnal, speed `28.9841°/h`, amplitude `0.280 m`, phase lag `65°`)
  - `S2` (Principal solar semidiurnal, speed `30.0000°/h`, amplitude `0.100 m`, phase lag `65°`)
  - `N2` (Larger lunar elliptic semidiurnal, speed `28.4397°/h`, amplitude `0.055 m`, phase lag `40°`)
  - `K1` (Luni-solar diurnal, speed `15.0411°/h`, amplitude `0.040 m`, phase lag `110°`)
  - `O1` (Principal lunar diurnal, speed `13.9430°/h`, amplitude `0.030 m`, phase lag `85°`)
  - `M4` (Shallow water overtide, speed `57.9682°/h`, amplitude `0.015 m`, phase lag `130°`)
- **Mean Sea Level (MSL)**: Calibrated datum baseline of `+0.58 m`.
- **Peak / Trough Detection**: Root-finding bisection algorithm (`bisectZeroRateCrossing`) locates zero-crossings of the tidal rate derivative with sub-minute precision.
- **Lunar Tracking**: Computes synodic month phase fractions, lunar illumination percentage, lunar phase emoji/names, and dynamically classifies **Spring Tides** (Syzygy) vs. **Neap Tides** (Quadrature).

### 2. Live Marine Weather Telemetry
When an internet connection is available, the dashboard fetches live telemetry via Open-Meteo:
- **Wind Speed & Direction**: Real-time speed in `km/h`, rotating visual compass needle, cardinal compass points (e.g. `NNE`, `N`), and peak gusts.
- **Beaufort Scale Rating**: Automated categorization into Beaufort Force 0–8 with watersport suitability suggestions (SUP, wing foil, kitesurfing, windsurfing).
- **Air Temperature**: Current temperature in `°C` alongside daily minimum and maximum bounds.
- **Sea Surface Temperature**: Water temperature in `°C` paired with wetsuit recommendations (e.g. 5mm full wetsuit vs. 3mm vs. rashguard/shorty).
- **Humidity & Calculated Dew Point**: Relative humidity percentage and psychrometric Magnus-Tetens dew point calculation.
- **Wave Metrics**: Significant wave height in meters and wave period in seconds.

### 3. Resilient Offline Architecture
If the device loses network connectivity or the weather API is unreachable:
- The dashboard automatically updates the status badge to **Offline Mode**.
- The astronomical tide curve, daily outlook, lunar phases, and circular gauge remain **100% operational and interactive**.
- Non-intrusive warnings notify the user while preserving all offline calculation utilities.

### 4. Dahab Spot Navigation Guide
Tailored local knowledge integrated directly into the dashboard:
- **Dahab Lagoon**: Tidal height thresholds for high-tide flat water (&gt;0.75m) and low-tide outer sandbar speed strips (&lt;0.35m).
- **Lighthouse & Eel Garden Reefs**: Fringing coral clearance indicators and sandy route entry recommendations during low tide.
- **Blue Hole & The Canyon**: Littoral drift information and slack water tidal transition windows for drop-off entry.

---

## Project Structure

```
tideforecast-lite/
├── app/
│   ├── index.html            # Complete self-contained application (HTML, CSS, JS, SVGs)
│   ├── preprompt.txt         # Project requirements and constraints
│   ├── prompt-grill.txt      # Specification details and architectural decisions
│   └── README.md             # Project documentation
└── README.md                 # Root documentation mirror
```

---

## Technical Specifications

| Parameter | Specification |
| :--- | :--- |
| **File Format** | Single standalone `.html` file |
| **Dependencies** | None (0 external libraries, 0 frameworks, 0 CDNs) |
| **Fonts** | Native system font stacks (`system-ui`, `-apple-system`, `BlinkMacSystemFont`, `Segoe UI`, `Roboto`) |
| **Icons** | Pure inline SVG vector elements |
| **Data Protocol** | Native asynchronous Fetch API with `Promise.allSettled` |
| **APIs Used** | Open-Meteo Weather & Marine Forecast APIs (Free, no API key required) |
| **Target Coordinates** | `28.5097° N`, `34.5136° E` (Dahab, South Sinai, Egypt) |
| **Browser Compatibility** | Chrome, Edge, Firefox, Safari, Brave, Opera (Desktop & Mobile) |
| **Offline Support** | Complete offline harmonic tide modeling and UI interaction |

---

## License & Credits

- Weather and oceanographic data courtesy of [Open-Meteo](https://open-meteo.com/).
- APIs sourced via the open-source directory [public-apis](https://github.com/public-apis/public-apis).
- Developed as a lightweight, resilient tool for the Dahab watersport and marine community.
