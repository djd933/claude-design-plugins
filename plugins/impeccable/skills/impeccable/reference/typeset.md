# Typeset

Typography carries information, hierarchy, and voice. Improve it inside the established visual world; do not replace the identity unless the user asked to.

## Visitor mode

- **Persuade + Experience:** display type may carry the voice. Use decisive contrast and responsive scale when the composition benefits.
- **Operate + Read:** stability, scanability, and measure come first. A single well-tuned family and a fixed role scale are often right.

If a typography replacement would create a new identity, treat the work as a redesign: preserve confirmed families and improve their use everywhere else.

## Assess

Inspect representative pages and styles, and answer with a file, selector, or computed value:

- **Authority and fit:** Which faces, weights, and roles are established? Do they fit the product and selected world, or are they unexamined defaults (Inter/Arial/system for everything)? Is every family necessary?
- **Hierarchy:** Can heading, body, label, metadata, and data roles be distinguished at a glance? Are adjacent sizes or weights too close to carry different jobs?
- **Scale and consistency:** Is there a deliberate role scale, or a collection of arbitrary values? Do repeated roles stay identical across screens and states?
- **Reading:** Does body copy stay within a comfortable 45-75 character measure? Are line height, paragraph rhythm, contrast, and tracking tuned to the actual face, width, language, and surface?
- **Stress:** What happens with long headings, localization expansion, zoom, narrow containers, missing weights, and font fallback?
- **Delivery:** Are only used assets loaded? Do fallback metrics, loading strategy, and variable-font settings avoid invisible text and disruptive reflow?

Then check the sources mechanically for arbitrary font sizes, weights, and line heights that bypass the scale. Synthesize both passes before editing.

## Set the system

Before editing, state: the roles the interface needs; the intended contrast between those roles; the reading measure and density; which existing faces and weights are authoritative; any performance, localization, or accessibility constraints.

Use the fewest roles and families that make the hierarchy unmistakable. Combine size, weight, space, and tone deliberately instead of asking size alone to do all the work. Role names and tokens describe purpose, not values.

## Apply

- Keep body copy readable and zoomable: 1rem / 16px is the ordinary web body floor unless a dense role or user setting justifies otherwise.
- Keep prose in the 45-75ch range. Tune line height inversely with measure: wider lines need more leading.
- Compensate light text on dark surfaces on all three perceptual axes: slightly more line height, a touch more tracking, one step more weight when the face needs it.
- Tune line height to the face, width, language, and contrast — not a universal ratio.
- Keep repeated roles consistent across screens and states.
- Use numeric, tabular, and code features when their content benefits (tabular numbers for tickers, timers, tables).
- Load only used font assets and weights. Provide metric-compatible fallbacks and avoid blocking text.
- Let marketing display type respond to available space when useful; keep dense product and reading surfaces spatially predictable.
- Preserve browser zoom and user font settings.
- Use paragraph spacing *or* first-line indentation as the paragraph rhythm; combining both usually double-marks the boundary.
- Do not make type decorative at the expense of comprehension, or introduce a second family without a clear role it alone can perform.

## Line breaks

After Matthew Butterick's *Practical Typography*, paraphrased:

- Set `text-wrap: balance` on headings and short centered blocks, and `text-wrap: pretty` on body copy. Both are progressive: a browser that ignores them keeps its default wrap.
- `pretty` pulls a word down beside a lone last word; it does not fill the last line, and a two-word tail is fine.
- In a column too narrow for either to help, edit the copy or widen the column.
- Break a heading by hand only between phrases, and check the break at every width the heading renders at. A hard break that lands mid-line on a phone is worse than the default wrap.
- An uneven break can carry meaning — a headline that lands on one word for weight, a poster line, a pull quote. Make it deliberate and keep it rare.

## Verify

- Primary, secondary, body, and metadata roles are recognizable without reading the copy.
- Long text remains comfortable across relevant widths and languages.
- The typography belongs to the product and its established world.
- Loading creates no disruptive reflow or invisible text.
- Zoom, text scaling, focus, and contrast paths remain usable.

When the hierarchy holds, finish with `/impeccable polish`.
