TITLE
Patching Guidance for unittest.mock.patch


PURPOSE
This document defines the mandatory patching standard for unit tests using unittest.mock.patch.

Its goals:
- Prevent brittle patch targets
- Ensure correct call-time reference patching
- Avoid artificial coverage
- Enforce consistent patch style


SCOPE
Applies to all unit tests using unittest and unittest.mock.patch.


HARD RULES

1. Patch the reference used by the unit under test at call time.
   Never patch the original definition site if the unit holds a different reference.

2. The only permitted patch style is:

       patch_file = <reference used by unit under test>
       patch_function = patch_file.<attribute>.__name__
       with patch.object(patch_file, patch_function, ...):

3. Attribute names must be derived via __name__.
   Writing attribute names as string literals when using patch.object is forbidden.

4. patch_file must be the exact object reference used by the unit under test at runtime.

5. Patch only external nondeterminism or side effects
   (I/O, filesystem, network, time, randomness, environment, subprocess, external services).

6. Do not patch internal helpers, private functions, or deterministic logic.

7. Exception patching is allowed only when simulating a realistic failure mode of an external dependency already used by the code.

8. If the correct call-time reference cannot be confidently identified, do not proceed with patching.


PATCH TARGET SELECTION

- Identify where the dependency is referenced inside the unit under test.
- If imported under an alias, patch that alias.
- If accessed through a module attribute, patch that module reference.
- Always bind that reference to patch_file before patching.
- Never patch by import-path string when a direct object reference exists.


COMMON FAILURE MODES TO PREVENT

1. Patching the wrong module path.

   Incorrect:
       Patching src.utils.excel_io.write_sheets_to_excel

   When the unit actually uses:
       src.exporters._excel_file.excel_io.write_sheets_to_excel

2. Patching the original definition site instead of the call-time reference.

3. Deriving __name__ from the wrong object reference.

   Using __name__ correctly does not compensate for patching the wrong object.


CANONICAL PATCHING PATTERN (REQUIRED)

Example:

    patch_file = ef.excel_io
    patch_function = patch_file.write_sheets_to_excel.__name__

    with patch.object(
        patch_file,
        patch_function,
        side_effect=Exception("boom"),
    ):
        ...


EXCEPTION PATCHING RULES

- The failure must represent a realistic external dependency failure.
- The unit under test must already handle or propagate the exception.
- Do not invent artificial failure paths.
- Do not patch internal logic to throw exceptions.


FACADE / INTERFACE MODE INTERACTION

When operating in facade/interface mode, patching must not violate interface coverage constraints.
Do not patch internal behavior to artificially satisfy coverage.


PATCH TARGET CHECKLIST

Before writing any patch, confirm:

1. Where is the dependency referenced at call time?
2. Is patch_file the exact runtime reference?
3. Is patch_function derived via __name__ from that reference?
4. Is the dependency an external side-effect boundary?
5. Is the failure mode realistic (if using side_effect)?
6. Would this patch alter internal logic rather than isolate an external boundary?

If any answer is unclear, do not proceed with patching.
