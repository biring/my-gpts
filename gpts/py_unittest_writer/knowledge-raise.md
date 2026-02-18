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

Assert message presence only (REQUIRED)
- Extract args using `actual_args = getattr(actual, "args", ())`
- Assert `bool(actual_args) and str(actual_args[0]) != ""`
- Never assert exact message text

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

### SUBTEST OUTPUT REQUIREMENTS
- Exception type comparison subTest name MUST be exactly:
  "Error"
- Exp and Act parameters MUST be provided
- Message presence subTest name MUST be exactly:
  "Message string is not empty"

### FORBIDDEN PRACTICES
- Using assertRaises or assertRaisesRegex
- Catching a specific exception type in except
- Asserting exact exception messages
- Asserting repr(exception) or str(exception) content
- Omitting actual = "" when no exception is raised
- Renaming or omitting Arrange–Act–Assert markers
- Combining multiple exception types in one test without subTest grouping

### MINIMUM ASSERTION REQUIREMENTS
A compliant raise test MUST:
- Assert exception type name equality
- Assert exception message presence only (non-empty)

