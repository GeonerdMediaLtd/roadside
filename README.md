# Roadside: Guess the Road

A free, open-source GeoGuessr-style game that runs entirely in the browser. You're dropped on a real street somewhere in the world. Look around, walk down the road, read the signs, then drop a pin on the map. The closer you are, the more points you get (up to 5,000 per round).

Street-level imagery comes from [Mapillary](https://www.mapillary.com) and the guess map from [OpenStreetMap](https://www.openstreetmap.org). There's no server and no build step: it's plain HTML, CSS and JavaScript.

## Features

- **Walk around:** 360° street-level views you can turn, zoom and move through (arrows on the road).
- **Modes:** Classic, No moving (look around only) and Daily (the same 5 roads for everyone each day, with a shareable score).
- **Regions:** whole world or one continent, with scoring scaled to the area.
- **Timer:** off, 60 s, 2 min or 5 min per round.
- **Phone-first:** full-screen viewer, bottom-sheet map. On desktop, a corner map that grows when you hover over it.
- **Installable:** add it to your home screen. The app shell works offline; streets need a connection.
- After each round you get the real spot on the map, a link to the image on Mapillary, and a link to explore the place in Google Earth.

## Quick start

1. **Get a Mapillary token (free).** Sign in at mapillary.com and open [Dashboard → Developers](https://www.mapillary.com/dashboard/developers). Register an application and copy its **client token** (starts with `MLY|`).
2. **Add the token** in one of two ways:
   - Put it in `js/config.js` as `mapillaryToken` so everyone who opens your site can play, or
   - Leave `config.js` empty and paste it in the game under **Mapillary token**. It's saved only in that browser.
3. **Turn on GitHub Pages:** go to **Settings → Pages**, set **Source** to **GitHub Actions**, and push to `main`. The workflow in `.github/workflows/pages.yml` publishes the site at `https://<you>.github.io/<repo>/`.

### Run locally

Any static server works:

```sh
python3 -m http.server 8000
# open http://localhost:8000
```

## How it works

1. `data/locations.json` holds about 200 seed places on six continents.
2. For each round the game picks a seed, wanders a few kilometres from it, and asks the Mapillary API for imagery in a small box around that point. It prefers 360° panoramas and falls back to normal photos.
3. [MapillaryJS](https://github.com/mapillary/mapillary-js) shows the image. The image's own coordinates are the answer.
4. The daily mode seeds the random choices with the date (UTC), so everyone gets the same roads.

Libraries are copied into `lib/` (MapillaryJS 4.1.2, Leaflet 1.9.4) so the site doesn't depend on a CDN.

## Add places

Edit `data/locations.json` and add an entry:

```json
{ "name": "Hobart", "country": "Australia", "region": "Oceania", "lat": -42.8821, "lon": 147.3272 }
```

`region` must be one of: Europe, Asia, Africa, North America, South America, Oceania. Add an optional `"spread": 0.1` (in degrees) to let the game wander further from the point; the default is `0.06`. Places with good Mapillary coverage work best. Check the map at mapillary.com/app first. See [CONTRIBUTING.md](CONTRIBUTING.md).

## About your token

A Mapillary client token is meant to be used in browser code, so anyone can read it from your site. That's normal for this kind of token. If someone abuses it, create a new one on the Mapillary dashboard and update `config.js`.

## Credits and licences

- Code: MIT (see `LICENSE`).
- Street imagery: © Mapillary contributors, [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). The contributor's name and a link to each image appear after every round. Mapillary's attribution overlay is hidden while you play so it doesn't give away the location.
- Map data: © OpenStreetMap contributors, ODbL. Tiles from tile.openstreetmap.org follow the [OSM tile usage policy](https://operations.osmfoundation.org/policies/tiles/). For heavy traffic, switch `tileUrl` in `config.js` to another provider.
- MapillaryJS (MIT) and Leaflet (BSD-2-Clause): licences in `lib/`.

## Ideas for next steps

- Live multiplayer and leaderboards (for example with Supabase or Firebase).
- Country-only guessing mode.
- A scheduled GitHub Action that checks every seed still has imagery.
- More seed places, especially in Africa, Central Asia and South America.
