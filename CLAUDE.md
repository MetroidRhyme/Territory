# Territory

A very simple single-file location-based hex exploration tracker. See
`README.md` for what it does.

## File location

Everything - HTML, CSS, and JS - lives in `index.html`. No build step, no
dependency beyond Leaflet.js (CDN) and map tiles. GitHub repo:
`MetroidRhyme/Territory`, branch `main`, published at
`https://metroidrhyme.github.io/Territory/` via GitHub Pages.

## App structure

- Flat-top axial hex grid (`q,r`), circumradius ~20m (`CELL_LAT_DEG`), grid
  anchored at lat 0 / lng 0 (no offset locking needed, unlike Porter/Bloom's
  zone-relative grids). `CELL_LNG_DEG` locks once at the player's first-ever
  GPS fix (longitude-degrees-per-meter shrinks with latitude).
- The mesh is drawn on its own canvas pane (`hexPane`, an `L.canvas` in
  `gridRenderer`), which Leaflet itself CSS-transforms live during pan/zoom -
  the code only repaints on `moveend`/`zoomend`/`viewreset`, never per frame.
- `state.visits` maps hex key `"q,r"` -> `{ count, lastEnteredAt }`, persisted
  to `localStorage` under `territory_state_v1`.
- `enterHex(q, r)` fires only on an actual hex *transition* (tracked via the
  runtime-only `currentHexKey`). A visit counts only if `COOLDOWN_MS` (5 min)
  has passed since that hex's `lastEnteredAt` - which is only ever updated on
  a counted visit, so ducking out and back in during the cooldown neither
  resets the clock nor grants a second credit.
- `heatT(count)` is an asymptotic log curve (`1 - 1/(1 + ln(count+1))`) with
  no hard cap, feeding a blue -> amber -> red gradient (`heatColor`) for a
  hex's fill.
- Below `GRID_MIN_ZOOM` (16) or above `GRID_MAX_CELLS` (2500) hexes in view,
  the mesh is skipped entirely rather than drawn cheaply-but-badly.

## Encoding

The file is kept 100% ASCII bytes on purpose (mirrors the PorterGame/Bloom
convention - non-ASCII glyphs in served strings cause mojibake). Verify
before committing:

```powershell
$p = "C:\Users\Anthony\Documents\GitHub\Territory\index.html"
$b = [System.IO.File]::ReadAllBytes($p)
$bad = @(); for ($i = 0; $i -lt $b.Length; $i++) { if ($b[$i] -gt 127) { $bad += $i } }
if ($b.Length -eq 0) { "ERROR: 0 bytes" } elseif ($bad.Count -eq 0) { "ASCII OK ($($b.Length) bytes)" } else { "NON-ASCII at: $($bad -join ',')" }
```

## Workflow

After completing any change, commit and push directly to `main`:

```
git add -A
git commit -m "..."
git push
```

No PRs, no branches. Pushes go live on GitHub Pages within a minute or two.
