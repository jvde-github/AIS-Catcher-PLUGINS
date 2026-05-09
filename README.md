# AIS-catcher-PLUGINS

Additional AIS-catcher plugins. Updated to the v5 plugin contract — every
plugin reads from the `window.AISCatcher` namespace rather than overriding
globals directly. This is required because AIS-catcher's frontend now runs
as an ES module: top-level identifiers are module-scoped and no longer leak
to `window`, so the old override pattern (`window.includeShip = ...` etc.)
silently no-ops.

## Plugin contract versions

- **v5** — current. Use the `AISCatcher.set*Provider` / `setShipFilter` /
  `setRefreshInterval` hooks for behavior overrides; map customization via
  `AISCatcher.addTileLayer`, `addOverlayLayer`, etc.
- **v4** — `addShipcardItem` requires a function callback (no inline string
  expressions; CSP-compatible).
- **v3** and earlier — broken under modern AIS-catcher builds.

Every plugin starts with the same feature-detect guard:

```js
if (typeof AISCatcher !== 'undefined' && AISCatcher.PLUGIN_API_VERSION >= 5) {
    // ...
}
```

This makes the plugin no-op cleanly on older AIS-catcher hosts that
predate the namespace.

## Plugins in this repo

- **filtership.pjs** — drops ships further than 1000 nmi from the map.
- **googlesearch.pjs** — adds a "Google" entry to the shipcard search menu
  that opens a Google search for the vessel name.
- **maps_with_no_labels.pjs** — replaces the default basemaps with a set
  of label-free variants (Voyager, Satellite, Positron, Dark Matter).
- **moremaps.pjs** — adds a large set of additional basemaps and
  overlays (Esri, OpenTopo, Stamen, OpenSeaMap, NOAA, ...).
- **refreshrate.pjs** — sets the browser's tab refresh rate to 5 s
  (default is 2.5 s).
- **shipnames.pjs** — example of overriding shipname and callsign for
  specific MMSIs (e.g. lighthouses, VTS stations).

## Installing

Copy the `.pjs` files you want into AIS-catcher's `plugins/` directory.
Plugins are loaded into the page in alphabetical order on next browser
refresh.
