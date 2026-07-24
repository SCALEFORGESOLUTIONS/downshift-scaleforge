# SKETCHIE — Working Context

> Everything needed to do good work on Sketchie without re-deriving it.
> **Source of truth: the live site, sketchie.ai** (crawled 24 Jul 2026). The project one-pager is now *secondary* — it is out of date in several places, listed in §7.
> Update §6 and §7 as things get answered.

---

## 1. What Sketchie is

Sketchie turns a prompt or a document into a narrated whiteboard explainer video that draws itself, in about a minute. Every video ships as **editable scenes**: you open a scene, change it in plain language, and only that scene re-renders. Built by a small team in **Lima, Peru**, with **no venture funding**. Launched **July 2026**.

**Positioning line:** Type it. Watch it draw. Fix one scene, not the whole video.
**Their own hero:** "Amazing explainer videos, under a minute."

Site pages: `/` `/examples/` `/pricing/` `/api/` `/docs/` `/education/` `/science/` `/vs-simi/` `/icons/` `/app/login` `/app/signup` `/app/` `/terms/` `/privacy/`
Docs sub-pages: `/docs/api/` `/docs/voices/` `/docs/languages/` `/docs/cli/` `/docs/mcp/`
GitHub: `Downshift/explainer-evals` (the open eval standard). Downshift is the org behind Sketchie.

---

## 2. Product facts (confirmed on the live site)

**Inputs.** Prompt, PDF, Word, PowerPoint, TXT, Markdown, MP3, MP4, article link. "Document mode" follows your source instead of inventing a generic script.

**Output.** MP4 in 16:9, 9:16 or 1:1. Export-ready for YouTube, TikTok, Instagram, LinkedIn, X. Plus the editable scene graph.

**Speed.** "About a minute" / "under a minute" for a full video — script, drawings, narration, word-level timing. A single scene edit re-renders in seconds. (Note: the docs quickstart says rendering "takes a few minutes" — see §7.)

**Scene editor.** Scene list; click a scene; edit heading, narration, labels, icon styling; Previous/Next navigation; describe a bigger change in plain language when the fields are not enough; save and re-render that scene.

**Length.** 6:00 hard ceiling, 30-second steps, or auto. Longer material becomes a chaptered series. 1:00–3:00 is the stated sweet spot per concept.

**Voices.** Six curated narrators: **Nora** (warm default), Miles, Dean, Clara, Piper, Wren. Playable previews. `GET /v1/voices`. Piper is the bright/playful one for younger learners; there is a "kid-explains-to-kids" voice framing on the education page. Cloned voices run a more expensive rail: **2× in app, 3× via API** (the two surcharges stack).

**Languages.** Contradictory across the site — see §7. Native-language renders shown for Spanish, Brazilian Portuguese and German.

**Layouts.** Sketchie picks the arrangement that fits the idea: single board, split comparison, radial cycle.

**API / MCP.**
- Base URL `https://sketchie.ai/api`, auth `Authorization: Bearer sk_...`, key created in Settings, shown once.
- `POST /v1/explainer` with `input`, optional `length` ("M:SS", 30s steps, up to "6:00"), `aspect`, `voice`, `language`. Legacy aliases `prompt` / `lengthSeconds` still accepted; friendly field wins.
- Returns `202 Accepted` with a queued record. Lifecycle: `queued → generating → rendering → ready | failed`.
- `GET /v1/explainer/:id` to poll; `GET /v1/explainer/events` for SSE. Ready record carries `videoUrl` and `sceneGraph`.
- `POST /v1/explainer/{id}/edit` with `{ "instruction": "..." }` — plain-language edit, one scene changes, versions kept in history, revertable.
- MCP server for Claude Desktop, Claude Code, the Claude app, Cursor, any MCP client. Also a `sketchie` CLI and `@sketchie/sdk` TypeScript client.
- Deterministic rendering: the same scene graph renders the same video every time.

**Examples page.** 19 real videos, 0:58 to 5:56, embedded from their own pipeline. Gallery MP4s live at `https://private-intel.pages.dev/shared/sketchie/gallery/<slug>.mp4` (compound-interest, supply-demand, supply-demand-split, water-cycle, water-cycle-loop, vaccines, photosynthesis, plate-tectonics, inflation, black-holes, neural-networks, doc-meeting-notes, doc-compound, how-delete-file, dns, encryption, lang-es-sky, lang-pt-sky, lang-de-sky). Site-hosted: `https://sketchie.ai/videos/recursion-ours.mp4`, `recursion-ours-30s.mp4`, `quantum-ours.mp4`, `customer-ours.mp4`, `pitch-ours.mp4`, `reveal-before-after.mp4`, `fill-c-vs-d.mp4`.

---

## 3. Pricing (from /pricing/, the authoritative page)

**Free:** 2 videos, up to 1 minute each, watermarked, no card, full product including the scene editor. Editing does not consume a free video; your first edit of a video is free.

