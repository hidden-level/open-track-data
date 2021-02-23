# Open Track Data - Frequently Asked Questions

## Why use the ECEF coordinate system?
ECEF has a key advantage of being a global reference frame that is unambiguous with respect to altitude.    Often when reporting coordinates, folks confuse MSL heights, Ellipsoidal Heights, AGL heights etc, or worse still the user is forced to figure it out for themselves.  ECEF also provides a handy cartesian frame of reference when doing various calculations and modeling between points.

The big drawback is that these coordinates are unusable by humans.  As such, the format specifies auxiliary fields which allow this data to be reinjected into a file.

And of course the "garbage in, garbage out principle" applies.  If a parser generating OTD does not use or interpret the input data source correctly, the resulting track will also not have a proper altitude.
