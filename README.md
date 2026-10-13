# MetaData-Response

A curated collection of tools for creating, editing, evaluating, and plotting seismic
station metadata and instrument responses (FDSN StationXML, RESP, dataless SEED workflows),
plus a sample sanitized StationXML file for a strong-motion accelerometer deployment.

## Contents

| Directory / File | Description |
|---|---|
| `evalresp-main/` | EarthScope/IRIS **evalresp** — C program (with Python and Robot Framework test suites) that evaluates instrument response information and outputs amplitude/phase (fap) or complex spectra. Reads RESP text files and StationXML; outputs ASCII or xPlore-style files. |
| `JEvalResp-main/` | **JEvalResp** — Java utility (EarthScope/ISTI heritage) for evaluating and processing instrument response descriptions from RESP text files and StationXML (v1.80+), with text and graphical output. |
| `JPlotResp_v1.80_dist/` | **JPlotResp v1.80** — distribution of the Java plotting tool for graphically comparing instrument responses (amplitude/phase vs. frequency). |
| `yasmine-stationxml-editor-4.1.1-beta/` | **Yasmine** (Yet Another Station Metadata INformation Editor) v4.1.1-beta — Python (Flask) web application for creating and editing geophysical station metadata in FDSN StationXML format; joint IRIS / Résif development. ExtJS frontend, SQLAlchemy/Alembic backend. |
| `XX.STATION_CODE.xml` | Sample FDSN StationXML v1.2 file for a GAIA SIGMA accelerometer station (4 channels: HNZ/HNN/HNE/HNC, 100 sps), produced by GaiaCode-OMEGA. Station code and site coordinates have been redacted for privacy; response stages (poles/zeros, gains, decimation) are intact. |

## Technology Stack

- **C** — evalresp core (CMake/Make build, test suite in C + Python + Robot Framework)
- **Java 8+ / Ant** — JEvalResp, JPlotResp
- **Python 3 / Flask / SQLAlchemy / Alembic** — Yasmine backend
- **ExtJS (Sencha)** — Yasmine frontend

## Hardware Dependencies

None required to run the tools. The sample StationXML describes a **GAIA SIGMA**
strong-motion accelerometer (used with GaiaCode digitizers); response files produced by
these tools apply generically to seismometers/accelerometers and their digitizers.

## Setup & Installation

### evalresp (C)
```bash
cd evalresp-main
mkdir build && cd build
cmake ..
make
```

### JEvalResp / JPlotResp (Java)
Requires Java 8+ and Apache Ant:
```bash
cd JEvalResp-main
ant            # builds runnable jar(s)
```
`JPlotResp_v1.80_dist` is a pre-built distribution — see its enclosed documentation.

### Yasmine (StationXML editor)
```bash
cd yasmine-stationxml-editor-4.1.1-beta/backend
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
# see yasmine documentation inside the directory for run instructions
```

## Usage

- **evalresp**: `evalresp <net> <sta> <loc> <chan> <year> <jday> <freqmin> <freqmax> <nfreqs> -u <units>` — evaluates the response for a channel over a frequency range.
- **JEvalResp**: run the built jar with a RESP/StationXML file to evaluate or inspect responses.
- **JPlotResp**: GUI — load one or more RESP/StationXML files and compare amplitude/phase curves.
- **Yasmine**: web UI for editing StationXML networks/stations/channels and responses.

## Data Formats / Protocols

- **FDSN StationXML** (v1.x) — station metadata schema
- **RESP** — SEED-style ASCII response files (poles/zeros, coefficients, FIR stages)
- **miniSEED / dataless SEED** — referenced by the toolchain (bulk waveform data is excluded from this repo)

## Sanitization Notice

This repository was prepared for private hosting with the following redactions:
station code, site coordinates (lat/lon/elevation), and hardware unit identifiers in the
sample StationXML file. Large bulk StationXML test fixtures (>25 MB) are excluded via
`.gitignore`. Component projects retain their original upstream licenses and attribution.