| Plan | $/mo | Minutes | Adds | Overage (opt-in) |
|---|---|---|---|---|
| Solo | 9.99 | 40 | Full scene editor, all voices, all aspects | $0.50/min |
| Plus | 19.99 | 100 | API + MCP access | $0.40/min |
| **Pro** (most popular) | 49.99 | 300 | Custom branding | $0.33/min |
| Max | 99.99 | 750 | — | $0.27/min |
| Team | 499.99 | 5,000 | Priority support | $0.20/min |
| Enterprise | Contact | Custom | Dedicated support | — |

- **Annual billing: 40% off every plan.**
- **One rule: anything beyond your plan is 2×.** Overage is 2× the plan's own per-minute rate, **opt-in and off by default** — with it off you stop at the ceiling with an upgrade prompt.
- **API and MCP generation debits 2× minutes**; in-app with a stock voice is 1×. (Contradicted elsewhere — §7.)
- Minutes **do not roll over**.
- **Seats and shared workspaces are unlimited and never metered.** Shared workspaces are "on the way"; today Sketchie is single-user.
- Enterprise-only asks — SSO, SCORM, LMS, SLA — are "let us talk", i.e. not shipped features.

---

## 4. The science (the credibility moat) — /science/

Eleven evidence-graded design laws, a numeric spec, and a three-layer eval harness. Grades: **Verified / Derived / Heuristic / Product / Pipeline**. All eleven laws are currently graded Verified with citations.

Key laws: L1 progressive reveal is mandatory (the core mechanism); L2 the visible hand is optional, the drawing is not; L3 signaling lowers load, which is what buys the gain; L4 segment and leave ≥0.7s processing gaps; L5 the six-minute cliff (6.9M edX sessions: ~1.0 engagement under 6 min, ~0.55 at 9–12, ~0.2 past 12); L6 Khan-style drawing beats slides (0.72 vs 0.52 normalized engagement at 3–6 min); L7 narrative and progressive reveal must be paired; L8 no seductive details; L9 end with a retrieval question, 1.5–2s pause, one-line recap; L10 event boundaries reset attention; L11 do not optimize for view-count aesthetics.

Numeric spec highlights: scenes 12–30s, one idea each; 3–7 elements per scene; a new reveal every 2–5s, never closer than 1.2s; reveal-to-word sync within ±500ms; ≥95% word-sync coverage; narration 140–160 wpm; labels ≤3 words, all-caps keyword; zero unreferenced visuals; hard cuts at idea boundaries only.

Eval harness: **Layer A mechanical** (JS assertions on the scene graph and timings, runs at generation time before spending on voice or render; failures are fed back into the prompt and regenerated), **Layer B semantic** (model-as-judge 1–5, any dimension below 3 fails the release gate), **Layer C output verification** (contact sheet, frame burst, loudness normalization).

---

## 5. Competitive position — /vs-simi/

Sketchie publishes a named, sourced comparison against **Simi (Lamina Labs)**, dated July 2026, written in their own voice with a "read it with that in mind" caveat and an honesty section.

Their framing: Simi generates in under 20 seconds and its launch copy calls the output "a finished video, not a draft." Sketchie's premise is that no first generation is finished. Simi: up to 1 hour one-shot, preset lengths in 1-minute steps, male/female voices, 80+ languages, custom branding at the $499.99 tier, API at 3× app rate gated to top tiers, overage not published, no scene editor visible, no retrieval ending.

**Where Simi is ahead, stated openly:** raw first-generation speed, language count, track record (thousands of users, press; Sketchie launched this week), and funding ($3.5M vs $0).

Their argued metric: *time to a video you will actually publish*, including every revision — not first-render speed.

---

## 6. Positioning decisions (July 2026 work)

- **Central idea:** the recurring gap between "it works" and "someone else gets why it matters."
- **Strongest differentiator is per-scene editability**, not speed. Speed is where they *lose* to Simi; editability is architectural.
- **Second differentiator: the public, graded science spec.** Nobody else in the category publishes one. It closes education and L&D.
- **Third: honesty as a brand asset.** Publishing a comparison that names where a competitor wins is the tone. Do not write copy that undercuts it.
- **The refusal is an asset.** The 6-minute cap, no talking heads, no stock footage — a strength, not a limitation to bury.
- **Primary CTA everywhere: "Get started free" → `https://sketchie.ai/app/signup`.**
- Hero speaks to builders/technical creators; education and L&D get their own section and `/education/`.
- Recommended deck type is still a **product pitch deck**, not investor: no traction, revenue or team data is published, and they say plainly that they launched in July 2026 with $0 raised.

---

## 7. ⚠ Contradiction ledger — resolve before publishing anything

These are conflicts **within the live site itself**. Flag them to the team; do not silently pick one.

