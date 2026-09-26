# Forest Seedling Plots: Synthetic Stem Maps with Hidden Mother Trees

## Overview

This dataset contains 600 synthetic forest plots of 200 m by 200 m. Each plot records a stem map of the adult trees, the positions of the young seedlings found in five yearly censuses, the direction the wind blew during each seed year, and a map of canopy openness. The mother of each seedling, either a specific adult tree in the plot or a seed source too far away to name, is recorded in a separate table and is the quantity of interest.

Nothing here is observed in the field. Every plot, tree, seedling, identifier and map value is produced by a generator whose draws are HMAC-SHA256 keyed to a withheld 256-bit secret, so no part of the release can be regenerated or matched against any public archive.

## Release At A Glance

- 600 plots, 125,556 adult trees, 253,673 mapped seedlings.
- Between 6 and 9 tree species per plot, and between 107 and 325 adult trees per plot.
- Each adult tree has a species, a position and a trunk diameter at breast height (10 to 120 cm).
- Each seedling has a species, a position and the census year in which it was first found (1 to 5).
- Canopy openness on a 20 by 20 grid of 10 m cells, and one wind direction per seed year.
- Species labels are local to a plot: species `A` in one plot has nothing to do with species `A` in another.
- A plot is the independent unit: its trees, seedlings and maps belong to it alone.

## How The Data Was Generated

Every adult tree produces seed each year. How much depends on its trunk size, with the most productive size different for each species, and on the year, since trees of one species tend to seed heavily in the same years while each tree also varies on its own. Seeds are thrown from the mother with a heavy-tailed distance distribution whose spread differs between species, and some species are pushed further along the wind of that year. Some seed arrives from outside the plot or from trees too far away to name. A seed becomes a recorded seedling more often in open canopy and less often close to many adults of its own species. Each seedling is recorded in the census after the year its seed fell.

A seedling's recorded mother is `distant` when its seed came from outside the plot or from a tree more than 50 m away. Otherwise it is that tree.

The settings follow published studies of seed dispersal kernels, trunk-size dependence of seed production, synchronous heavy seed years within a species, and density-dependent seedling survival. Three parts are ours: the exact shape of the size curve, how strongly the wind stretches the dispersal kernel, and the range of the share of seed arriving from far away.

## Raw File Structure

The uploaded ZIP is flat and contains exactly these nine files at its root:

- `plots.csv`: one record per plot: `case_id`, `n_trees`, `n_seedlings`.
- `trees.csv`: one record per adult tree: `tree_id`, `case_id`, `species`, `x_m`, `y_m`, `dbh_cm`.
- `seedlings.csv`: one record per mapped seedling: `seedling_id`, `case_id`, `species`, `x_m`, `y_m`, `census_year` (1 to 5).
- `wind.csv`: one record per plot and seed year: `case_id`, `seed_year` (0 to 4), `wind_toward_deg`, the direction the wind blew toward, measured from the x axis toward the y axis.
- `canopy.csv`: one record per plot and 10 m cell: `case_id`, `cell_x`, `cell_y` (0 to 19), `openness` (0 to 1).
- `mothers.csv`: one creator-side record per seedling: `seedling_id`, `case_id`, `cause`, which is `distant` or `tree:<tree_id>`; used by `prepare.py` and never copied into public prepared data for the test plots.
- `LICENSE`: CC BY 4.0 notice and licence URL.
- `DATASET_DESCRIPTION.md`: this description, shipped inside the archive so the card and the data cannot drift apart.
- `PACKAGE_MANIFEST.sha256`: SHA-256 checksum of every other file in the package.

## Intended Use And Limitations

The dataset is intended for work on assigning offspring to candidate parents from spatial evidence alone, where the parents differ in size and output and the dispersal process has to be inferred. It is fully synthetic. It simplifies real forests: there is no terrain, no pollen parent, no seed predation or secondary dispersal by animals, and adults do not grow or die during the five years. Results on it say nothing about any real forest.

## Licence

CC BY 4.0. The dataset is synthetic and contains no personal data and no third-party material.
