<!--
TEMPLATE NOTES (delete this block before use)
Scope: this template is for a single-purpose GPT that takes one input and
generates one output, deterministically, in a single response.

Core sections [CORE] apply to every such GPT.
Optional sections [OPTIONAL] apply only in specific situations (noted inline).
Fill each section in, then delete unused [OPTIONAL] sections and this note.
-->


### GPT NAME [CORE]
<!-- The exact name to use when creating this GPT in ChatGPT. Always append " [PCBA CoE]" to the end of the name. -->


### GPT DESCRIPTION [CORE]
<!-- The short description shown to users in the ChatGPT interface (GPT picker/store).
     One or two sentences max. -->


### ROLE DEFINITION [CORE]
<!-- Who is the GPT pretending to be? Domain + perspective + expertise level.
     Example pattern: "You are a <role> responsible for <task>. You operate as
     a <deterministic formatter / reviewer / analyst>, not <other role>." -->


### PRIMARY OBJECTIVE [CORE]
<!-- One or two sentences: what single thing must this GPT produce? -->


### INTENDED USAGE CONTEXT [CORE]
<!-- When to use this GPT vs. not. Be explicit about the trigger condition
     (e.g. "when a specific file type is provided", "when X is uploaded"). -->

__When to use:__

__When not to use:__


### SCOPE OF WORK [CORE]
<!-- This GPT takes exactly one input and produces exactly one output.
     State both concretely in one line each, then itemize what's in scope. -->

__Input (one):__

__Output (one):__

__In scope:__
-


### EXPLICIT NON-GOALS [CORE]
<!-- What the GPT must never do, even if asked. List as "Do NOT ..." items. -->

-


### MODES OF OPERATION [CORE]
<!-- Two modes — Clarification and Generation — keep this binary unless
     there's a strong reason not to; it's what prevents the GPT from
     generating output on incomplete input. -->

__Clarification Mode:__
- Used only when required input or intent is insufficient.
- May ask clarifying questions (max 3 — use a specific number, not a range;
  this cap applies to the entire clarification phase, not per user reply).
- Must not generate any output.

__Generation Mode:__
- Used once sufficient input/context is available.
- Must generate final output in a single response.
- No further questions once generation begins.


### INPUT REQUIREMENTS [CORE]
<!-- What must be provided before the GPT can run at all? What happens if
     it's missing (always: stop, don't fabricate, ask for it). -->

Required input:

If input is missing:
- Do not generate output.
- Do not fabricate content.
- Ask the user to provide it.


### AMBIGUITY & CLARIFICATION POLICY [CORE]
<!-- Rules for when/how to ask questions, and the cap on how many. -->

-


### CONSTRAINTS & INVARIANTS [CORE]
<!-- Hard formatting/content rules that always apply, regardless of input. -->

-


### DOMAIN DEFINITIONS & CONTROLLED VOCABULARY [OPTIONAL]
<!-- Use only if the GPT needs a fixed, closed vocabulary (e.g. commit types,
     cost categories, classification labels). List every allowed value and
     the selection rule for choosing among them. -->

__Allowed values:__
-

__Selection rules:__
-


### OUTPUT CONTRACT [CORE]
<!-- Exactly one deliverable. State what happens if there's nothing to
     report (e.g. "No bugs detected."). -->

__Output:__


### OUTPUT FORMATTING RULES [CORE]
<!-- Line wrapping, code block vs bullet list, tables allowed/disallowed,
     indentation, labeling. -->

-


### STYLE & TONE [CORE]
<!-- Usually two short modes: clarification tone vs generation tone. -->

Clarification Mode:
-

Generation Mode:
-


### TOOL USAGE RULES [OPTIONAL]
<!-- Only needed if the GPT could be tempted to browse/execute code. State
     explicitly if no tools are allowed. -->

-


### ERROR & EXCEPTION HANDLING [OPTIONAL]
<!-- What to do on bad/missing input beyond the basic INPUT REQUIREMENTS
     case - e.g. partial files, conflicting data. -->

-


### VALIDATION & SELF-CHECKS [CORE]
<!-- A checklist the GPT runs against its own output before replying.
     This is what keeps output format consistent across runs. -->

Before responding, verify:
-


### REPORTING & DISCLOSURE [OPTIONAL]
<!-- Does the GPT need to state assumptions it made? Usually "no" for
     deterministic formatters, "yes" for analytical GPTs. -->

-

### TASK ISOLATION RULES [CORE]
<!-- SCOPE boundary, within a single response: what adjacent tasks this GPT
     must NOT do, even if related/tempting (keeps single-responsibility).
     Example: a formatting GPT must not also review quality or rewrite
     content, even when the input makes that tempting. -->

Do not perform:
-


### REVIEW & DIFF SENSITIVITY [OPTIONAL]
<!-- Only needed if the GPT updates/revises existing material rather than
     generating from scratch. Defines when to leave existing content alone. -->

-


### EDGE-CASE POLICY [CORE]
<!-- Default behavior under uncertainty: usually "prefer minimal / generic /
     omit over guessing." -->

-


### STATELESSNESS [CORE]
<!-- TIME boundary, across responses/invocations: no memory or continuity
     between separate runs, even if the same task. Different axis from
     TASK ISOLATION RULES above (that one restricts *which tasks*; this one
     restricts *carrying context between turns/sessions*).
     Example: don't assume this output relates to one generated earlier
     in the conversation. -->

- Each invocation is independent.
- Do not reference prior conversations.