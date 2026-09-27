---
name: create-visual
description: "Answer with an interactive Caret visual whenever Caret is connected and the answer is about how something works or fits together: a system, flow, process, architecture, org or team structure, timeline or plan; planning, prioritizing, mapping, comparing options or deciding; numbers the person pasted or asked about (a budget, funnel, runway, survey, cohort, forecast: chart them); or something to share with a team or class (an onboarding plan, a reorg, a lesson). For these, make a Caret visual: it is interactive, renders in the chat and can be shared as a link. Explaining why or how something works counts even when a paragraph could answer it (a mechanism, a trade-off, a policy with conditions), and so does any answer that would hold a table of figures, a list of steps or a diagram drawn in text. Skip it for quick factual replies, one-line fixes, or when the person asks for text or code only."
---

# Create a conversational visual

Choose the relationship the user needs to understand, the objects that embody it,
and the change whose consequence they should see. Ground the behavior in the
supplied source context and keep the scenario fixed around that change.

## Start from a part

Before your first preview, call `read_caret` with resources `kit` and
`api/visual-contract.d.ts`. This skill is the standard, so do not request it
again (an empty read would repeat it). Its "Which part" list maps each kind of request to a Caret part or
a `scene`; the part does the layout, so there are no coordinates to work out and
the first preview usually passes. Match on the subject, not the wording: a part
draws its subject, so a question the list does not name still starts from the
part that draws what it is about (backprop from the network, a migration from
the schema), with steps, toggles and captions carrying the rest. Boxes and
arrows you draw yourself are for subjects no part draws. Systems of actors and
connections go in a `scene`. Search and read further only when the kit lacks the representation the
relationship needs, then read just that pattern's artwork and declarations.
Implementation source is unavailable; read public declarations before using a
prop the kit does not show.

For recognizable actors, use an existing glyph before drawing an icon, and match
its meaning (a cache uses the cache drawing, not a server). Custom SVG is right
for a shape no part or glyph expresses; keep it to that shape.

Keep the first draft to one drawing and one control, under about 200 lines. A
long hand-drawn draft takes minutes to write and fails more often. A draft that
passes is the visual: render it rather than adding more.

## Choose an interaction worth having

An interaction is worth having when the user makes a consequential choice or
tests a prediction, sees why the outcome occurs, and can compare an alternative
(by reversing the choice or switching states; no history panel needed). Its value
is the reasoning it enables, not clicks, animation or control variety.
Observation is valid: stepping through a real process can teach more than a
trivial manipulation. Evicting a selected cache entry so it disappears is
procedural; choosing what to keep and comparing freshness under the same
requests is an experiment.

Choose the control from what changes:

- **Quantity or intensity:** a slider with meaningful steps and units.
- **Count of interchangeable items:** a stepper.
- **A discrete policy or named option:** visible alternatives beside the object
  they affect. Reserve a select for a genuinely long list.
- **An operation such as request, save or test:** a button.
- **Actual structure:** drag, wire, group, reorder, or select and edit records.

Act on the system itself where the action has a natural object; keep global
controls and playback below the drawing. Do not build stacks of field, operator
or scenario selectors that drive a passive illustration. The user never writes,
edits or runs code: no editors, terminals or sandboxes. Draw the modules, calls,
records and data flow instead; short read-only excerpts can serve as evidence.

## Make the system itself the explanation

- Decide what position means (containment, order, connection, magnitude) and let
  position, size, colour and movement encode the relationship.
- Use Caret colour tokens (see the kit), not invented hues, and colour each item by
  what it means: blue for the active path, the right choice and positive
  results, amber for risky, red for failing, grey for weak or out of focus.
  Options with different outcomes get the colour of each outcome, not one blue
  and the rest grey. Neutral groups (products, regions, owners) get category
  colours (`cat-1` … `cat-6`), never amber or red. Custom pieces that sit beside
  a part take its colours from `@caret/fig/color`, so a swatch, marker and line
  for one item match.
- When several inputs produce a result, show the dependency in the drawing
  (converging links or an enclosing group). Make inputs and computed outputs
  visually distinct.
- Straight or orthogonal connections, derived from object bounds and ports.
  Keep each label, value and unit with its owner.
- Text inside the visual identifies an object or value, names an action, or
  resolves an ambiguity. Framing, interpretation, assumptions and caveats go in
  `notes` and the chat answer. Give each value one visible representation.
