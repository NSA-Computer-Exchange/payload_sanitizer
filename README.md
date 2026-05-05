
Payload Sanitizer Library
Library name: payload_sanitizer

Purpose: Sanitize inbound and outbound payloads to remove characters that commonly break XML, JSON, CSV, and flat-file processing in Infor ION environments (ION, IDM, SX, Data Lake, Java services, and Progress backends).

This library is pure Python (no external dependencies) and is compatible with Python 3.9–3.12.

Design Principles
·       Format-aware sanitization for XML, JSON, NDJSON, and CSV

·       UTF-8 enforced for all payloads

·       Invisible Unicode removed (ZWSP, BOM, word joiners)

·       JSON payloads are parsed and re-serialized (no regex-only cleanup)

·       Optional strict mode for human-readable JSON output

Public API
sanitize_payload(text, payload_type="auto", strict=False)
Automatically sanitizes a payload based on the specified type or detected format.

Parameters:

Parameter

Type

Description

text

str

Raw payload text

payload_type

str

auto, xml, json, ndjson, csv, text

strict

bool

JSON/NDJSON only; removes escaped controls and normalizes NBSP

Auto-detection rules:

·       Payload starts with '<' -> XML

·       Payload starts with '{' or '[' -> JSON

·       Otherwise -> generic text sanitization

Example:

from payload_sanitizer import sanitize_payload

clean = sanitize_payload(raw_payload, strict=True)

sanitize_xml(text)
Sanitizes XML payloads according to XML 1.0 rules.

Removes:

·       Illegal XML control characters (SOH, VT, etc.)

·       Invisible Unicode (ZWSP, BOM)

·       Non-UTF-8 bytes

Example:

from payload_sanitizer import sanitize_xml

clean_xml = sanitize_xml(raw_xml)

sanitize_json(text, strict=False)
Sanitizes JSON safely by parsing and re-serializing the payload.

strict=False (default):

·       Produces standards-compliant JSON

·       Preserves escaped control sequences (example: \u0001)

·       Recommended for system-to-system integrations

strict=True:

·       Removes escaped control sequences (example: \u0001, \u000B)

·       Normalizes non-breaking spaces (NBSP -> space)

·       Recommended for logs and human-readable exports

Example:

from payload_sanitizer import sanitize_json

clean_json = sanitize_json(raw_json, strict=True)

sanitize_ndjson(text, strict=False)
Sanitizes newline-delimited JSON (NDJSON) line-by-line.

Behavior:

·       Each line is parsed independently

·       Raises an exception with the line number when a line is invalid JSON

·       Supports strict mode (same behavior as sanitize_json)

Example:

from payload_sanitizer import sanitize_ndjson

clean_ndjson = sanitize_ndjson(raw_ndjson, strict=True)

sanitize_csv(text)
Sanitizes CSV and flat-file payloads.

Removes:

·       Control characters that cause column shifting

·       Invisible Unicode characters

·       Non-UTF-8 bytes

Example:

from payload_sanitizer import sanitize_csv

clean_csv = sanitize_csv(raw_csv)

Usage Guidelines
Payload Type

Recommended Function

XML / BOD

sanitize_xml()

JSON API

sanitize_json(strict=False)

Human-readable JSON

sanitize_json(strict=True)

NDJSON

sanitize_ndjson()

CSV / Flat files

sanitize_csv()

Unknown / Mixed

sanitize_payload()

Versioning
The library exposes its version:

import payload_sanitizer
payload_sanitizer.__version__

Recommended to log the library version once per script execution.

Important Notes
·       JSON payloads must be parsed; regex-only cleanup is unsafe for JSON.

·       XML sanitization follows XML 1.0 (not XML 1.1).

·       Invisible Unicode characters are removed intentionally.

·       Designed for ION -> IDM -> SX -> Data Lake pipelines.

Compatibility
·       Python 3.9+

·       Infor ION Script Libraries

·       No external dependencies
