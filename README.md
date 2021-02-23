# Open Track Data Standard
This document presents a simplified, generic [MIT Licensed](./LICENSE.md) data format for sensor systems that measure another object's position over time, also known as a "track". The name of this format is Open Track Data (OTD). It is meant to be agnostic to the sensing technology being used. Example systems that may be converted to this format include: primary radar, secondary radar, ADS-B, onboard GPS, etc.

The three main use-cases for the OTD are:

1. Data from a stationary sensor tracking a target vehicle. For example a ground based RF sensor detecting and tracking an aircraft.
2. Data from the target vehicle itself. For example the GPS track from a UAV.
3. Data from a sensor onboard one vehicle platform measuring another vehicle platform. The targets may be stationary or moving. For example a drone tracking a manned craft, or a ground sensor mounted to a vehicle on the move.

OTD is primarily intended for data at rest and uses [RFC-7464 jsonseq](https://tools.ietf.org/html/rfc7464) as its format.  It may also be useful as a streaming format in some situations.


The standard itself is comprised of 2 major documents:

1.  A json schema file describing the JSON structures in detail.
2.  A supplementary markdown file.

It also holds some basic examples of valid files used for parsing.

## Driving requirements
* Human readable
* Machine parsable
* Extensible, but with a required core
* Global, unambiguous coordinate system
* UTC time format

## Known Drawbacks
* Space intensive (when uncompressed)
* Slow serialization / deserialization of JSON (especially if also validating against the schema)
* Aux fields are a "wild west"

## Contributing
Community contributions (bug reports, merge requests, enhancements) are welcome.  Over time our goal is to build a healthy ecosystem of open source converters and parsers.

## Project History
OTD originally denoted "Offline Data Format" and developed as a "middle man" format for internal analysis tools used at Hidden Level.  Since a need for such a format has been identified in industry, in February of 2021 the format was open sourced so that other collaborators can make it better over time.

And yes, we are well aware of [XKCD 927](https://xkcd.com/927/).

### 1.0.0
This is simply an exact replica of the Offline Track Data format.  It is intended to be a baseline for expansion to future versions with the help of the community.

## Roadmap
See [ROADMAP.md](./ROADMAP.md)