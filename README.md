# Street Pac — GPS Walking Quest (PWA prototype)

A mobile-first static web app prototype for a real-world coin-collecting walking game.

## Features

- Character picker and adjustable 1.0–3.0 km coin zone.
- GPS location tracking requests `enableHighAccuracy: true` and rejects weak/stale fixes for gameplay.
- Distance is estimated from filtered fixes, ignoring small jitter steps and implausible jumps.
- Coins are placed on mapped pedestrian ways (footways, paths, pedestrian zones, steps) or beside a road only when OSM explicitly records a sidewalk; unannotated road centre-lines are not coin locations. The intersection pause zone is 25 m (reduced by 5 m); gameplay resumes after leaving a 35 m hysteresis zone. Coins are excluded from a 30 m buffer around mapped junctions and marked crossings. Street-data requests use GET and multiple public instances.
- Cute ghosts roam a simple graph built from the returned street network.
- A nearest-safe-coin direction arrow (it suppresses straight-line hints that would cross a mapped road mid-block), a heading cone based on compass heading or recent GPS course, disabled map zoom gestures, best-effort intersection pause zones, a 10-minute session timer, manual end button, score summary, and Screen Wake Lock when supported.
- A best-effort mid-block crossing detector that ends the session if a sufficiently accurate GPS segment appears to cross a mapped vehicle road away from a mapped junction or tagged crossing.
- PWA manifest and service worker shell caching.

## Deploy on GitHub Pages

1. Copy `index.html`, `manifest.webmanifest`, `icon.svg`, and `sw.js` into a folder in your GitHub Pages repository.
2. If the folder is a project page such as `/street-pac/`, keep all four files in that folder. The relative paths are intentional.
3. Publish the page over HTTPS.
4. Open it on a phone, grant location access, and use the browser menu to add it to the home screen.
5. Start outdoors with location services enabled and a reliable mobile data connection.

## Important limitations before public launch

- **A browser cannot force hardware GPS.** `enableHighAccuracy` requests the best available position, but the operating system/browser chooses available location sources. It is not a guarantee of GPS-only fixes.
- **Intersection pausing and unsafe-crossing alerts are only best-effort.** They infer junctions/crossings and road-crossing segments from OpenStreetMap. OSM data may be incomplete; GPS can lag or jump; and an unsafe crossing cannot be determined reliably by a browser. False positives and false negatives are possible. Never rely on the game for road safety. Stop, look up, and obey traffic rules. Do not use while cycling or driving.
- Distance is a GPS-derived estimate, not a certified fitness measurement. Urban canyons, tree cover, battery controls, and browser suspension can reduce accuracy.
- Map zoom gestures are disabled, and the public Overpass endpoints and OpenStreetMap raster tile service are community-operated and have no availability guarantee. The app tries multiple Overpass instances with a short timeout and displays a retry message if they all fail. Before a larger public launch, choose an appropriate hosted mapping/tiles provider and review its terms and usage limits. Keep visible map attribution.
- Map tiles, third-party map assets, and the initial street-data query require an internet connection. The service worker caches the app shell only; it does not provide offline maps.
- Live GPS trail data is not sent to an app backend by this prototype, but OpenStreetMap/Overpass requests reveal the requested map area to those services. Review this privacy behavior before releasing to children or collecting analytics.
- Ghost encounters are playful and may remove one coin; the app should not encourage users to chase ghosts or look at the screen while moving.
- Test extensively on iOS Safari and Android Chrome before treating the app as a safety feature.
