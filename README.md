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

## Install
Install from Install Extensions in Extensions menu in SillyTavern. 

`https://github.com/nialyn-mid/nialyns-razor-st`

Or, copy/clone this folder into your SillyTavern extensions directory.

Reload SillyTavern. The extension reads the chat through the stable
`SillyTavern.getContext()` API, rescans after every chat event, and writes
the guidance into extension prompt key `nialyns_razor` (in-chat, depth 2,
system role, not scannable by lorebooks).

## Tuning

- Injection spot: KEY / POSITION / DEPTH constants at the top of
  src/adapter/sillytavern.js in the source tree.
- Detector thresholds, suppression lists, debug mode: build/config.mjs.

