# Polish

*Additional context needed: quality bar and shipping constraints.*

Polish is refinement, never concealed redesign. Preserve the incumbent visual world, content, behavior, and everything outside scope. If the concept itself is wrong, say so and recommend a redesign — or `bolder` — instead of smuggling in a replacement.

## 1. Establish the system

Read DESIGN.md (if present) and the representative tokens, shared components, patterns, and neighboring flows. If no formal system exists, use coherent project conventions. Classify each drift before fixing it:

- **Missing token:** the system needs a reusable value.
- **One-off implementation:** an existing shared component or pattern should replace it.
- **Conceptual mismatch:** the flow, information architecture, or hierarchy differs from comparable product areas.
- **Local defect:** the implementation is simply incomplete or inconsistent.

Fix the cause at the narrowest correct level. Ask when a binding system principle cannot be inferred.

## 2. Gather the evidence

Use the feature yourself at the surface's representative sizes (desktop and mobile on the web). Determine whether the path is functionally complete, the intended quality bar and time available, known constraints or deliberately unfinished work, and the states, content lengths, roles, and input methods users will actually encounter.

## 3. Triage

Separate functional defects from cosmetic ones and fix in this order:

1. Broken or blocked tasks, data loss, misleading state, inaccessible paths.
2. Missing loading, empty, error, success, disabled, and permission states.
3. Flow, hierarchy, responsive, and design-system drift.
4. Visual and motion inconsistencies.
5. Code and asset cleanup.

Do not perfect one corner while leaving the rest below the same quality bar.

## 4. Polish the whole path

**Flow and hierarchy** — Match neighboring mental models, terminology, disclosure, routing, and save behavior. Make the primary task and current state obvious without flattening every element to equal weight. Ensure arrival, transition, empty, and recovery paths connect instead of behaving as isolated screens.

**Layout and type** — Align to the project's grid and spacing scale; fix optical as well as mathematical alignment. Group related content tightly, separate distinct groups generously. Keep same-role typography consistent; test measure, wrapping, localization expansion, zoom, and font loading. Verify every supported viewport, not only the current screenshot.

**Color, imagery, and icons** — Use semantic tokens and stable color meanings across themes. Verify text, control, and focus contrast in every state. Keep icon families, stroke/weight, sizing, and optical alignment coherent. Prevent image layout shift: correct aspect ratios, responsive sources, useful alt text.

**Interaction and state** — Every control needs appropriate default, hover, focus, active, disabled, loading, error, and success behavior. Preserve visible keyboard focus, logical tab order, labels, and platform-appropriate touch targets. Keep motion coherent, interruptible, and performant — do not add animation merely to make polish visible. Validate long, missing, localized, offline, slow, and permission-limited content where the product can encounter it.

**Content and code** — Keep terminology, capitalization, punctuation, and factual copy consistent; ask before changing claims. Remove debug output, dead code, unused imports, and obsolete styles. Replace custom implementations with shared components where the system owns the pattern. Promote genuinely reusable values to tokens; do not create a system abstraction for one local exception.

## 5. Verify and finish

Walk the complete path again with mouse, keyboard, and touch where applicable. Check: all supported layouts; loading, empty, error, success, disabled, long-content, and missing-content states; zoom, contrast, focus, semantics, and screen-reader names; console errors, layout shift, interaction latency, and image loading; agreement with DESIGN.md, neighboring features, and the user's scope.

Finish with the source diff: remove accidental churn, orphaned code, redundant values, and temporary artifacts. Ship only when the feature is functionally complete and consistently finished across the path.
