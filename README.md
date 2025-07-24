
# Minneapolis City Directory OCR Parser

This project extracts structured resident data from scanned pages of the *Minneapolis City Directory (1900)* using the Google Cloud Vision API and custom parsing logic. The output includes names, addresses, occupations, and annotations (e.g., moved, deceased) in machine-readable JSON format.

---

## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Authentication](#authentication)
- [Input Format](#input-format)
- [Usage](#usage)
- [Output Format](#output-format)
- [Directory Structure](#directory-structure)
- [Notes](#notes)
- [Acknowledgments](#acknowledgments)

---

## Features

- OCR via [Google Cloud Vision API](https://cloud.google.com/vision)
- Parsing of two-column historical scans
- Accurate extraction of:
  - Names (First, Middle Initial, Last)
  - Spouse names
  - Occupation and company
  - Residential and work addresses
  - Telephone numbers
  - Individuals marked as **moved** or **deceased**

---

## Installation

Install required packages:

```bash
pip install --upgrade google-cloud-vision
apt-get install -y tesseract-ocr
pip install pytesseract opencv-python-headless pillow
```

---

## Authentication

1. Create a Google Cloud Project.
2. Enable the **Vision API**.
3. Generate a service account key (JSON format).
4. Update your script:

```python
KEY_PATH = "/path/to/your-key.json"
```

---

## Input Format

Input images should be named sequentially:

```
104.jpeg, 105.jpeg, 106.jpeg, ...
```

Each file should represent a scanned page from the 1900 Minneapolis City Directory.

---

## Usage

1. Authenticate using your Google Cloud Vision API key.
2. Run the OCR pipeline:
   - Processes each page
   - Stitches text from left and right columns
   - Parses entries into structured data
3. Output files will be saved as JSON.

---

## Output Format

### Example `residents.json` entry

```json
{
  "FirstName": "John",
  "MiddleInitial": "F",
  "LastName": "Doe",
  "Spouse": "Mary",
  "Occupation": "Clerk",
  "CompanyName": "Smith Bros.",
  "HomeAddress": {
    "Indicator": "r",
    "StreetNumber": "123",
    "StreetName": "Main St"
  },
  "Telephone": "4567",
  "WorkAddress": "456 Market St",
  "DirectoryName": "Minneapolis 1900",
  "PageNumber": 104
}
```

### Example `deceased.json` entry

```json
{
  "Name": "Brown, Charles W",
  "Age": "58",
  "DateOfDeath": "Feb 23, 1899",
  "PageNumber": 108
}
```

---

## Address Indicator Reference

| Indicator | Meaning         |
|-----------|-----------------|
| r         | Resident        |
| b         | Boarder         |
| rms       | Rooms / Rented  |

---

## Directory Structure

```
.
├── 104.jpeg - 108.jpeg         # Input scanned pages
├── your-key.json               # GCP service account key
├── residents.json              # Final structured resident records
├── deceased.json               # Deceased individual records
├── moved.json                  # Relocated individuals
├── residents_combined.json     # Intermediate full dump
├── deceased_combined.json      # Intermediate full dump
└── parser_script.ipynb         # OCR + parsing script
```

---

## Notes

- Entries may contain abbreviations, inherited names (e.g., quotation marks), or incomplete lines that are automatically handled.
- Spouse names are detected from context (e.g., “& Mary”).
- Company names are distinguished from occupations using custom rules.
- Indicators like `r`, `b`, and `rms` are preserved and parsed.
- Advertisements and noise are filtered out.
- Deceased entries may include age and full death date.
- Individuals with "moved to" annotations are stored separately.

---

## Acknowledgments

- [Google Cloud Vision API](https://cloud.google.com/vision) for OCR
- [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) for preprocessing
- Source data: Public domain scans of the *Minneapolis City Directory (1900)*

---
