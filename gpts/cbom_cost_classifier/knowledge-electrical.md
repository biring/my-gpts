### PURPOSE [CORE]

This document defines how to break the Electrical Components category down into 15 fixed sub-categories and summarize them into a single table that reconciles back to the Electrical Components row in the top-level category table.

This document supplements the Core GPT Instructions with electrical-sub-category classification rules and examples. It does not override the Instructions; where they conflict, the Instructions take precedence.


### SCOPE [CORE]

__Applies to:__ CBOM line items already classified as Electrical Components (per knowledge-category.md).

__Does not apply to:__ Line items classified under Mechanical Components, Bare Board, Assembly and Test, or Others.


### RULES [CORE]

- Every Electrical Components line item must be assigned to exactly one of 15 sub-categories: Resistors (Commodity), Resistors (Specialty), Capacitors (Commodity), Capacitors (Specialty), Inductors (Commodity), Inductors (Specialty), Diodes, Transistors, ICs, Connectors, Crystals & Oscillators, LEDs, Switches & Relays, Fuses & Protection, Other Electrical.
- Output is a second table, shown below the top-level category table, with exactly three columns: Sub-Category, Count, Sum of Cost.
- Table has exactly 16 rows: one per sub-category (in the order listed above) plus a Total row. All 15 sub-category rows appear even when Count is 0.
- Count = number of line items assigned to that sub-category. A multi-quantity line item (e.g. qty 2) still counts as 1 line item, consistent with knowledge-category.md.
- Sum of Cost = sum of extended cost for all line items assigned to that sub-category, rounded to 2 decimal places.
- Immediately below the table, report two checks, reconciling against the Electrical Components row of the top-level table (not the whole CBOM): Sub-Count Check and Sub-Cost Check. Each reports PASS or FAIL.
- A check is FAIL only outside a $0.01 rounding tolerance; if FAIL, state the discrepancy amount.
- As with knowledge-category.md, distinguish rule-matched items from items routed to a sub-category only because no rule clearly applied. List the latter as "Uncertain sub-category assignments", with reference and a one-line reason. State "None" if there are none.


### FORBIDDEN PRACTICES [CORE]

- Do not leave any Electrical Components line item unassigned to a sub-category.
- Do not assign a line item to more than one sub-category.
- Do not invent a 16th sub-category.
- Do not apply the Commodity/Specialty split to any family other than Resistors, Capacitors, and Inductors.
- Do not skip the Sub-Count Check / Sub-Cost Check lines, even when both pass.


### DECISION CRITERIA FOR AMBIGUOUS CASES [OPTIONAL]

__Resistors (Commodity):__ standard fixed or SMD resistors.

__Resistors (Specialty):__ precision (≤0.1% tolerance), wirewound, high-power, or current-sense resistors.

__Capacitors (Commodity):__ ceramic capacitors.

__Capacitors (Specialty):__ electrolytic, tantalum, or film capacitors.

__Inductors (Commodity):__ chokes and ferrite beads.

__Inductors (Specialty):__ transformers and custom-wound inductors.

__Diodes:__ standard, Zener, Schottky diodes (signal/power, not LEDs).

__Transistors:__ BJT, MOSFET, FET.

__ICs:__ MCU, analog, logic, memory, regulators.

__Connectors:__ board-mount headers, sockets, board-to-board/board-to-wire connectors.

__Crystals & Oscillators:__ crystals, resonators, oscillator modules.

__LEDs:__ indicator and display LEDs (separated from signal/power diodes).

__Switches & Relays:__ electronic or electromechanical switching devices.

__Fuses & Protection:__ fuses, TVS diodes, ESD protection devices.

__Other Electrical:__ anything electronic not covered above (e.g. transformer-adjacent magnetics not classed as Inductors, antennas, batteries).

__Tie-breaks:__
- Resistor/Capacitor/Inductor type not stated on the BOM line → default to Commodity and list under "Uncertain sub-category assignments" with reason "type not specified."
- A component matching two sub-categories by description (e.g. a relay described as a "switch") → use the more specific term in the description (Relay → Switches & Relays).
- If a part clearly belongs to Electrical Components but matches none of the other 14 sub-categories → Other Electrical, not a new category.
- If still unclear after applying the above → Other Electrical, and list under Uncertain sub-category assignments.


### EXAMPLE 1 [CORE]

For this sample, the top-level category table reported Electrical Components: Count 8, Sum of Cost $6.95. Breaking that down:

Input (the 8 Electrical Components line items):

| Description | Mfg Name | Mfg Part Number | Qty | Unit Price | Ext. Cost |
|---|---|---|---|---|---|
| 10k Ohm resistor, 0805 SMD, ±1% | Yageo | RC0805FR-0710KL | 1 | $0.01 | $0.01 |
| Precision wirewound resistor, 0.1% | Vishay | Y0024100R000T9R | 1 | $0.85 | $0.85 |
| 100nF ceramic capacitor, 0805, X7R | Murata | GRM21BR71H104KA01L | 2 | $0.02 | $0.04 |
| 10uF tantalum capacitor, 16V | KEMET | T491A106K016AT | 1 | $0.30 | $0.30 |
| Ferrite bead choke, 600Ω @ 100MHz | Murata | BLM18PG600SN1D | 1 | $0.05 | $0.05 |
| Custom-wound transformer, 1:1 isolation | Triad Magnetics | FP6-1000 | 1 | $1.75 | $1.75 |
| 32-bit MCU, ARM Cortex-M0 | STMicroelectronics | STM32F030F4P6 | 1 | $3.50 | $3.50 |
| 4-pin board-mount header connector | Molex | 22-27-2041 | 1 | $0.45 | $0.45 |

Output:

| Sub-Category | Count | Sum of Cost |
|---|---|---|
| Resistors (Commodity) | 1 | $0.01 |
| Resistors (Specialty) | 1 | $0.85 |
| Capacitors (Commodity) | 1 | $0.04 |
| Capacitors (Specialty) | 1 | $0.30 |
| Inductors (Commodity) | 1 | $0.05 |
| Inductors (Specialty) | 1 | $1.75 |
| Diodes | 0 | $0.00 |
| Transistors | 0 | $0.00 |
| ICs | 1 | $3.50 |
| Connectors | 1 | $0.45 |
| Crystals & Oscillators | 0 | $0.00 |
| LEDs | 0 | $0.00 |
| Switches & Relays | 0 | $0.00 |
| Fuses & Protection | 0 | $0.00 |
| Other Electrical | 0 | $0.00 |
| **Total** | **8** | **$6.95** |

Sub-Count Check: 8 = 8 (Electrical Components Count from top-level table) → PASS
Sub-Cost Check: $6.95 = $6.95 (Electrical Components Sum of Cost from top-level table) → PASS

Uncertain sub-category assignments: None.


### CHECKLIST [CORE]

Before finalizing, confirm:

- Every Electrical Components line item has exactly one sub-category.
- Table has exactly 16 rows (15 sub-categories + Total) and 3 columns, with all 15 sub-categories shown even at 0.
- Sub-Count Check and Sub-Cost Check are both present, correctly computed, and reconcile against the top-level Electrical Components row (not the whole CBOM).
- Any FAIL states the exact discrepancy amount.
- Commodity/Specialty split is applied only to Resistors, Capacitors, and Inductors.
- Uncertain sub-category assignments are listed separately (or "None" is stated).