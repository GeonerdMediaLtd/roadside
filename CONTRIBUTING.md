# Contributing

Thanks for helping! The easiest way to contribute is to add places.

## Adding a place

1. Open [mapillary.com/app](https://www.mapillary.com/app) and find an area with plenty of green coverage lines (360° imagery is best).
2. Copy the latitude and longitude of a point there.
3. Add an entry to `data/locations.json`:
   ```json
   { "name": "Town", "country": "Country", "region": "Europe", "lat": 0.0, "lon": 0.0 }
   ```
4. Open a pull request. The deploy workflow checks the file is valid.

Please keep places spread out. Ten seeds in one city make games repetitive.

## Code changes

There's no build step. Edit the files, serve the folder with `python3 -m http.server`, and test on a phone-sized window as well as desktop. Keep the game working without a backend.