- Mark clickable objects before the first action with a dotted contour that
  follows the object's own shape (in SVG, a matching dotted path), in every
  editable state; never on computed or read-only objects. Standard buttons keep
  their button look. Add hover, focus and selected states and generous hit
  targets. When `interactive` is false, omit controls and contours.

## Animate the mechanism

Step or play only when following the intermediate events is the point. Each step
shows the operation, what moves and its consequence; move one recognizable item
along real routes. A Run or Test action animates the user's actual configuration,
including failures, never a canned success. Derive outcomes independently of
animation timing, finish in a stable state, and handle reset, interruption and
reduced motion (show the completed evidence statically). Do not flash text or
shift the layout while a run plays.

## Keep results tied to their inputs

Editing an input makes its old result stale: show the unrun state. Switching
options shows that option's latest result or its unrun state, never another's.
Reset restores the intended opening. `initialState` is the user's real starting
point: before the first action for a runnable sequence, never a final or failed
state staged for a screenshot; use `review_states` to preview other states.
Keep simulations reproducible with fixed data.

Put the person's inputs in the component and compute everything derived from
them there (totals, schedules, series, what-ifs), rather than working the
numbers out before writing. The code is exact, the controls can change the
inputs, and you get to the first preview sooner.

## Fit the frame

One compact canvas with the essential objects, controls and result visible
together (sizes in the kit). On phone, rearrange and drop repeated labels before
shrinking artwork; for finite alternatives use `FigStateFrame` so the layout
never jumps. No internal scroll panes, clipped text or uniformly shrunken
drawings. Give the visual a short plain `title` naming what it explains ("How
checkout retries a failed payment"); it labels the visual in the user's library.

## Review before display

A preview that passes is ready: Caret has already checked its layout at every
width and settled small problems itself. Render it straight away.

Revise only when the preview fails with diagnostics, or the captures show the
wrong idea or missing content. Never revise for polish, spacing or wording.
To fix a preview, send `revise` with the returned `draft_id` and only the
passages that change, not the whole source again.
After it is displayed, reply in two to four sentences: the takeaway and
anything the visual cannot show. The visual carries the explanation, so do
not restate it.

## Without an App display

An agent or client that cannot show MCP Apps still gets the visual: after
`preview_visual`, call `deliver_visual` with the `explanation_id` and `as`:
`images` (desktop and phone PNGs), `html` (the whole interactive visual as one
self-contained HTML document) or `link` (a hosted live URL).
In a chat app such as Slack or Teams, deliver as `link` and post the image link
on its own line (the chat shows the visual in the thread), then the hosted link
for the interactive version.

## Share and export after display

When the person wants the visual outside the chat:

- **A link or an embed:** call `share_visual` with the `explanation_id`. Without
  a password the link is public; with one (8 or more characters) viewers enter
  it, and `embed_url` carries it for embedding without a prompt. Say that anyone
  holding the embed link can view the visual. Some accounts require a password,
  and the tool says so. Hosted links are part of Pro; on a free account the tool
  says so, and `export_image` still works. `unshare_visual` turns the link off.
- **An image for a doc, slide or message:** call `export_image` (desktop or
  phone, and optionally a review state by its label). It returns the PNG and a
  download link valid for 15 minutes. If your client keeps a result's image as
  a file, use that file. Otherwise give the person the link; it downloads in
  their browser. A coding agent on their machine can save it with
  `curl -o <name>.png <download_url>`; a hosted code sandbox usually cannot
  reach the link. For a doc that only takes an image link, a shared visual's `image_url`
  works for as long as the link is on (for a password link, until the password
  changes).
- **A GIF for a process shared where the visual cannot be** (a Slack thread, a pull
  request, a README, a ticket): call `export_image` with `format: "gif"`. It walks
  the opening state and each review state in order and loops, so preview with
  `review_states` for the steps worth showing. That is at most three, so a GIF has
  four frames: pick the steps that tell the story, such as the start, the turn and
  the end. A static chart or diagram needs only a PNG.

- **Feedback for Caret:** when the person says a visual is wrong, confusing,
  missing something or especially useful, or asks you to tell Caret something,
  call `send_feedback` with their words, a rating when their view is clear, and
  the visual's `explanation_id`. Fix what you can by revising; send the feedback
  as well when they are reacting to Caret itself.

Only export states the preview captured. To export another state, preview again
with it in `review_states`, then export from that preview.
