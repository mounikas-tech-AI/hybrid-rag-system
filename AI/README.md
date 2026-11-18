# Data Extractor

This project automates the process of parsing NTSB (National Transportation Safety Board) Aviation Accident Reports in PDF format. It uses Python to extract critical metadata (Accident Number, Date & Time) and locate specific sections (e.g., Pilot Information) across multiple reports, compiling the results into a structured CSV dataset.

## Features
- **Automated Dependency Setup**: Fast installation of OCR and PDF parsing libraries.
- **Metadata Extraction**: Regular-expression-driven parsing to isolate unique incident identifiers and timestamps.
- **Dynamic Section Detection**: Scans document contents to locate which pages contain detailed personnel records.
- **Batch Processing**: Scans an entire directory of PDFs and exports a consolidated `.csv` index.

---

## 1. Setup & Dependencies
Install the required Python packages for text processing and layout-aware PDF analysis.
