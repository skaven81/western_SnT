Sky & Telescope Stellarium Skyculture
=====================================

Constellation Shapes
--------------------

### Usage

The constellation shapes (`constellationship.fab`) are generated using
the `generate_constellationship.py` script.  This script accepts
Sky & Telescope's constellation data (`SnT_constellations.txt`) on STDIN,
and returns Stellarium's constellation shape syntax to STDOUT.  Diagnostic
and progress data are printed to STDERR.

To generate a new `constellationship.fab` file:

```
# Create a Python virtualenv to work in
$ virtualenv -p /usr/bin/python3 python_virtualenv
$ . python_virtualenv/bin/activate
$ pip install astroquery percache

# Generate the constellationship.fab file
$ ./generate_constellationship.py < SnT_constellations.txt > constellationship.fab

# (optional) force doing new Vizier queries
$ rm .hip_cache
```

### Data lookup and caching

The script queries the VizieR service (http://vizier.u-strasbg.fr/viz-bin/VizieR)
to translate the RA/Dec line endpoints in the S&T data, to Hipparcos
catalog IDs used by Stellarium.  Since querying VizieR can take a while
(a few seconds for each star), a cache is implemented so that previous
results are cached in the `.hip_cache` file for use by subsequent
runs of the script.  If you make changes to the `get_hip()` function,
you should delete the `.hip_cache` file to ensure that you don't return
cached values.

### Translation caveats

#### Line weight

Sky & Telescope draws constellation lines with various line weights.
For example, the "teaspoon" asterism to the east of the "teapot" of
Sagittarius is drawn using lighter lines than the primary constellation.

Stellarium does not have any way to draw different weights of constellation
lines, so `generate_constellationship.py` simply draws all of them. It
would be relatively easy to build in a filter so that it only includes
the heaviest weighted lines, but for the purposes of trying to make
Stellarium's lines look the most like what you see in a Sky & Telescope
atlas, having all of the lines is preferable.

#### Non-constellation stars

Sky & Telescope uses a convention of terminating constellation lines
that end at a star outside the constellation (for example, Alpheratz
in Pegasus) a half-degree short of the target star.  Stellarium's
constellation line drawing feature requires that all lines terminate
at stars.

