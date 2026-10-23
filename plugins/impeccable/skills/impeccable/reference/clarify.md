# Clarify

*Additional context needed: audience knowledge and emotional state.*

Rewrite unclear interface text so users understand what happened, what matters, and what to do next. Preserve factual meaning, product terminology, and brand voice.

## Audit the language

Read the entire interaction path, not isolated strings. Identify:

- Ambiguous nouns, verbs, and actions
- Internal jargon or assumed knowledge
- Vague labels, outcomes, and system states
- Missing consequences, recovery, or timing
- Inconsistent terminology and capitalization
- Redundant headings, intros, helper text, and confirmations
- Text that breaks at realistic widths or in translation
- Tone that ignores stress, risk, success, or urgency

Infer audience and task from product context and surrounding UI. Ask before changing factual claims, legal meaning, or a term that may be domain-specific.

## Set the message hierarchy

For each state, decide:

1. The one fact the user needs now.
2. The action available next.
3. Supporting context that changes the decision.
4. The appropriate tone for this moment.

Say each idea once. If the heading already explains the state, the introduction should add new information or disappear.

## Rewrite by function

**Actions and navigation** — Use a specific verb and object when the outcome is not already obvious. Labels describe what will happen, not the gesture used to trigger it. Keep the same noun and verb for the same concept throughout the product. For destructive actions, name the object and consequence. Prefer undo over confirmation when recovery is safe; when confirmation is necessary, name the action on both the message and the button instead of `Yes`, `No`, `OK`, or `Submit`.

**Forms** — Use persistent labels; placeholders are examples, not labels. Put format and eligibility requirements before submission. Explain why information is requested only when it is not obvious. Keep required/optional treatment consistent. Validation says what needs attention and how to correct it, without blaming the user. Keep related instructions near the field and announce errors accessibly.

**Errors and permissions** — An actionable error answers: (1) what failed; (2) why, when known and useful; (3) how to recover or what alternative remains. Do not expose internal codes as the primary message. Do not promise a cause or resolution the system cannot know. Treat privacy, payment, deletion, access loss, and blocked work seriously — warmth is welcome, jokes are not.

**Loading, empty, and success states** — Loading text names the real operation and sets an honest expectation when the wait is meaningful; show determinate progress when available, never invent progress. An empty state distinguishes first use, no results, filters, permissions, and failure — explain the state and provide the next useful action. Success text confirms what happened and what is true now; don't celebrate routine operations.

## Verify

- A first-time user can predict what every control does before touching it.
- Every error names the problem and the recovery.
- The same concept has the same name everywhere on the path.
- Nothing is said twice; nothing load-bearing was cut.

When the copy holds, finish with `/impeccable polish`.
