---
name: trim
description: >-
  Trim anything dense — a UI surface, a module/API/config, a doc or piece of
  writing, a feature set, a process — down to what actually earns its place
  against the one job it does. Grounds every element in the goal, ranks them
  least-useful-first, lets the human pick the cut line, then implements the cut
  cleanly (relocate any real signal, remove orphans, verify). Invoke when the
  user says "trim this", "this is too busy / too much / bloated / wordy", "rate
  these columns/filters/options/steps", "what can we remove", "is this
  over-engineered", "tighten this text/draft", "what can I cut from this
  paragraph", or shows a cluttered surface or wordy draft and asks what's worth
  keeping.
argument-hint: [url, file/module, or description of what to trim]
---

# Trim

The default drift of any system is *more* — more columns, more badges, more
filters, more options, more abstraction layers, more steps, more sections. But
space and attention are zero-sum: **every element is paid for in the focus,
maintenance, and complexity it steals from its neighbours.** This skill is the
algorithm for deciding what survives, whatever the surface — a UI, a chunk of
code, an API, a config, a document or piece of writing, a feature set, a
process.

The core test is **value against the surface's one job**: does this element help
reach *the* goal this thing exists for? If it can't, it's cost, not value — no
matter how true, complete, or clever it is.

## The algorithm (the spine — applies to everything)

1. **Name the goal** — the single decision, action, or outcome the surface
   exists to drive, in one sentence. Ground it; don't guess from labels.
2. **Inventory the elements** — every feature/element that *serves* that goal,
   read from the source (code/data), not from the surface impression.
3. **Rank least-useful-first** — score each element by how much it helps reach
   the goal, worst first, so the cut line is obvious.
4. **Suggest the cut line** — recommend a default split ("remove 1–N, keep the
   rest"), then let the human pick the border.
5. **Implement cleanly** — relocate any real signal, remove the orphans, verify.

The rest of this doc is that spine in detail, plus the kill-signals and the
domain-specific guidance (UI / code / text / general).

## When to use / not use

- **Use** for: any surface where "this is too much / too busy / bloated /
  over-engineered" is the complaint, or for a pure *rating* ("which of these are
  worth keeping?") with no edit yet. Works on UI (rows, tables, filter bars,
  toolbars, cards, panels), code (a module's exports, an API's params/endpoints,
  a config's options, a class's methods, a dependency set), and general artifacts
  (a doc's sections, a roadmap's features, a checklist's steps).
- **Don't use** for: net-new work (UI → `frontend-design`; code → design it),
  pure visual polish of an already-lean UI (`critique` / `polish`), or bug fixes.
- **Trim vs. `simplify`/`code-review`:** trim decides *whether a capability
  should exist* — it removes a whole feature/column/endpoint/option against the
  goal, gated on naming the goal and a human picking the cut line. For making
  code you're *keeping* cleaner (dedupe, inline, tighten) use `/simplify`; for
  bug-hunting a diff use `/code-review`. **If you're not removing a user- or
  caller-visible capability, you want `simplify`, not `trim`.**

## 1. Name the goal (ground, don't guess)

Find the **single decision, action, or outcome** the surface drives, in one
sentence: *"this exists to DO X."* Everything is rated against that verb.

- A contacts list isn't "show contacts" — it's *"pick who to reach out to next,
  in what order."*
- A module isn't "handle users" — it's *"turn a request into an authenticated
  session."*
- An onboarding doc isn't "explain the system" — it's *"get a new dev to a green
  build on day one."*

Ground it — don't infer from the names:
- Find where the intent is already written down — the PRD/ticket, the design
  doc, the module's docstring + its callers/tests, or your team's notes. The
  goal is usually recorded somewhere; don't reinvent it.
- Read the code/data/source behind the surface to know what each element
  *really* is and does.

If you can't state the goal in one sentence, stop and ask the user — the whole
ranking hangs on it.

## 2. Inventory from the source, not the impression

The surface impression lies by omission (empty states, conditionals, what's
identical vs. varying, what's actually reachable). Go to the source and, for
**every** element, capture what it really is:

- **UI**: the exact field/data source it reads, what it renders (text, badge,
  icon, sublines), and its empty/conditional states (lots of `-`, "no action",
  hidden-when-null).
- **Code**: each export/param/option/method/branch/dependency — who calls it,
  what it's for, whether it's reachable, how many real call-sites it has.
- **General**: each section/feature/step — what goal it serves, who consumes it,
  whether it's ever acted on.
- **Text / prose**: work *two* granularities. First split into **paragraphs**
  and name each paragraph's job — the one point it makes toward the document's
  goal. Then, for each paragraph, split into **sentences** and name each
  sentence's job. A paragraph or sentence whose job you can't name in a few
  words — or that just repeats its neighbour's job — is a cut candidate.

Fan this out to a search agent if the surface spans several files.

## 3. Score each element — the kill signals

Rate every element against the goal. The demerits that kill an element (each has
a UI form and a code/general form):

- **Non-discriminating / never-varies** — UI: identical on every row, so it
  can't help the user *choose*. Code: a config option nobody ever changes, a
  parameter always passed the same value, an abstraction with one caller, a flag
  that's always true. If it never varies, it carries no signal. *(The single
  most common offender, and the easiest to miss from a static impression.)*
- **Empty-now / speculative** — UI: the data isn't populated yet (no due dates,
  no owners). Code: YAGNI scaffolding for a future that isn't here, a generic
  hook with no second implementation. This is *"earns its place later,"* not
  *"worthless"* — treat differently (see verdicts).
