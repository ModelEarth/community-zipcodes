# About Zip Data

Our repository provides [zipcode-related economic data](https://github.com/ModelEarth/community-zipcodes/tree/main/industries/naics/US/zip), automatically updated via [GitHub Actions workflows](https://github.com/ModelEarth/community-zipcodes/blob/main/.github/workflows.md). The data is sourced from the U.S. Census API, processed, and stored in a DuckDB database, and exported to [CSV files](https://github.com/ModelEarth/community-zipcodes/tree/main/industries/naics/US/zip) for easy access. Developers can also explore the code to customize the data processing pipeline and also use our NAICS .csv files prepared for [the entire US, individual states and counties](/data-pipeline/industries/naics/).

At the zipcode level, employees and payroll are often omitted by the census to protect privacy.  
Our [NAICS Imputation using ML](/machine-learning) can be updated to estimate blank values.

## Table of Contents

- [About Zip Data](#about-zip-data)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Data Storage \& Structure](#data-storage--structure)
    - [Database Structure](#database-structure)
      - [Table: DimYear](#table-dimyear)
      - [Table: DimNaics](#table-dimnaics)
      - [Table: DimZipCode](#table-dimzipcode)
      - [Table: DataEntry](#table-dataentry)
    - [CSV Files](#csv-files)
  - [Accessing the Data](#accessing-the-data)
  - [For Developers](#for-developers)
  - [Documentation](#documentation)

## Overview

This repository is designed to provide users with up-to-date economic data by ZIP code, categorized by industry levels. Currently, the data is configured to include industry levels **2**, **5**, and **6**. Data is automatically fetched from the U.S. Census API, processed, and stored in a structured format, ensuring easy access and utilization.

## Data Storage & Structure

### Database Structure

The database is designed to store economic data related to various ZIP codes and industries. It consists of four tables: **DimYear**, **DimNaics**, **DimZipCode**, and **DataEntry**. Key constraints are not enforced in the current database to optimize data ingestion; however, data can be migrated to a database with constraints if needed in the future.

#### Table: DimYear
- **Year**: INTEGER, Primary Key - Represents the year.
- **YearDescription**: TEXT - Describes the NAICS code version for the year (e.g., 'NAICS2017' for 2017–2023).

#### Table: DimNaics
- **NaicsCode**: TEXT - Represents the NAICS code.
- **industry_detail**: TEXT - Provides a description of the industry.

#### Table: DimZipCode
- **GeoID**: TEXT, Primary Key - Geographic identifier (typically a ZIP code).
- **City**: TEXT - Corresponding city for the ZIP code.
- **State**: TEXT - Corresponding state for the ZIP code.

#### Table: DataEntry
- **EntryID**: INTEGER, Primary Key, AUTOINCREMENT - Unique identifier for each data entry.
- **GeoID**: TEXT - Foreign key referencing the GeoID in the DimZipCode table.
- **NaicsCode**: TEXT - Foreign key referencing the NaicsCode in the DimNaics table.
- **Year**: INTEGER - Foreign key referencing the Year in the DimYear table.
- **Establishments**: INTEGER - Number of establishments.
- **Employees**: INTEGER - Number of employees.
- **Payroll**: INTEGER - Total payroll.
- **IndustryLevel**: INTEGER - Indicates the level of detail in the industry classification.

**Relationships**:
- The **DataEntry** table references the **DimZipCode** table through the **GeoID** foreign key.
- The **DataEntry** table references the **DimNaics** table through the **NaicsCode** foreign key.
- The **DataEntry** table references the **DimYear** table through the **Year** foreign key.

### CSV Files

Data is exported to CSV files, stored in a structured directory. Files are named based on the state, industry level, and year:

```plaintext
industries/naics/US/zip/AK/US-AK-census-naics6-zip-2012.csv
```

- **US**: Indicates the country.
- **AK**: State code (Alaska in this case).
- **naics6**: Industry level 6.
- **2012**: Year of the data.

## Accessing the Data

Users can directly access the CSV files or the DuckDB database files to retrieve the data they need. It is recommended to access the DuckDB files directly for better efficiency, as this approach eliminates the overhead of creating pandas DataFrames from the CSV files. The files are updated regularly, ensuring that the latest data is always available in this repository.

## For Developers

Developers interested in modifying the data pipeline should fork the repository and work on their own branch. The main scripts involved in data processing are located in the `industries/naics/duck_zipcode_db/` directory:

- **Populator Scripts** (`industries/naics/duck_zipcode_db/populator`): These scripts are responsible for populating the DuckDB database with ZIP code data.

- **Exporter Scripts** (`industries/naics/duck_zipcode_db/exporter`): These scripts manage the export of data to CSV format.

**Deprecated Files**: Any deprecated files have been moved to a `deprecated` folder in the repo. If you need to use these files, ensure to create a new branch and move them back to the original location for testing or other purpose.

For detailed information on the configuration of the *data management workflow* (GitHub Actions), please refer to the [Data Management Workflow README](.github/README.md).

## Documentation

For detailed logic and implementation, refer to the inline documentation within the scripts located in the repository. This includes explanations for the populator and exporter scripts.

---

If you're looking to understand how data is automatically updated, check the [.github/workflows.md](https://github.com/ModelEarth/community-zipcodes/blob/main/.github/workflows.md) for details on the workflow.

