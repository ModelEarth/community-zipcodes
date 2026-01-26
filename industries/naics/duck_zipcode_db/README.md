# duck_zipcode_db

This folder contains older ZIP-code NAICS tooling and the DuckDB exports used to map ZIP codes to states.

## ZIP → State Mapping

THIS FILE IS NOT USABLE - Provides only city zips

`zip_data/duck_db_manager/database/exported_tables/DimZipCode.csv` provides:

- `GeoID` (ZIP code)
- `City`
- `State` (2‑letter abbreviation)

That file was used to split ZIP‑level outputs into per‑state folders, for example:

`US-AK-zipcodes-naics2-2023.csv`

The current `annual.py` ZIP scope AVOIDS using the DimZipCode.csv mapping to split ZIP output by state. The file zip-state-geonames.tsv pulled from [GeoNames](https://download.geonames.org/export/zip/) is used instead.
