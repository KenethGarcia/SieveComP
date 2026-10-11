# SieveComP Web Interface (Frontend)

The SieveComP Frontend (`app.py`) is a powerful, interactive Streamlit application designed for users who prefer a graphical interface over the command line. It wraps the core SieveComP engine in a user-friendly dashboard, allowing analysts to visually filter catalogs, execute complex rule checks, manually review anomalous events, and maintain a persistent audit log of their work.

# Execution

Run the Streamlit app from the root directory of the package:

```bash
streamlit run examples/frontend/app.py
```

# Key Features & Navigation

The interface supports seamless toggling between English (EN) and Spanish (ES) and is divided into two primary views accessible via the sidebar:

## **Current Review**
This is the main dashboard for executing the Seismicity Review Routine.

- **Search Parameters:** Define your query by selecting a specific Author (e.g., `scanloc`, `kgarcia`) and precise Start/End Date and Time windows. The app automatically routes your query across SeisComP3 and SeisComP6 databases based on the internal `2026-03-17` temporal cutoff.
- **Dynamic Check Configuration:** A dedicated popover menu allows you to selectively enable or disable any of the 30 built-in TOML checks. The checks are cleanly grouped into logical categories (e.g., Quality & Uncertainty, Velocity Models, Magnitudes by Zone).
- **Locatable Events Toggle:** A quick-access dropdown lets you instantly evaluate or ignore the high-volume "Potentially locatable event" rule without digging into the detailed configuration.
- **Interactive Data Editor:** Once the engine runs, results are presented in interactive tables. You can check the "Reviewed" (Revisado) box, type custom comments into the "Notes" (Notas) column, and immediately see translated observations.
- **Save & Audit:** Click "Save Reviewed Events" to append all checked rows directly to a local persistence file (`historical_revision_log.csv`), automatically stamping them with your Reviewer Name and the current timestamp.

## Revision History
A dedicated historical view that loads the `historical_revision_log.csv` file. It displays a chronological ledger of every event previously marked as "Reviewed," complete with the reviewer's name, the date of review, user-added notes, and the specific anomalies (Observations) that flagged the event in the first place.

## Localization
Thanks to the integrated `translations.py` dictionary, the entire UI (including table headers, backend flagged observations, and categorized rule names) dynamically translates between English and Spanish based on the toggle in the upper right corner. This ensures the app is highly accessible to both local network operators at RSNC and international reviewers.