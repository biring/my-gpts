# Python Module Header Docstring — Knowledge Reference

This file defines canonical structure, formatting, and examples for generating module-level header docstrings for Python source files.

This file is authoritative. If conflicts arise, this file takes precedence over inferred conventions or external standards.

---

## Canonical Docstring Structure

A valid module-level header docstring MUST follow this structure exactly,
with one blank line between sections and no wrapped lines.

1. Description
2. Key Responsibilities
3. Example Usage
4. Dependencies
5. Notes
6. License

The docstring must be the first statement in the file and must use triple quotes.

Immediately after the closing docstring, the module must declare:

__all__ = []

---

## Description Section

Rules:
- First line: one concise summary sentence.
- Second paragraph: one short paragraph describing purpose and context.
- No symbol names.
- No implementation details.

Example:

"""
Utility functions for sanitizing and normalizing text inputs.

This module provides reusable helpers for cleaning, normalizing, and preparing
raw text data for downstream processing pipelines.
"""

---

## Key Responsibilities Section

Rules:
- Bullet list only.
- Evidence-based.
- Conservative phrasing.
- Single-tab indentation.
- No speculation.

Example:

Key Responsibilities:
	- Remove non-printable or control characters from input text
	- Normalize whitespace for consistent downstream processing

---

## Example Usage Section

Rules:
- Exactly two examples.
- Imports + one simple function call per example.
- No additional explanation text.
- Single-tab indentation.

If module is public:

Example Usage:
	# Preferred usage via public package interface:
	from package import api
	result = api.process_text(raw_text)

	# Direct module usage (acceptable in unit tests or internal scripts only):
	import package._internal_module as internal
	result = internal.process_text(raw_text)

If module is private and no public interface exists, replace the first example with:

	Not applicable. This is an internal module and should be accessed via a façade when exposed.

---

## Dependencies Section

Rules:
- Bullet list only.
- Single-tab indentation.
- List only what is directly imported or implied by syntax.

Example:

Dependencies:
	- Python version: >= 3.9
	- Standard Library: re, string

---

## Notes Section

Rules:
- Bullet list only.
- Design intent and usage assumptions only.
- No future intent.
- No recommendations unless required for correctness.

Example:

Notes:
	- Intended for internal use within text processing utilities
	- Separates text cleanup from structural parsing concerns

---

## License Section

Rules:
- Must be labeled exactly as shown.
- No additional text beyond the bullet.

Example:

License:
	- Internal Use Only

---

## Stable Docstring Criteria

A docstring is considered stable if:
- All required sections are present and ordered correctly
- Formatting invariants are respected
- Content accurately reflects observable code behavior
- No required sections are missing or incorrect

If a docstring is stable, it must not be rewritten.

The only allowed response in this case is:

No update needed. Existing docstring is stable and compliant.

---

## Canonical Full Example

"""
Utility functions for sanitizing and normalizing text inputs.

This module provides helpers for removing control characters, normalizing
whitespace, and preparing raw text for downstream processing.

Key Responsibilities:
	- Remove non-printable characters from input text
	- Normalize whitespace for consistent processing

Example Usage:
	# Preferred usage via public package interface:
	from src.utils import interfaces as utils
	clean = utils.remove_all_whitespace(raw_text)

	# Direct module usage (acceptable in unit tests or internal scripts only):
	import src.utils._text_sanitizer as sanitizer
	clean = sanitizer.remove_all_whitespace(raw_text)

Dependencies:
	- Python version: >= 3.9
	- Standard Library: re, string

Notes:
	- Intended for internal use within the utils package
	- Designed for use in text preprocessing utilities

License:
	- Internal Use Only
"""
__all__ = []
