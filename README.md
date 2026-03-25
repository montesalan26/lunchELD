# Lunch Auditor

`Lunch Auditor` is a Windows desktop Tkinter application for reviewing Creative Energy shift data, lunch records, and Samsara ELD/HOS logs in one place.

It is built for an exception-review workflow: load the CE files, fetch ELD, inspect mismatches and lunch issues, preview the report inside the app, and export a Word document when the report looks right.

## Main Features

- Loads a shift Excel file and a lunch Excel file.
- Joins the shift and lunch files into the main Creative Energy review table.
- Fetches Samsara ELD/HOS data for a selected date range.
- Shows Creative Energy and ELD side by side with a movable divider.
- Highlights lunch violations, missing clock outs, and CE vs Samsara timing mismatches.
- Adds per-driver Samsara totals rows for driving and total work hours.
- Tracks `Work Hours Exceeded` when:
  - driving hours are over `10:00`
  - or total work hours are over `16:00`
- Includes clickable top summary cards for:
  - `Drivers Checked`
  - `Lunch Violations`
  - `Missing Clock Outs`
  - `Work Hours Exceeded`
- Includes additional review windows for:
  - `Lunch Verify`
  - `Samsara-CE Mismatch`
  - `Pre-Trip/Post Trip`
  - `Lunch Violation History`
  - `Driver Shift Assignments`
- Generates a report preview inside the app before export.
- Exports the previewed report to a Word `.docx` file.

## Current Report Flow

The sidebar button is `Preview Report`, not direct export.

When you click it:

1. The app builds the report data.
2. A preview window opens in a document-style layout.
3. You can review each section before export.
4. You can:
   - click a row to select it
   - press `Delete` to remove the selected row
   - right-click a cell to edit its text
5. The summary at the top updates from the current preview content.
6. Click `Export Word` to save the final `.docx`.

Empty sections are omitted from both the preview and the exported Word file.

## Report Sections

The report can currently include:

- `Work Hours Exceeded`
- `CE vs Samsara Mismatch`
- `Lunch Violations`
- `Missing Lunch Annotation on Samsara`
- `Pre-Trip/Post-Trip`

Notes pages and the old `Lunch on CE Outside of Samsara On Duty` section are not included anymore.

## Main Files

- [`excel_viewer.py`](./excel_viewer.py): main desktop application
- [`samsara_api.py`](./samsara_api.py): Samsara API helper
- [`shift_assignments.csv`](./shift_assignments.csv): saved driver shift assignments
- [`lunch_violation_history.csv`](./lunch_violation_history.csv): saved lunch violation history
- [`.env`](./.env): optional local configuration, including the Samsara API token

## Requirements

- Windows
- Python 3.13 recommended for the current packaged build
- Tkinter available in the Python install

Optional Python packages:

- `python-docx` for Word export
- `certifi` for SSL certificate handling with Samsara
- `pyinstaller` if you want to build the standalone `.exe`

## Setup

Install the optional packages you need:

```powershell
pip install python-docx certifi pyinstaller
```

Create a `.env` file in the project folder if you want local Samsara configuration:

```env
SAMSARA_API_TOKEN=your_token_here
```

You can also set `SAMSARA_API_TOKEN` as a normal environment variable.

## Run From Python

```powershell
python excel_viewer.py
```

## Build Standalone EXE

The current one-file build uses:

```powershell
python -m PyInstaller --noconfirm LunchAuditorOneFile.spec
```

The standalone executable is written to:

- [`dist/LunchAuditorOneFile.exe`](./dist/LunchAuditorOneFile.exe)

## Typical Workflow

1. Upload the shift Excel file.
2. Upload the lunch Excel file.
3. Review the joined Creative Energy table.
4. Set the Samsara date range if needed.
5. Click `Get ELD`.
6. Review the side-by-side CE and ELD tables.
7. Use the top summary cards and sidebar tools to inspect exceptions.
8. Click `Preview Report`.
9. Edit the preview if needed.
10. Click `Export Word`.

## Saved Local Data

The app writes and reuses local CSV/config files:

- [`shift_assignments.csv`](./shift_assignments.csv)
- [`lunch_violation_history.csv`](./lunch_violation_history.csv)
- [`lunch_violation_history_folder.txt`](./lunch_violation_history_folder.txt)

Runtime and packaged-app data may also be written under the user app-data folder depending on how the program is launched.

## Notes

- Word export requires `python-docx`.
- Samsara features require a valid API token and working network access.
- The app is tailored to the internal MJ Tank Lines / Creative Energy workflow and expected file layouts.
