# API-NOTES.md – Lanka Explorer (Week 04)

Notes on the three external services used in the project.

## 1. Open-Meteo – weather

- **Endpoint:** `https://api.open-meteo.com/v1/forecast`
- **Example request:**
  `https://api.open-meteo.com/v1/forecast?latitude=7.957&longitude=80.7603&current=temperature_2m,relative_humidity_2m,wind_speed_10m,weather_code&daily=weather_code,temperature_2m_max,temperature_2m_min,precipitation_probability_max&timezone=Asia%2FColombo&forecast_days=3`
- **Key needed:** no (free for non-commercial use)
- **What I use from the response:** `current.temperature_2m`, `current.relative_humidity_2m`,
  `current.wind_speed_10m`, `current.weather_code`, and the `daily` arrays.
- **Learned from the docs:** the `daily` object holds *parallel arrays* – index `0` of every array
  belongs to the same day – so one loop with a shared index builds the whole forecast. Weather
  conditions are given as WMO codes, not text, so a `describeWeather()` function translates them.
  `timezone=Asia%2FColombo` (`/` is URL-encoded as `%2F`) makes the daily dates local to Sri Lanka.

## 2. Frankfurter – currency

- **Endpoint:** `https://api.frankfurter.dev/v2/rate/usd/lkr`
- **Key needed:** no
- **What I use:** `rate` – how many Sri Lankan rupees one US dollar buys today.
- **Learned from the docs:** the rate is published once a day from central-bank data, so there is no
  reason to request it on every page; I store it in a module-level variable (`usdToLkrRate`) and
  return the cached value on later calls. That is also why the page can keep working in USD when the
  request fails.

## 3. Leaflet + OpenStreetMap – maps

- **Library:** `https://unpkg.com/leaflet@1.9.4/dist/leaflet.js` (+ `leaflet.css`)
- **Tiles:** `https://tile.openstreetmap.org/{z}/{x}/{y}.png`
- **Key needed:** no
- **What I use:** `L.map()`, `L.tileLayer()`, `L.marker()`, `bindPopup()` and `fitBounds()`.
- **Learned from the docs:** a map needs a container with a real height, otherwise it renders as a
  zero-height strip; OpenStreetMap's usage policy requires the attribution text; and `z-index: 0` on
  the map container keeps it below the sticky header.

## Error handling

| Failure | What the user sees |
| --- | --- |
| Weather request fails or is offline | "Weather is unavailable right now. Please try again later." |
| Currency request fails | Nothing extra – the USD price stays visible and the LKR value is hidden |
| Leaflet script blocked | "The map could not be loaded." |
| `destinations.json` fails | "Sorry, destinations could not be loaded. Please try again later." |
| Wrong `?id=` on the details page | "Destination not found" with a link back to the list |

While requests are running, the weather box, the card grids and the trip summary all show a spinner
with a message (`showLoading()`), so the user never sees an empty box.
