# All Stolpersteins

## How to obtain the data needed

For projects like stolpersteine-in-bonn.de where you only need the stolpersteins in Bonn and the surrounding
area we are talking of an extent of a few hundred square kilometers. For that
[Overpass API](https://wiki.openstreetmap.org/wiki/Overpass_API) is a feasible option.

Europe and Russia combined cover an area of about 23.3 million square kilometers so using Overpass API is out
of the question. The obvious choice is to obtain all the raw OSM data for Europe and Russia import it into a
database and query that result. If you ever tried to import a large amount of data into an OSM-style database
you noticed that this approach will severely your computer and chances are it will give up because of memory
constraints.

As I have been doing that more than once I have been looking for an alternative that allows me to process the
whopping 25 Gigs of data on an inconspicuous Raspberry Pi 4 with 8 Gigs which is on par with an average office
PC. I actually found one that just requires a bit of time but not even gets close to running out of memory:
Use command line tools written in C.

Here is how to proceed:

First of all you need the raw data. I recommend using Geofabrik's excerpts - they are already stripped of the
author information:

* aria2c https://download.geofabrik.de/europe-latest.osm.pbf
* aria2c http://download.geofabrik.de/russia-latest.osm.pbf

Then you need to convert the data into the [o5m](https://wiki.openstreetmap.org/wiki/O5m) format

* osmconvert europe-latest.osm.pbf -o=europe-latest.o5m
* osmconvert russia-latest.osm.pbf -o=russia-latest.o5m

Finally you need to extract the stolperstein data

* osmfilter europe-latest.o5m --keep="memorial:type=stolperstein or memorial=stolperstein" -o=stolperstein-europe.osm
* osmfilter russia-latest.o5m --keep="memorial:type=stolperstein or memorial=stolperstein" -o=stolperstein-russia.osm

Feel free to replace aria2c by wget, curl, or the like.

## How to obtain the commands

* Aria2
  * https://command-not-found.com/aria2c
  * https://aria2.github.io/
* Curl
  * https://command-not-found.com/curl
  * https://curl.haxx.se/download.html
* OSMconvert
  * https://command-not-found.com/osmconvert
  * https://wiki.openstreetmap.org/wiki/Osmconvert
* OSMfilter
  * https://command-not-found.com/osmfilter
  * https://wiki.openstreetmap.org/wiki/Osmfilter
* Wget
  * https://command-not-found.com/wget
  * https://www.gnu.org/software/wget
  * https://eternallybored.org/misc/wget/

A rather current version of the osm files you get can be found in this directory or (in various compressed
forms) in the [download](./download) subdirectory.
