# Street Pac — GPS Walking Quest (PWA prototype)

A mobile-first static web app prototype for a real-world coin-collecting walking game.

## Features

- Character picker and adjustable 1.0–3.0 km coin zone.
- GPS location tracking requests `enableHighAccuracy: true` and rejects weak/stale fixes for gameplay.
- Distance is estimated from filtered fixes, ignoring small jitter steps and implausible jumps.
- Coins are placed along nearby walkable street/path geometry from OpenStreetMap data.
- Cute ghosts roam a simple graph built from the returned street network.
- Best-effort intersection pause zones, a 10-minute session timer, manual end button, score summary, and Screen Wake Lock when supported.
- PWA manifest and service worker shell caching.

## Deploy on GitHub Pages

1. Copy `index.html`, `manifest.webmanifest`, `icon.svg`, and `sw.js` into a folder in your GitHub Pages repository.
2. If the folder is a project page such as `/street-pac/`, keep all four files in that folder. The relative paths are intentional.
3. Publish the page over HTTPS.
4. Open it on a phone, grant location access, and use the browser menu to add it to the home screen.
5. Start outdoors with location services enabled and a reliable mobile data connection.

## Important limitations before public launch

- **A browser cannot force hardware GPS.** `enableHighAccuracy` requests the best available position, but the operating system/browser chooses available location sources. It is not a guarantee of GPS-only fixes.
- **Intersection pausing is only best-effort.** It infers junctions from OpenStreetMap road geometry and uses a conservative geofence buffer. OSM data may be incomplete; GPS can lag or jump; and a fix may arrive after a user has entered an intersection. Never rely on the game to cross roads safely. Stop, look up, and obey traffic rules. Do not use while cycling or driving.
- Distance is a GPS-derived estimate, not a certified fitness measurement. Urban canyons, tree cover, battery controls, and browser suspension can reduce accuracy.
- The public Overpass endpoints and OpenStreetMap raster tile service are community-operated and have no availability guarantee. Before a larger public launch, choose an appropriate hosted mapping/tiles provider and review its terms and usage limits. Keep visible map attribution.
- Map tiles, third-party map assets, and the initial street-data query require an internet connection. The service worker caches the app shell only; it does not provide offline maps.
- Live GPS trail data is not sent to an app backend by this prototype, but OpenStreetMap/Overpass requests reveal the requested map area to those services. Review this privacy behavior before releasing to children or collecting analytics.
- Ghost encounters are playful and may remove one coin; the app should not encourage users to chase ghosts or look at the screen while moving.
- Test extensively on iOS Safari and Android Chrome before treating the app as a safety feature.
