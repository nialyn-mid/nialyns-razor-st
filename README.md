# Nialyn's Razor

Deterministic AI-prose steering for roleplay chat. Every AI reply is scanned
for constructions that are statistically more common in AI writing than in
human writing (based on the StoryScope study): somatic-emotion pileups,
smell/scene-setting openings, thematic wrap-ups, epiphany endings,
narration-heavy replies, philosophical monologues, stock phrases, AI-slop
vocabulary, tricolons, not-X-but-Y constructions, and more.

When the weighted score crosses a threshold, a short steering note is added
to the context for the next generation. The note quotes the exact flagged
sentences and says what to do instead. A standing style contract ships every
turn. Detection is pure rule-based text analysis: no LLM judge, no network
calls, no saved state, about 10 ms per turn.

A note is suppressed when the identical issue set persists for three
consecutive turns, so the same correction is never repeated back to back.

Guidance is not single-turn. The newest note sits at the very end of the
context, and the last five replies keep their own signal blocks, each placed
beside the reply it describes (prompt depth slots `nialyns_razor_h1` to
`_h5`). An issue that keeps recurring is explained once and then appears as a
compact `(recurring)` line, so history costs little context and still reads
as a habit rather than a one-off.

## Install
Install from Install Extensions in Extensions menu in SillyTavern. 

`https://github.com/nialyn-mid/nialyns-razor-st`

Or, copy/clone this folder into your SillyTavern extensions directory.

Reload SillyTavern. The extension reads the chat through the stable
`SillyTavern.getContext()` API, rescans after every chat event, and writes
the guidance into extension prompt key `nialyns_razor` (in-chat, depth 2,
system role, not scannable by lorebooks).

## Ignored content

Text that is machine-readable rather than prose is not scored: paired XML-ish
blocks such as `<ooc>...</ooc>` or `<state>...</state>`, fenced code blocks,
balanced `{...}` or `[...]` spans that actually parse as JSON, and stray tag
markup. Quoted evidence therefore never points at a status line.

This is deliberately blunt. If a reply is mostly or entirely syntax, the turn
is skipped silently rather than scored on its syntax — no advice is preferred
over confusing advice. `maxMaskedRatio` (default `1`, i.e. no fallback) is the
lever; a per-tag exception list is the planned refinement if a model turns out
to wrap every reply in a tag.

## Tuning

- Injection spot: KEY / POSITION / DEPTH constants at the top of
  src/adapter/sillytavern.js in the source tree.
- Detector thresholds, suppression lists, excluded-syntax masking and
  `maxMaskedRatio`, debug mode: build/config.mjs.

