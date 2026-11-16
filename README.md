# FlipTrace

A browser-based decision-reversal detector delivered as a single HTML application.

You know how someone says "let's go with React" on Monday, "actually, Vue" on
Wednesday, and by next week they're back on React and nobody remembers agreeing
to any of it? FlipTrace analyses chat logs, email threads and task-board
exports, extracts decisions, and flags patterns that keep getting reversed.
It gives each topic a 0–100 Flip-Flop Score and each person a stability profile.

## Quick start

The repository currently stores the complete HTML document as `code.txt`.
After cloning, copy or rename it to `index.html`, then open that file in a
modern browser.

**Windows PowerShell**
```powershell
git clone https://github.com/ShoryaPratapSinghChandrawat/FlipTrace-Decision-Reversal-Detection-Analytics-Platform.git
cd FlipTrace-Decision-Reversal-Detection-Analytics-Platform
Copy-Item code.txt index.html
Start-Process index.html
```

**macOS / Linux**
```sh
git clone https://github.com/ShoryaPratapSinghChandrawat/FlipTrace-Decision-Reversal-Detection-Analytics-Platform.git
cd FlipTrace-Decision-Reversal-Detection-Analytics-Platform
cp code.txt index.html
# Open index.html in your browser
```

Click **Load demo data** to explore the sample workflow. Or use **Import** to
bring in supported WhatsApp-style text, Slack-style JSON, raw `.eml`/mbox text,
and CSV/JSON task-board data. Press `Ctrl+K` (or `Cmd+K`) for the command palette
and `?` for keyboard shortcuts.

## What it does

**Ingest.** Parsers handle WhatsApp-style text exports, Slack-style JSON, raw
`.eml`/mbox text, and CSV/JSON task-board dumps. Email parsing strips quoted
replies and signatures. Task-history events such as status changes and due-date
moves become decision records.

**Extract.** Each message is split into sentences and scored against editable
phrase lexicons for commitments, reversals, rejections, and hedges. The output
records who, when, polarity, selected option, confidence, and the phrases that
triggered the match.

**Group.** Decisions become TF-IDF vectors. Topic grouping combines cosine
similarity, Jaccard overlap, and option-vocabulary affinity. Users can manually
merge or split topics; both actions are undoable.

**Detect.** Decisions are ordered by time. A reversal fires when a selected
option changes or a commitment is followed by a rejection inside a configurable
window (45 days by default). An A → B → A "boomerang" has its own type.
Ordinary task progress (backlog → in progress → done) is filtered out.

**Score.** The Flip-Flop Score combines flip count (30%), boomerangs (25%), flip
speed (15%), oscillation (15%), decision entropy (10%), and confidence drift
(5%). Score bands range from Stable to Full Pendulum.

**Alert.** Configurable thresholds trigger alerts with the source sentence,
speaker, timestamp, and matching rule.

## Implementation

The application includes:

- An IndexedDB layer with seven stores, indexes, a versioned migration hook,
  and a localStorage fallback for browsers that restrict storage on `file://`.
- A small REST-like client-side API; the UI does not access the database layer
  directly.
- A Web Worker for analysis with a main-thread fallback.
- Separate services for ingestion, topic grouping, reversal detection, scoring,
  alerts, and export.
- A pub/sub event bus and a diagnostic backend console.
- A dashboard, interactive timeline, topic explorer, pattern map, person
  profiles, import center, alert feed, settings, command palette, onboarding,
  undo/redo, three themes, simulated live feed, and JSON/CSV/print export.

The source modules are listed in the repository's original project notes. The
single-file HTML bundle can be rebuilt from those modules when the source tree
is available.

## Honest limitations

- The phrase lexicons are English-only and tuned on the demo corpus plus
  typical Slack/WhatsApp phrasing.
- "Slack export" means copied channel text and simple JSON arrays, not the
  complete Slack ZIP export format.
- Person names are matched as literal strings; "Sam" and "Sam Rivera" are
  treated as different people.
- Large histories may be slow on older devices.
- The timeline's pendulum animation is decorative.

## Privacy

FlipTrace has no application backend or analytics service. Imported content,
analysis, and saved records stay in browser storage; the app does not send that
data to a project server. The HTML references Google Fonts for typography, so
the browser may contact Google's font endpoints to fetch those fonts. System
font fallbacks are available if those requests are blocked or the device is
offline.

## Licence

Do whatever you like with it. If you catch a teammate's framework boomerang
with it, that's on you.