- **Redundant** — UI: shows/filters the same underlying field as another
  element. Code: duplicate logic, two functions doing the same job, a wrapper
  that only forwards. One way to do a thing is enough.
- **Operational / internal** — UI: reflects pipeline/sync/system state, not the
  user's decision. Code: debug scaffolding left in, defensive code for
  impossible states, dead error branches. Belongs on a detail/ops view or
  deleted, not on the primary surface.
- **High cost** — eats disproportionate space/complexity/maintenance: a vertical
  badge stack, a sixth dropdown, a heavy dependency pulled in for one helper, an
  abstraction layer with little payoff. Cost is half the ratio; weigh it
  explicitly.

For **text**, the same demerits read as: *non-discriminating* = a sentence that
restates what the paragraph already said, or a topic sentence carrying no new
information; *empty / speculative* = throat-clearing wind-up ("In today's
fast-paced world…", "It's worth noting that…") that delays the point;
*redundant* = two sentences making the same point, or hedging that just repeats
the claim; *operational / internal* = meta-commentary about the text itself ("As
mentioned above", "In this section we'll…") and filler transitions; *high cost*
= a qualifier / subordinate-clause pile or an adjective-adverb stack that buries
the one point. The sentence that *states the point* is the hero.

The merits that save an element: it's **on the critical path** to the goal, it
**is the entity/core logic itself**, it's the **sort key / public API**, it
**discriminates** (genuinely varies / is genuinely load-bearing), or it's the
**jump-to** (search, entry point). A "hero" is non-negotiable.

## 4. Rank least-useful-first (the deliverable)

Produce a table, **worst first**, so the cut line is obvious:

| # | Element | What it is | Value toward the goal *now* | Verdict |
|---|---------|------------|-----------------------------|---------|

Verdicts (pick one per element):
- **Remove** — structurally low value (non-discriminating, redundant,
  internal/dead). Gone from this surface.
- **Defer / Hide-until-needed** — empty-now or speculative but structurally
  meaningful. Don't delete the concept; collapse the column / drop the
  speculative code until it carries signal, so it reappears once it earns its
  place.
- **Fold / Merge** — redundant with another element; collapse the two into one
  (two status filters → one; two near-identical functions → one), losing no real
  capability.
- **Relocate** — mostly noise but hides *one* useful bit; move that bit inline /
  to the detail page / to the right module and drop the rest.
- **Tighten** *(text)* — the point is real but buried; keep the sentence's job,
  cut the words around it (rewrite to fewer words) rather than dropping it whole.
- **Keep** / **Keep (hero)**.

For **text**, run the ranking at two levels: rank the **paragraphs** first and
cut whole paragraphs that don't earn their place, then within each surviving
paragraph rank its **sentences**. The `Element` column holds the paragraph or
sentence — quote its first few words so the cut line is unambiguous.

Always call out the **structural finding** above the table — e.g. "three of six
dropdowns filter the same field," "rows are 110px tall because of one badge
stack," "this module exports 12 symbols, 8 of which have a single internal
caller," or "every paragraph opens with a sentence of throat-clearing before the
point." That's the insight; the table is the evidence.

## 5. Let the human pick the cut line

Recommend a default cut ("remove 1–N, keep the rest"), then **stop and let the
user choose the border.** They'll often answer `"remove 1–N inclusive"`. Do not
unilaterally delete — taste is theirs, and a ranking they can veto is more
useful than a fait accompli.

## 6. Implement the cut cleanly

When the line is set:
- **Don't lose real signal.** When a Relocate/Fold drops something that had one
  useful bit (a discriminating badge, a warm-path hop, a load-bearing branch),
  move that bit to where it belongs — don't let it vanish with the wrapper.
- **Preserve full capability elsewhere.** Affordances/behaviour you remove from
  the surface must already live where they're still reachable (verify they do).
- **Remove the orphans.** A removal leaves a trail: unused components, now-dead
  imports, leftover `className`/conditional branches, stale `useState`,
  dependency-array entries, orphaned helpers, dangling tests, dead config keys.
  Sweep them all — an unused import fails lint/typecheck.
- **One surface per commit.** Trim the row, then the filter bar; or the module,
  then its callers — as separate commits with a *why* in the message ("identical
  on every row / single caller / duplicates field Y"), not just a *what*.
- **For text:** after cutting, re-read the seams — fix transitions and pronoun
  references so the shortened version still flows. A clean cut shouldn't leave a
  jump or a dangling "this".

## 7. Verify

Run the project's `typecheck`, `lint`, and `test`, plus a `grep` for the removed
symbols to catch dangling refs. **Distinguish pre-existing failures from yours**
(e.g. stale generated types, unrelated warnings) and say so explicitly rather
than blaming or ignoring them. For **text**, there's no typecheck — the check is
reading it aloud: does the goal still land, and is every surviving sentence
pulling its weight?

## 8. Ship

Commit, and deploy if the project has a deploy step. Report the before→after
shape ("6 dropdowns → 3", "11 columns → 4, ~3× rows per screen", "12 exports →
4, 2 deps dropped").

## Anti-patterns

- **Rating from the impression alone.** You'll miss that an element never varies,
  is empty-only-for-now, or is actually unreachable — exactly the signals that
  decide its fate. Always read the source.
- **Deleting "empty-now / speculative" permanently.** Prefer defer /
  collapse-until-needed; the concept earns its place once it carries signal.
- **Losing the one good bit when collapsing.** Relocate it.
- **Trimming without naming the goal.** Without the verb, "useful" is unanchored
  and you'll keep things out of habit.
- **Cutting the line yourself.** Present the ranking; the human picks.
