# Street Pac — GPS Walking Quest (PWA prototype)

A mobile-first static web app prototype for a real-world coin-collecting walking game.

## Features

- Character picker and adjustable 1.0–3.0 km coin zone.
- Configurable 10, 20, 30, 40, 50, or 60-minute game countdown.
- GPS location tracking requests `enableHighAccuracy: true` and rejects weak/stale fixes for gameplay.
- Distance is estimated from filtered fixes, ignoring small jitter steps and implausible jumps.
- Coins are placed on mapped pedestrian ways (footways, paths, pedestrian zones, steps) or beside a road only when OSM explicitly records a sidewalk; unannotated road centre-lines are not coin locations. The intersection pause zone is 10 m; gameplay resumes after leaving a 10 m hysteresis zone. Coins are excluded from a 20 m buffer around mapped junctions and marked crossings. Street-data requests use GET and multiple public instances.
- Cute ghosts roam a simple graph built from the returned street network.
- A nearest-coin direction arrow, a heading cone based on compass heading or recent GPS course, disabled map zoom gestures, best-effort intersection pause zones, a configurable session timer, manual end button, score summary, and Screen Wake Lock when supported.
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
- Live GPS trail data is not sent to an app backend by this prototype, but map and street-data requests reveal location-related information to third parties. See the privacy and store disclosure notes below.
- Ghost encounters are playful and may remove one coin; the app should not encourage users to chase ghosts or look at the screen while moving.
- Test extensively on iOS Safari and Android Chrome before treating the app as a safety feature.

## Privacy and app-store disclosures

The following describes the current prototype and is a starting point for a user-facing privacy policy and store declarations, not legal advice. Keep it updated if code, vendors, or app packaging changes.

### Data and permissions to explain to users

- **Location permission:** The game asks for location access when the user taps **Start Game**. Explain before the system prompt that location is required to place the player on the map, estimate distance, detect nearby intersections/coins/ghosts, and run the walking game. The browser/device supplies location while gameplay is active; tapping **End walk** stops the app's location watch. The app does not request location permission just to view the setup screen.
- **Compass/orientation:** The app may request device-orientation access after the start tap where the platform requires permission. Heading is used for map rotation and direction indicators. If unavailable or declined, the game can use recent GPS movement direction where available.
- **What stays on the device:** The app calculates gameplay state in the browser and has no account, application server, analytics, or GPS-route upload in this prototype. It stores returned street data locally in IndexedDB with its query center, radius, and save time; entries expire after three months. The cache contains map features, not a recorded GPS trail.
- **What map providers receive:** If there is no suitable local cache entry, the app sends the location center and requested radius in a street-data query to one of its configured Overpass services. The map also requests raster tiles from `tile.openstreetmap.org` for the map area being viewed. Those services can receive request metadata such as IP address, time, and technical request information, and can infer the queried or viewed area. Explain this third-party disclosure even though the app does not upload a continuous GPS trail to its own server.
- **Other third parties:** Leaflet's CSS/JavaScript are loaded from unpkg.com. The current page does not include analytics or advertising code. Recheck all bundled libraries and native-wrapper SDKs before release; their behavior may change or add disclosures.
- **User controls:** Users can deny or revoke location/orientation permission in browser or device settings. They can end a walk in the app. Explain that denying location prevents gameplay and that cached street data remains on-device until it expires or the user clears site/app data.

### Store submission checklist

- Publish a privacy policy that names the data used, purposes, local storage/retention, map-service requests and providers, permissions, user controls, and a contact for privacy requests. Link it in the app and store listing where required.
- Complete Apple App Privacy details and Google Play Data safety answers based on the shipped build and the destination stores' current definitions. Review whether location data or other data is collected/shared with third-party map services under each store's definitions; do not answer solely from whether the app has its own backend. Include relevant data handled by SDKs and the native wrapper.
- Use accurate platform permission descriptions. State that location is used for the active walking game and clarify whether any use occurs in the background. This prototype is designed for foreground gameplay; do not claim background tracking or safety-critical location monitoring.
- Preserve clear OpenStreetMap attribution and follow the tile provider's terms. If switching tile/Overpass providers, update the privacy policy and store disclosures to identify the new data recipients and handling.
- **Audience assumption:** This app is not directed to, and is not knowingly used by, children under 13. Reassess audience and applicable child-privacy requirements if the intended audience or actual use changes; a README statement does not by itself establish legal compliance.
