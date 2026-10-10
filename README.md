# Arpent city data

Street data for the [Arpent](https://github.com/bodz-bzh/arpent) app: for each city, the named streets and
their nodes, extracted from [OpenStreetMap](https://www.openstreetmap.org) by the app's `street-extractor` tool.

The app reads these files through the jsDelivr CDN:
`https://cdn.jsdelivr.net/gh/bodz-bzh/map@main/index.json`.

## Layout

The catalog is a tree of small JSON indexes, so finding one city only downloads a few kilobytes:

```
index.json                                       countries
fr/index.json                                    regions
fr/11-ile-de-france/index.json                   departments
fr/11-ile-de-france/92-hauts-de-seine/index.json cities, with their outlines
fr/11-ile-de-france/92-hauts-de-seine/92012-boulogne-billancourt.json.gz
```

Each index lists the next level with its bounding box (`areas`), or the cities of a department (`cities`).
Paths are relative to the folder of the index listing them. City files are gzip-compressed JSON. Every file
carries a `formatVersion`, raised whenever its structure changes.

## Updating

Files are generated, never edited by hand. From the app repository:

```sh
./gradlew :tools:street-extractor:run --args="--insee 92012 --output build/city-data"
./gradlew :tools:street-extractor:run --args="--publish build/city-data --repository ../map"
```

then commit and push this repository. jsDelivr caches `@main` for up to 12 hours.

## License

Data © OpenStreetMap contributors. These files are a derived database of OpenStreetMap and are available
under the [Open Database License (ODbL) 1.0](https://opendatacommons.org/licenses/odbl/1-0/). See `LICENSE`.
