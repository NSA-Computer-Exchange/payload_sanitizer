## Quick Start (TUG Attendees)

1. Click "Code"
2. Download ZIP (no Git required)

OR

git clone https://github.com/NSA-Computer-Exchange/payload_sanitizer

---

# Payload Sanitizer

## Overview

Payload Sanitizer is a lightweight, dependency-free Python library for sanitizing XML, JSON, NDJSON, CSV, and flat-file payloads.

It is designed for use in Infor ION scipting environments, including ION, IDM etc. but can also be used as a stand alone library.

---

## Why This Exists

Common issues in integration pipelines include:

- Invalid XML characters breaking processing
- Hidden Unicode characters causing failures
- JSON payloads failing parsing despite appearing valid
- CSV column shifts due to control characters

This library removes those issues before they impact downstream systems.

---

## Features

- Format-aware sanitization (XML, JSON, NDJSON, CSV)
- UTF-8 enforcement
- Removal of invisible Unicode characters (ZWSP, BOM, word joiners)
- Safe JSON handling via parsing and re-serialization
- Optional strict mode for human-readable JSON
- No external dependencies

---

## Quick Start

```python
from payload_sanitizer import sanitize_payload

clean = sanitize_payload(raw_payload, strict=True)
```

---

## API Reference

### sanitize_payload(text, payload_type="auto", strict=False)

Auto-detects and sanitizes payloads.

Detection rules:
- Starts with "<" → XML
- Starts with "{" or "[" → JSON
- Otherwise → text

---

### sanitize_xml(text)

- Removes illegal XML characters
- Cleans invisible Unicode
- Enforces UTF-8

```python
from payload_sanitizer import sanitize_xml

clean_xml = sanitize_xml(raw_xml)
```

---

### sanitize_json(text, strict=False)

Default (strict=False):
- Safe for system integrations
- Preserves escaped sequences

Strict mode (strict=True):
- Removes escaped control characters
- Normalizes non-breaking spaces

```python
from payload_sanitizer import sanitize_json

clean_json = sanitize_json(raw_json, strict=True)
```

---

### sanitize_ndjson(text, strict=False)

- Processes each line independently
- Raises errors with line numbers

```python
from payload_sanitizer import sanitize_ndjson

clean_ndjson = sanitize_ndjson(raw_ndjson)
```

---

### sanitize_csv(text)

- Removes control characters
- Prevents column shifting
- Cleans invisible Unicode

```python
from payload_sanitizer import sanitize_csv

clean_csv = sanitize_csv(raw_csv)
```

---

## Usage Guidelines

| Use Case             | Function                         |
|---------------------|----------------------------------|
| XML / BOD           | sanitize_xml()                   |
| JSON API            | sanitize_json(strict=False)      |
| Debug / Logs        | sanitize_json(strict=True)       |
| Data Lake (NDJSON)  | sanitize_ndjson()                |
| CSV / Flat Files    | sanitize_csv()                   |
| Unknown payload     | sanitize_payload()               |

---

## Real-World Examples

Fixing broken XML:

```python
raw_xml = "<Name>Test\u0001</Name>"

clean_xml = sanitize_xml(raw_xml)
```

Cleaning JSON:

```python
raw_json = '{"name": "Rob\u000B"}'

clean_json = sanitize_json(raw_json)
```

---

## Versioning

```python
import payload_sanitizer

print(payload_sanitizer.__version__)
```

---

## Important Notes

- JSON must be parsed; regex-only cleanup is unsafe
- XML sanitization follows XML 1.0
- Invisible Unicode characters are intentionally removed
- Designed for ION → IDM → CSD → Data Lake pipelines

---

## Compatibility

- Python 3.9+
- Infor ION Script Libraries
- No external dependencies
