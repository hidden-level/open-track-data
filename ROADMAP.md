# Open Track Data Roadmap

## Project Mission and Summary
OTD seeks to build some level of industry concensus around a good file format. 


## Infrastructure thoughts
* Autobuild
  - Verify Json schema valid
  - Convert JSON schema to user-friendly HTML pages
  - Generate a standalone PDF file for distribution

## Ecosystem thoughts
* Hidden Level has a number of internal parsers / reference libraries (python).  These should be released when feasible.
* Create index to various derived projects
* Clearing house document of aux fields added by different parsers from various sensing systems

## Initial 2.0.0 thoughts
* Initially the sensor location was an integral part of the structure, with sensor information being at the top level as a peer to track.  This got moved to the aux field, and the structure seems somewhat awkwardly nested now.  The biggest thing we delineate with is that the top level aux tends to be metadata about the file vs about the track itself.
* Could consider standardizing on geojson objects for better interoperability.  Would follow the Hidden Level AMS API.
* Support for 2D sensor systems could be added.  Perhaps rename the top level "3d_track" and then defined something for a 2D source.  This would likely be a reference point + Az/El combo.
* Perhaps push time into the track object.  That would allow multiple updates at different times.
* Consider allowing track updates to be repeated fields.
* More tightly define classification ontology.