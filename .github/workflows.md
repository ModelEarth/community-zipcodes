# Zipcode Data Management Workflow

This workflow is designed to manage and maintain data updates in the repository by checking for missing years, populating data, exporting it, and validating the exported files.

## Workflow Triggers

The Data Management Workflow is triggered by two events:
- **Scheduled Runs:** This workflow is intended to run automatically at midnight on the 1st of every month (UTC) - but it doesn't.
- **Manual Trigger:** The workflow can also be manually triggered using the GitHub UI.

## Workflow Jobs

### 1. Checkout Code
- Uses the `actions/checkout` action to check out the repository code.

### 2. Set Up Python
- Sets up a Python environment using `actions/setup-python`.
- The Python version used is `3.8`.
- **Important Note:** The Python **environment** is set to the **root of the repository**. All paths for the Python files run by the scripts are set relative to the root of the repository.

### 3. Install Dependencies
- Installs required Python packages from the `requirements.txt` file.

### 4. Check for Updates
- Runs the `check_for_updates.py` script located in the `.github/scripts/` folder.
- Outputs the missing years to a file called `gap_years.txt` and sets an environment variable `GAP_YEARS` for further processing.

### 5. End Workflow if No Missing Years
- Checks if there are any missing years detected. If none are found, the workflow ends gracefully.

### 6. Populate Missing Years
- Executes the `populate_data.py` script for each missing year found.

### 7. Export Data
- Runs the `export_data.py` script to export the populated data for each missing year.

### 8. Validate Export
- Validates the exported data by running the `run_validation_test.py` script for each year.
- Outputs results and sets an environment variable `VALIDATION_SUCCESS` to indicate whether all validations passed.

### 9. Commit and Push Changes if Validation Passes
- If all validations succeed, it commits and pushes the changes back to the remote repository.

## Folder Structure

```plaintext
.github/
├── workflows.md                    # Our Zipcode Data Management Workflow Documentation
├── scripts/
│   ├── check_for_updates.py     # Checks for missing years in the dataset
│   ├── populate_data.py         # Populates missing years in the database
│   ├── export_data.py           # Exports data to CSV files
│   └── run_validation_test.py    # Validates the exported data for accuracy
└── workflows/
    └── data_management.yml      # GitHub Actions workflow configuration
