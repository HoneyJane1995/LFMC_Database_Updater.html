# LFMC Database Updater

A browser-based tool for updating the NRT Fire Research master database with monthly ArcGIS Survey123 field exports. No installation required — open the HTML file in any browser and run.

---

## What it does

Each month, ArcGIS Survey123 generates two export files from field surveys. This tool reads those files and appends new records into the correct sheets of the Fire Master Database, preserving all existing formatting, colours, and styles.

| Survey123 Export | Sheets Updated |
|---|---|
| `LFMC_Plot_Data_*.xlsx` | 01_PLOT_DATA_SHEET, 02_FLS_CWD, 03_PLOT_COVERAGE, 04_PLOT_SPECIES_COVERAGE |
| `LFMC_Field_Survey_*.xlsx` | 30_LFMC_PLOT_ATTRIBUTES, 31_LFMC_SPECIES_ATTRIBUTES, 32_FMC_MEASUREMENTS |

---

## How to use

1. Open `LFMC_Database_Updater.html` in a web browser (Chrome or Edge recommended)
2. Upload the **Fire Master Database** (`Fire_MasterDatabase_*.xlsx`)
3. Upload one or both Survey123 exports for the month
4. Choose a duplicate handling mode (see below)
5. Click **Run update**
6. Download the updated database file

---

## Duplicate handling modes

| Mode | Behaviour |
|---|---|
| **Skip** *(default)* | Existing records matched by GlobalID are left untouched. New records are appended. |
| **Overwrite** | Existing records are updated in place with values from the new export. |
| **Append** | All records from the export are added regardless of duplicates. |

Records are matched primarily by **GlobalID** (from Survey123). If a GlobalID is not available, the tool falls back to a composite key of `LOCATION_PLOT_NUMBER` + relevant identifiers per sheet.

---

## Field survey sheet routing

Survey123 exports the Field Survey file with sheets in a fixed order. The tool routes them as follows:

| Sheet index | Survey123 sheet name | Master database target |
|---|---|---|
| 0 | `1_0` (plot attributes) | `30_LFMC_PLOT_ATTRIBUTES` |
| 1 | `species_sample_attributes_1` | `31_LFMC_SPECIES_ATTRIBUTES` |
| 2 | `fuel_moisture_samples_2` | `32_FMC_MEASUREMENTS` |

---

## Notes

- The tool runs entirely in the browser — no data is sent to any server
- All existing sheet formatting, header colours, and fonts in the master database are preserved
- String values from Survey123 (e.g. trailing newlines from text fields) are automatically cleaned before writing
- The output file is named `Fire_MasterDatabase_Updated_YYYYMMDD.xlsx`
- Computed fields: `FRESH_WT` (net fresh weight) is calculated as `Fresh Weight − Container Tare`

---

## Files

```
LFMC_Database_Updater.html    Main application — open this in a browser
README.md                     This file
```

---

## Maintainer

HoneyJane1995 — NRT Fire Research  
Built for integration with ArcGIS Experience Builder via the Embed widget.
