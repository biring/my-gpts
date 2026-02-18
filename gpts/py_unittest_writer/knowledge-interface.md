### PURPOSE
This document defines the canonical approach for interface (façade-level) tests using unittest.

Interface tests here are integration-style tests: they execute the public callable with real, representative data and real collaborating components, while still keeping assertions limited to interface-level contracts.

### SCOPE
Use this guidance only when the unit under test is intended to be an interface/façade surface.

If the provided code is not clearly an interface/façade, follow the core unit-test instructions instead.

### CORE PRINCIPLE
Interface tests verify only interface-level contracts:

1. The callable exists and is reachable from the intended public import path.
2. The callable can be invoked using realistic, minimal, known-good inputs.
3. The call succeeds on the happy path.
4. The return type matches the documented or evidenced contract.
5. Raise behavior is tested only when the callable itself explicitly raises (or the public contract explicitly requires a raise path).

### INTERFACE TESTS MUST

1. Use unittest only.
2. Treat each public function OR public method as an independent unit of work.
3. Create exactly one unittest.TestCase per callable.
4. Provide test methods focused on interface-level behavior:
   a) Happy-path test method
   b) Raise-path test method (only when allowed; otherwise do not include it)
5. Use Arrange–Act–Assert markers as inline comments: `# ARRANGE`, `# ACT`, and `# ASSERT`
6. Wrap all assertions in `self.subTest()`.
7. Keep assertions limited to reachability/callability, invocation success/failure per contract, and return type.
8. Use fixtures via setUp and tearDown when the callable requires real resources or real data.

### INTEGRATION-STYLE FIXTURE REQUIREMENTS

1. Prefer real collaborating components over mocks.
2. Use real, representative data (not placeholders) consistent with the contract.
3. Keep fixtures deterministic and self-contained.
   a) Do not rely on live external services.
   b) Do not rely on developer machine state.
4. setUp is responsible for creating the fixture state needed by the callable.
5. tearDown is responsible for cleaning up all resources created by setUp and by the test.
6. If real resources are used (files, directories, sockets, temp locations), they must be created in setUp and removed in tearDown.
7. If the required fixture data or environment cannot be created deterministically from what is provided, stop and request the missing fixture files or supporting modules.

### INTERFACE TESTS MUST NOT

1. Re-test internal logic, transformation details, or algorithmic correctness.
2. Assert deep field-level values, ordering, or exact contents.
3. Depend on internal helper functions, private attributes, or implementation details.
4. Patch internal logic to force coverage.
5. Patch unless strictly necessary to control an external nondeterministic dependency already used by the callable.

### TEST CLASS AND METHOD NAMING (MANDATORY)

One callable per test class
* For a public FUNCTION named <function_name>: Test class name: Test<function_name>
* For a public CLASS METHOD named <method_name> on class <ClassName>:Test class name: Test<ClassName><method_name>. Rationale: method names are not globally unique; include the owning class. No underscore is permitted between class name and method name.

### ASSERTION SCOPE (MANDATORY)

Happy path assertions must include, at minimum:

1. Existence/reachability and callability check
* For functions: verify getattr(module, name, None) is callable
* For methods: verify getattr(class_or_instance, name, None) is callable

2. Return value assertion (assert return value first)
* If contract returns None: assert is None
* Otherwise: assert return type only using assertIsInstance or type equality

### CONSTRUCTOR / INSTANTIATION HANDLING (CLASS METHODS)

If the unit under test is an instance method and an instance is required:
1. Instantiate the class using realistic minimal known-good inputs evidenced by the code.
2. Assert instance type only (isinstance).
3. Invoke the method and assert return type only.
   Do not assert internal state, fields, or side effects unless explicitly observable and explicitly part of the interface contract.

### CUSTOM OBJECT RETURNS (RESTRICTED)

If the callable returns a custom object:

1. Always assert the returned object type.
2. Do not compare fields unless the interface contract explicitly requires field-level verification.
3. If the contract explicitly requires field-level verification, compare only the required fields, one subTest per field. If unclear, type assertion only.

### ORDERING WITHIN EACH TEST CLASS

1. test_happy_path
2. test_raises (only when allowed)

### MINIMAL CANONICAL PATTERNS

PATTERN 1 — FUNCTION INTERFACE (HAPPY PATH)

* Verify callable exists
* Call with minimal known-good inputs
* Assert return value and return type only

PATTERN 2 — CLASS METHOD INTERFACE (HAPPY PATH)

* Verify class exists
* Instantiate class with minimal known-good inputs
* Verify method exists
* Call method
* Assert return value and return type only

PATTERN 3 — RAISE TEST (ONLY WHEN ALLOWED)

* Use the Raise Style Guide capture-and-assert pattern
* Trigger the raise condition using realistic inputs consistent with the signature

### QUICK COMPLIANCE CHECKLIST

* unittest only
* One test class per callable
* test_happy_path present
* test_raises present only when allowed
* AAA comments present
* All assertions wrapped in self.subTest
* No deep correctness assertions
* No internal implementation coupling
