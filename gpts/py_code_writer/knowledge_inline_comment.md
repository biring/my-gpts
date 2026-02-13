TITLE
Python Code Review Standards – Commentary and Examples

PURPOSE
This document provides explanations, rationale, and examples supporting the Core GPT Instructions.
It does not override or weaken any rule.
If conflicts exist, the Core GPT Instructions take precedence.

INLINE COMMENTING GUIDANCE

Good inline comments explain intent, invariants, assumptions, and failure modes.
They clarify why logic exists and why it is safe.
Repeat intent across docstrings and inline comments
Comment multiple lines within the same logical block
Explicitly restate invariants before and after enforcement
Avoid only comments that restate Python syntax with no semantic value.

Example:
# Exit early once all required identifiers are found to avoid unnecessary scanning
if len(found) == len(required):
    return

Bad inline comments restate mechanics or narrate syntax.

Bad example:
# Increment i by one
i += 1

DOCSTRING EXAMPLES

Good function docstring:

def read_metadata(...):
    """
    Read metadata values relative to labeled cells.

    Resolves labels using normalized lookup and applies row and column offsets.
    In strict mode, missing labels or out-of-bounds offsets raise exceptions.

    Args:
        label_offsets (dict[str, tuple[int, int]]): Mapping of label to row and column offsets.
        strict (bool, optional): Whether to raise on missing labels or bounds errors. Defaults to True.

    Returns:
        dict[str, str]: Mapping of label to resolved metadata value.

    Raises:
        KeyError: If a label is not found and strict is True.
        IndexError: If an offset resolves outside bounds and strict is True.
    """

Weak docstring to avoid:
"""Reads metadata."""

CLASS DOCSTRING EXPECTATIONS

Class docstrings should explain:
- Responsibility and scope
- Invariants enforced by the class
- Key attributes owned or mutated

Example:

"""
Wrapper around a pandas DataFrame that enforces a metadata template.

Validates the presence of required labels and provides offset-based
read and write access to metadata values.

Invariants:
- Template identifiers are unique after normalization.
- First occurrence of a label is authoritative.

Attributes:
    df (pd.DataFrame): Backing DataFrame.
    template_identifiers (tuple[str, ...]): Required template labels.
"""

WHAT COUNTS AS NON-TRIVIAL

Automatically non-trivial:
- Validation or verification logic
- Loop-based scanning
- Normalization or mapping
- Strict vs non-strict behavior
- Indexing or bounds checks
- Mutation of state or external objects

Possibly trivial:
- One-line getters
- Simple pass-through wrappers with no branching

ERROR AND RAISES GUIDANCE

- Always list exceptions explicitly raised by the code.
- Include IndexError or KeyError only if caused by explicit checks.
- Do not list pandas or library-internal exceptions unless caught or wrapped.

WHY THIS SPLIT EXISTS

- Core instructions stay under platform limits.
- Knowledge document preserves rigor and institutional knowledge.
- Rules remain enforceable; explanations remain flexible.