| Fact | Conflict | What I used, and why |
|---|---|---|
| **API metering** | Homepage plan cards say "API + MCP access at **1x**". Docs say "included on **every paid plan**… draws from your plan minutes **at the same rate as the app**". Pricing FAQ and the API page say **2×**, included **from Plus up**. The pricing table gives Solo no API line. | **2×, from Plus up** (pricing + API page + one-pager agree; docs and homepage are the outliers). Highest-priority fix — it is a billing claim. |
| **Voice count** | Homepage FAQ: "**30** curated voices." API page, docs, education, vs-simi: "**six** curated narrators" (Nora, Miles, Dean, Clara, Piper, Wren). | **Six named narrators.** The "30" appears once and is unsupported everywhere else. |
| **Language count** | Homepage: "**78** languages." vs-simi table: "78 supported languages." vs-simi honesty section: "we cover **23** on our own stack with long-tail coverage rolling out." Docs: "the **34** supported narration languages." | **No number used.** Wrote around it and pointed to `/docs/languages/`. Three published figures is a credibility risk on a page whose whole pitch is honesty. |
| **Plan count** | Homepage: "**Four plans**… Business adds custom branding." Pricing page: **six** plans (Solo, Plus, Pro, Max, Team) plus Enterprise, and **Pro** adds custom branding. There is no Business plan. | **The pricing page.** The homepage block is stale. |
| **Render time** | Site: "under a minute" / "about a minute." Docs quickstart: "Rendering runs in the background and **takes a few minutes**." One-pager: "36.7s for a 30s video, measured on production." | **"About a minute."** The 36.7s figure is not published anywhere public — do not use it externally without a dated benchmark. |
| **Free tier unit** | Free is metered in **videos** (2) while every plan is metered in **minutes**. | Presented as-is; it reads fine as a trial, but confirm it is intentional. |
| **Downshift ↔ Sketchie** | The one-pager brands it "Downshift · Product Studio"; the live site never mentions Downshift except as the GitHub org. | Kept Downshift out of public copy. Confirm the intended public framing. |

**Internal-only, never on a public page:** COGS (~$0.01–0.02/min) and the ~80% in-app margin from the one-pager. Neither appears anywhere on the live site — treat as confidential.

---

## 8. Voice

Smart, direct, dry, unusually honest. Short sentences, concrete nouns, real numbers. The personality is in specificity — "the doc nobody reads," "take five," "a cough at 0:40," "your plan starts when you've seen it work" — not in exclamation marks. Their own headings are the register to match: *"The honest FAQ." "Simple, honest pricing." "Not a manifesto, a gate." "Fix it like a lesson plan, not a film shoot."*

**Never use:** revolutionary, game-changing, cutting-edge, seamless, transform your workflow, unlock the power of, future of, all-in-one, next-generation.
**Prefer:** "Fix one scene instead of remaking the whole video." Never: "Experience unprecedented control over your AI-powered creative workflow."

**Brand tokens.** Violet `#6C5CE7` primary, hover `#5A4BD4`, tint `#F3F0FF`. Ink `#1E1E28`, warm paper `#FBF8F3`, white `#FFFFFF`, warm surface `#F4EFE7`. Accents: sky `#339AF0`, leaf `#40C057`, tangerine `#FF922B`, coral `#FF6B5B`, sunny `#FFC933`, pink `#E8538F`. For small text, use darker accent shades (`#1971C2`, `#2B8A3E`, `#D9480F`) — the bright ones fail contrast.
Type: **Shantell Sans** headings (marker), **Inter** body, **IBM Plex Mono** labels and metrics. 12px radius, dotted-grid texture, warm whitespace.
Mascot: `https://sketchie.ai/mascot/sketchie-a-inkling.svg` (inkling) and `sketchie-c-sketcher.svg` (holding a marker).

---

## 9. Rules for future work

1. **The live site wins.** Then the pricing/API/science pages over the homepage — the homepage carries the stale plan and metering copy.
2. Never invent traction, customers, testimonials or partnerships. They have published none, and they say plainly that they just launched.
3. Never present SSO, SCORM, LMS, SLA or shared workspaces as shipped.
4. Never put COGS or margin in customer-facing material.
5. Flag contradictions from §7 rather than smoothing them over. Smoothing them is the one thing that would damage this brand.
6. Use the real CTA (`/app/signup`) and real page links, not placeholders.
7. Match the honesty register: if a competitor is better at something, say so.

---

## 10. Prior deliverables

- `sketchie-positioning-landing-and-deck.md` (24 Jul 2026) — strategy, landing copy, three alternate heroes, 12-slide product pitch deck, claim table. **Written before the site crawl: its pricing table (7 tiers with a $19.99 "first API tier") and its 36.7s / 78-language / 30-voice claims need correcting against §3 and §7.**
- `index.html` (24 Jul 2026, rebuilt after the crawl) — production landing page, accurate to the live site, with real video embeds, real pricing and the interactive scene-edit demo.
