# pickup-position-db
A collection of bass pickup measurements. Started here:  https://www.bassic.de/threads/pu-positions-database.14789156/

## Architecture

Basically this is a [Hugo](https://gohugo.io/) generated website.
We use the [Hinode theme](https://gethinode.com/) because its [data tables](https://gethinode.com/docs/content/tables/#data-tables) deliver all of the core requirements we need (sortable and searchable).

The core data (in a normalized form) is stored in the [data.yaml](./data.yaml) file. A generator (written in Haskell) uses it to fill a data table and merges it with [templates](./ze-ueber-generator/templates) to fill the (index.md)[./content/_index.md] which in turn will be picked up by Hugo to create the final website.

## Requirements

 * Hugo (tested with version v0.150.0)
 * Haskell / Cabal (tested with ghc 9.0.2 and cabal 3.12.1.0. Newer versions will most likely work)

## Build

Refer to the [makefile](./GNUmakefile) for build targets.

You'll most likely just want to run `make` in order to:
 * compile the generator
 * generate the new content file
 * clean up the old site
 * generate the site

After generating the new content you can also run
```
hugo server
```

This allows you to test the site interactively at `localhost:1313`. Commit the changes in `public/`.

## Data

Currently the effective data is hardcoded in `content/_index.md`.
In the long run this file will be generated.
Data for the generation has been checked into `data.yaml`.

The root of `data.yaml` is an array of basses (an their measurements).
The structure of the elements is:

 * **brand**: Text. Mandatory. The brand of the bass.
 * **make**: Text. Mandatory. The make of the bass.
 * **scale**: Numeric. Optional. The scale of the bass in inch. If omitted every measurement requires a **scale** element.
 * **reporter**: Text. Optional. The name/handle of the person who reported the measurement.
 * **comment**: Text. Optional. Global comment on the bass.
 * **year**: Text. Optional. The year the bass was made. _Currently not used. Will be rendered into the make._
 * **measurements**: Array of objects.
    * **description**: Text. Mandatory. Description of the measurement.
    * **value**: Numeric. Mandatory. Value of the measurement (in cm) from the 12th fret.
    * **comment**: Text. Optional. Comment on the measurement. Will be merged with potentially existing global comments.
    * **scale**: Numeric. Optional. The scale of the bass for this measurement. Overrides the per-bass **scale** (if it exists). Mandatory if the per-bass **scale** does not exits.
