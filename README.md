# Cognitive Council

> A thinking tool for working through a specific dilemma yourself.

[cognitivecouncil.com](https://cognitivecouncil.com) — a single-page experience that takes a decision or dilemma you're stuck on, runs a Socratic intake to gather context, then points you toward the cognitive tendencies that *might* be at play and the established exercises you can work through yourself.

It isn't a chatbot, and it won't tell you what to do. The thinking stays yours: the tool is a map and a librarian, not an oracle or a counsellor. The "Council" is reframed as the *council of your own mind* — the systems that quietly shape how you decide.

> **Sister site:** [Cognitive Counsel](https://cognitivecounsel.com) works the same way but on the level *up* — the recurring patterns *across* situations, rather than a single dilemma in front of you.

## How it flows

1. **Welcome** — what the tool is (and isn't).
2. **Query** — describe the dilemma on your mind.
3. **Socratic intake** — the tool privately reads your dilemma and generates 4 tailored questions (emotional layer, recurring patterns, a reframe, concrete specifics). *You* do the thinking here; the answers stay in your browser.
4. **Guide** — two things, neither of them a verdict about you:
   - **What might be at play** *(conditional)* — when a well-established mechanism is genuinely relevant, it's surfaced as a tappable primer. Framed as a hypothesis ("might"), never a diagnosis. Skipped entirely when nothing clearly fits.
   - **Exercises you can work through** — 2–3 established techniques from named evidence-based traditions (CBT, ACT, behavioural science, Stoic practice), each with a "why this might fit" drawn from your own words.
5. **Exercise** — step through a chosen exercise interactively, filling it in yourself. At the end you see your own words back, with a closing question (never an interpretation), and can copy or download your work.

### Design principles ("the line we hold")

- **The engine reasons, but never narrates a verdict about the user.** It decides which primers and exercises to surface; it does not pronounce on who you are.
- **Hypotheses, not diagnoses.** Mechanisms are always "what *might* be at play."
- **Gated education.** A mechanism is only named when it's both widely acknowledged as relevant *and* genuinely helpful — otherwise the step is skipped, not filled.
- **Whitelist of mechanisms.** The model may only name mechanisms in the vetted, static primer set (`primers` in `index.html`). This keeps replication-failed pop-neuroscience (ego depletion, amygdala hijack, left/right-brain) out by construction.
- **Established, not outdated.** Exercises are recognised techniques in current professional use; a `BLOCKLIST` excludes discredited ones (e.g. critical incident stress debriefing, catharsis-as-release).
- **Open loop.** The exercise leaves with the user. The AI does not grade or interpret their answers.

## The primer set (the whitelist)

Stored statically and inline in `index.html` as `primers`. Each is mechanism-level, hedged, attributed to a tradition (not a specific study), and framed around evolutionary mismatch — systems built for an environment we no longer live in. Current set:

Loss aversion · Present bias · Sunk-cost effect · Status-quo bias · Negativity bias · Affective-forecasting error · Social threat & belonging

The primer copy is editable in one place without touching any logic. The exercises stay dynamic; the facts stay fixed.

## Tech

Single-file static site — no build step required.

- React 18 (UMD, in-browser)
- Babel standalone (in-browser JSX transform)
- Tailwind CSS (CDN)
- jsPDF (PDF export of the user's own exercise work)
- marked + DOMPurify (sanitised markdown rendering for primers)

Backend dependency (separate service):
- `api.cognitivecouncil.com/api/chat` — Anthropic Claude API proxy. The client sends the full prompt in `message` with a `systemRole` label (`intake_analyst`, `question_generator`, `guide_select`, `exercise_build`); the proxy forwards it. No backend change is required to add new modes.

## Running locally

It's just an HTML file. Any static server works:

```bash
python -m http.server 8000
# or
npx serve .
```

Then open `http://localhost:8000`. API calls hit the production backend at `api.cognitivecouncil.com`, so you need network access for intake and exercises to generate.

## Phasing

- **Phase 1 (shipped):** the core inversion — Socratic intake → gated static primers + 2–3 established exercises → interactive step-through → open-loop review with export. Thin-input handling included. Copy and generated text follow a house style tuned to avoid AI-writing tells (see `HOUSE_STYLE` in `index.html`).
- **Phase 2 (planned):** "none of these — describe what would help" self-authoring path; on-request bespoke exercise generation. (Printable worksheets dropped — online-only.)
- **Phase 3 (later):** the Counsel relationship reworked as sister-site exploration (not a handoff).

## Privacy

Your answers and exercise responses stay in your browser (localStorage). Intake questions and exercise scaffolds are generated through Anthropic's Claude API; nothing you write is stored by this application. Anthropic retains API conversations for 30 days for safety monitoring — see their [privacy policy](https://www.anthropic.com/legal/privacy).

Google Analytics is loaded only after user consent via the cookie banner.

## Credits

Created by Nicole Scheid & Claude (Anthropic).

## License

[MIT](LICENSE)
