# Territory

A very simple location-based exploration tracker. Open it on your phone, allow
location access, and walk around - every hex of a hex grid overlaid on the map
gets claimed the first time you step into it, and deepens in color (blue -
amber - red) the more times you revisit it.

- **Map + hex grid**: a flat-top hex grid (about 20m across) is drawn over an
  OpenStreetMap-based map, zoom in to see it.
- **Visit tracking**: every hex you enter is recorded in your browser's local
  storage, with a running visit count.
- **Color grows with visits**: a hex's fill color moves from a faint cool blue
  toward a deep red the more times it's visited, on a log scale that never
  hard-caps.
- **Re-entry cooldown**: leaving a hex and stepping straight back in does not
  count as a new visit - a hex only counts again once 5 minutes have passed
  since your last counted visit there.

No accounts, no server, no build step - it's a single `index.html` file using
[Leaflet.js](https://leafletjs.com/) (loaded from a CDN) for the map, the
browser's Geolocation API for tracking, and `localStorage` to remember your
explored hexes on that device.

Play it at: https://metroidrhyme.github.io/Territory/

## Files

- `index.html` - the entire app (HTML, CSS, and JS in one file).

## Encoding

The file is kept 100% ASCII on purpose - see `CLAUDE.md`.
