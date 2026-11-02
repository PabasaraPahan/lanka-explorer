# Lanka Explorer – Discover the Pearl of the Indian Ocean

**Live site:** _add your hosted link here after deploying (see Hosted_Link.txt)_

**Student:** Your Name &nbsp;|&nbsp; **Index number:** Your Index No. &nbsp;|&nbsp; **Individual mini project (Weeks 01–05)**

Lanka Explorer is a travel guide web app for Sri Lanka. It lists ten destinations across four
categories (Heritage, Beach, Nature, Wildlife), shows live weather and a three-day forecast for each
place, converts entry fees from US dollars into Sri Lankan rupees, plots every destination on an
interactive map, and lets the visitor build a trip with a total budget. Everything is built with
plain HTML, CSS, JavaScript and JSON – no frameworks.

## How to run the site

The site must be opened through a web server, because `fetch()` is blocked on `file:///` pages.

1. Open the `lanka-explorer` folder in Visual Studio Code.
2. Right-click `index.html` → **Open with Live Server**.
3. The browser should show `http://127.0.0.1:5500/`.

Any other static server works too, for example `python -m http.server 8000` inside the folder.

## Folder structure

```
lanka-explorer/
├── index.html            Home page
├── destinations.html     All destinations (search, filter, sort, map)
├── details.html          One destination in detail (?id=1 … ?id=10)
├── trip.html             My Trip planner and budget
├── about.html            About the project + enquiry form
├── README.md             This file
├── API-NOTES.md          Notes on the three APIs used
├── css/
│   └── style.css         All styling, responsive rules and dark mode
├── js/
│   └── app.js            All JavaScript, organised in numbered sections
├── data/
│   └── destinations.json The ten destinations
└── images/               One photo per destination + page hero image
```

## The data file

`data/destinations.json` is an array of ten objects. Each object has the fields `id`, `name`,
`district`, `category`, `description`, `image`, `lat`, `lng`, `entryFeeUSD`, `bestTime` and
`featured`. Exactly three destinations are marked `"featured": true`, which is what the Home page
uses. Every card, filter result and trip row on the site is generated from this one file: add an
object and the whole site updates without touching the HTML.

## Features

- **Home page** – three featured destinations built by JavaScript from the JSON file.
- **Destinations page** – live search (name, district and description), category filter and three
  sort orders that work together, plus a result counter.
- **Details page** – reads `?id=` from the URL, shows the full description and quick facts, and
  handles a wrong id with a friendly "Destination not found" message.
- **Enquiry form** – JavaScript validation with all errors shown at once (name length, email
  pattern, past travel date, traveller range, at least one interest).
- **Weather** – current conditions and a 3-day forecast from Open-Meteo for each destination's own
  coordinates, with a loading spinner and a friendly error message.
- **Currency** – today's USD → LKR rate from Frankfurter, fetched once and cached, shown next to the
  entry fee.
- **Maps** – Leaflet + OpenStreetMap: one pin on each details page and a map of all ten destinations
  with popups that link to the details pages.
- **My Trip** – add and remove destinations, choose the number of travellers, and see the total entry
  cost in USD and LKR. Trips are saved in `localStorage`, so they survive a reload.
- **Dark mode** – a toggle in the navigation switches a second set of CSS variables, and the choice is
  remembered. On a first visit the site follows the operating system setting.
- **Responsive layout** – one card grid that works from 375 px to 1280 px and wider, with a hamburger
  menu on small screens.

## APIs used

| Service | Endpoint | What it provides | Key needed |
| --- | --- | --- | --- |
| Open-Meteo | `https://api.open-meteo.com/v1/forecast` | Current weather + 3-day forecast for a latitude/longitude | No |
| Frankfurter | `https://api.frankfurter.dev/v2/rate/usd/lkr` | Today's USD → LKR exchange rate | No |
| Leaflet 1.9.4 + OpenStreetMap tiles | `https://tile.openstreetmap.org/{z}/{x}/{y}.png` | Map library, map images and pins | No |

All three are free and allow requests from any website (CORS), so no key or proxy is needed. See
`API-NOTES.md` for the details.

## Testing and Lighthouse

Checked on the Home, Destinations, Details, My Trip and About pages at 1280 px and 375 px, in light
and dark mode: navigation, search/filter/sort, wrong-id handling, add/remove trip, traveller totals,
form validation, and the API error states (each API blocked in DevTools → Network → Offline).

Lighthouse (Chrome DevTools → Lighthouse → Mobile, run on the hosted site):

| Page | Performance | Accessibility | Best practices | SEO |
| --- | --- | --- | --- | --- |
| Home | 93 | 100 | 100 | 100 |
| Destinations | 79 | 96 | 100 | 100 |
| Details | 90 | 100 | 100 | 100 |
| My Trip | 95 | 100 | 100 | 100 |
| About | 100 | 100 | 100 | 100 |

Issues I found and fixed:

1. **Colour contrast** – the grey meta text (result counter, trip summary) and the Beach/Wildlife
   badges were below the 4.5:1 ratio. Darkened `--grey` to `#55605f` and the two badge colours.
2. **Heading order** – the destinations page jumped from `h1` to the card `h3`. Added a
   screen-reader-only `h2` ("Search results") above the grid.
3. **Layout shift on the details page** – the JavaScript-built content pushed the map down after
   load (CLS 0.69). Fixed by reserving heights on `#destination` and `#weather` (CLS now 0.11).

Remaining (third-party): the Leaflet map pins are 25 × 41 px and sit close together, which Lighthouse
flags under *target size* on the Destinations page – that is the 96 instead of 100.

Screenshots of every page at 375 px and 1280 px, in light and dark mode, are in `screenshots/`.

## Individual reflection

See `REFLECTION.md` for the short reflection (what I built, the hardest problem, what I would improve).

## Image credits

Destination photos are free-to-use images from Pexels and Unsplash and similar free sources,
downloaded and resized to 1200 px wide. Map tiles and credit: © OpenStreetMap contributors.

## What I would improve next

A real booking flow with dates and prices, a server-side form handler (JavaScript validation can be
switched off in DevTools), and saving the trip in the URL so a plan can be shared with a friend.

