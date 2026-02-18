### PURPOSE

This document defines the required style for unittest tests that verify exceptions are raised ("raise tests").
The goal is to ensure raise tests:
- Follow a strict Arrange–Act–Assert structure
- Capture and assert the raised exception deterministically
- Produce consistent and debuggable subTest output

This guide must be followed in addition to the global unittest GPT instructions.

### SCOPE
- Applies to Python unit tests written with unittest
- Applies to built-in and custom exception types

### HARD RULES

Raise tests MUST include all three sections with these exact markers:
  - ARRANGE 
  - ACT 
  - ASSERT

Do NOT merge ACT and ASSERT into a single section.

Do not use assertRaises or assertRaisesRegex

Raise tests MUST capture exceptions using try / except. This keeps assertions explicit and consistent.

Capture the exception deterministically
- Call the unit under test exactly once in the try block
- If no exception is raised, set: actual = ""
- If an exception is raised, set: actual = e

Assert the exception type name (REQUIRED)
- expected_type = <ExceptionClass>.__name__
- actual_type = type(actual).__name__
- Assert equality using a subTest

Assert message presence (REQUIRED)

- Extract args using: `actual_args = getattr(actual, "args", ())`
- Assert message is non-empty: `bool(actual_args) and str(actual_args[0]) != ""`

Reason preservation (CONDITIONAL)

- When the unit under test wraps or normalizes an underlying exception,
  assert that the underlying reason string is preserved using assertIn.
- Do not assert exact message equality.
- Do not assert full formatted string equality.
- Do not require reason containment when no wrapped exception exists.

Raise tests must follow the global assertion-wrapping requirement (all assertions in subTest) and, in addition, must use the subTest names and Exp/Act fields specified in this guide.

Use realistic invalid inputs
- Trigger exceptions using inputs consistent with the function signature
- Do not invent impossible inputs
- If static typing complains, suppress only the affected line using: `# type: ignore[<code>]`

### CANONICAL RAISE TEST PATTERN

Use this exact pattern. Only variable names may change.

    def test_<condition>(self) -> None:
        """
        Should <expected behavior>.
        """
        # ARRANGE
        <setup>
        expected_type = <ExceptionClass>.__name__
        expected_reason: str | None = ... # Only set when unit under test wraps/normalizes an underlying exception

        # ACT
        try:
            <call unit under test>
            actual = ""
        except Exception as e:
            actual = e

        # ASSERT
        actual_type = type(actual).__name__
        with self.subTest("Error", Exp=expected_type, Act=actual_type):
            self.assertEqual(expected_type, actual_type)

        actual_args = getattr(actual, "args", ())
        with self.subTest("Message string is not empty"):
            self.assertTrue(bool(actual_args) and str(actual_args[0]) != "")

        # Optional: include only when expected_reason is set (unit wraps/normalizes an underlying exception)
        if expected_reason is not None:
            actual_message = str(actual)
            with self.subTest("Message contains reason"):
                self.assertIn(expected_reason, actual_message)


### SUBTEST OUTPUT REQUIREMENTS
- Exception type comparison subTest name MUST be exactly: "Error"
- Exp and Act parameters MUST be provided
- Message presence subTest name MUST be exactly: "Message string is not empty"
- Reason containment subTest name MUST be exactly: "Message contains reason"
- For "Message contains reason", Exp and Act SHOULD be provided when expected_reason is available

### FORBIDDEN PRACTICES
- Using assertRaises or assertRaisesRegex
- Catching a specific exception type in except
- Asserting exact exception messages
- Asserting repr(exception) or str(exception) content, except for the conditional “reason containment” assertion which MUST use assertIn and MUST NOT use exact equality
- Omitting actual = "" when no exception is raised
- Renaming or omitting Arrange–Act–Assert markers
- Combining multiple exception types in one test without subTest grouping

### MINIMUM ASSERTION REQUIREMENTS

A compliant raise test MUST:
- Assert exception type name equality
- Assert exception message presence (non-empty)

Additionally:
- Assert reason containment when the unit under test wraps or normalizes an underlying exception.


