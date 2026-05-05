🧼 Payload Sanitizer








A lightweight, dependency-free Python library for sanitizing XML, JSON, NDJSON, CSV, and flat-file payloads in Infor ION pipelines.

📑 Table of Contents
🚀 Overview
🎯 Why This Exists
⚙️ Features
📦 Installation
🧠 How It Works
🚀 Quick Start
🔧 API Reference
📘 Usage Guidelines
🧪 Real-World Examples
🔢 Versioning
⚠️ Important Notes
✅ Compatibility
🚀 Overview

Payload Sanitizer is designed to clean and normalize payloads that commonly break integrations across:

XML (BODs, IDM documents)
JSON (ION API, REST services)
NDJSON (Data Lake, streaming data)
CSV / flat files

Built specifically for:

Infor ION
IDM
SX / CSD
Data Lake pipelines
Java / Progress backend integrations
🎯 Why This Exists

If you've worked with ION long enough, you've probably hit:

❌ Invalid XML characters breaking BOD processing
❌ Hidden Unicode characters causing mysterious failures
❌ JSON payloads that look fine but fail parsing
❌ CSV column shifts due to control characters

This library eliminates those problems before they hit your pipelines.

⚙️ Features
✅ Format-aware sanitization (XML, JSON, NDJSON, CSV)
✅ UTF-8 enforcement across all payloads
✅ Removes invisible Unicode (ZWSP, BOM, word joiners)
✅ Safe JSON handling via parsing (no regex hacks)
✅ Optional strict mode for clean, human-readable JSON
✅ Zero dependencies (perfect for ION scripting environments)
📦 Installation
Option 1: Copy into your project
payload_sanitizer/
Option 2: Package (future-ready)
pip install payload-sanitizer
🧠 How It Works
Format	Strategy
XML	Removes illegal XML 1.0 characters
JSON	Parses → cleans → re-serializes
NDJSON	Processes line-by-line
CSV	Removes control + invisible characters
Text	Generic Unicode + control cleanup
🚀 Quick Start
from payload_sanitizer import sanitize_payload

clean = sanitize_payload(raw_payload, strict=True)
🔧 API Reference
sanitize_payload(text, payload_type="auto", strict=False)

Auto-detects and sanitizes payloads.

Detection Rules
< → XML
{ or [ → JSON
Otherwise → text
sanitize_xml(text)
Removes illegal XML characters
Cleans invisible Unicode
Enforces UTF-8
from payload_sanitizer import sanitize_xml

clean_xml = sanitize_xml(raw_xml)
sanitize_json(text, strict=False)
Default (strict=False)
Safe for system integrations
Preserves escaped sequences
Strict Mode (strict=True)
Removes escaped control characters
Normalizes NBSP → space
Ideal for logs / debugging
from payload_sanitizer import sanitize_json

clean_json = sanitize_json(raw_json, strict=True)
sanitize_ndjson(text, strict=False)
Processes each line independently
Raises errors with line numbers
from payload_sanitizer import sanitize_ndjson

clean_ndjson = sanitize_ndjson(raw_ndjson)
sanitize_csv(text)
Removes control characters
Prevents column shifting
Cleans invisible Unicode
from payload_sanitizer import sanitize_csv

clean_csv = sanitize_csv(raw_csv)
📘 Usage Guidelines
Use Case	Function
XML / BOD	sanitize_xml()
JSON API	sanitize_json(strict=False)
Debug / Logs	sanitize_json(strict=True)
Data Lake (NDJSON)	sanitize_ndjson()
CSV / Flat Files	sanitize_csv()
Unknown payload	sanitize_payload()
🧪 Real-World Examples
🔹 Fixing Broken XML (ION BOD)
raw_xml = "<Name>Test\u0001</Name>"

clean_xml = sanitize_xml(raw_xml)
🔹 Cleaning API JSON Before Sending to ION
raw_json = '{"name": "Rob\u000B"}'

clean_json = sanitize_json(raw_json)
🔹 Preparing Data Lake NDJSON
clean_ndjson = sanitize_ndjson(raw_ndjson, strict=True)
🔹 Fixing CSV Import Issues
clean_csv = sanitize_csv(raw_csv)
🔢 Versioning
import payload_sanitizer

print(payload_sanitizer.__version__)

💡 Log this once per execution for traceability.

⚠️ Important Notes
🚫 Do not use regex-only cleanup for JSON
XML follows XML 1.0 rules (not 1.1)
Invisible Unicode removal is intentional

Designed for:

ION → IDM → SX → Data Lake
✅ Compatibility
Python 3.9+
Infor ION Script Libraries
No external dependencies
