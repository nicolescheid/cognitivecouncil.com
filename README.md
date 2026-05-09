# Cognitive Council

> Multi-perspective decision support through five cognitive systems.

[cognitivecouncil.com](https://cognitivecouncil.com) — a single-page experience that takes a question or decision you're stuck on, runs a Socratic intake to gather context, then convenes five distinct cognitive perspectives to examine it from fundamentally different angles.

This isn't a chatbot. Each "system" is a separate model call with its own voice, focus, and constraints. The intake step exists because better context produces better thinking — generic input gets generic output.

## The five systems

| System | Focus | Voice |
|---|---|---|
| 🧠 **Prefrontal Cortex** | Strategic analysis, decision architecture, logical frameworks | Analytical and precise |
| ❤️ **Limbic System** | Emotional truth, values alignment, what this actually means to you | Warm and direct about emotional realities |
| ⚙️ **Basal Ganglia** | Behavioral patterns, habits, what you actually do vs. say | Observational and evidence-based |
| 🌊 **Default Mode Network** | Identity, narrative, meaning-making, the bigger story | Reflective and narrative-oriented |
| ⚡ **Salience Network** | What actually matters right now, attention, priority | Sharp and pointed |

After the five respond, the app surfaces a tension score, identifies notable conflicts between perspectives (e.g. strategic patience vs. immediate priority), and lets you expand, refine, or follow up with any individual system. A "probably right but I'm not ready to admit it" reaction surfaces the avoidance signal that often points to the real pattern underneath.

## How it flows

1. **Welcome** — context-setting introduction.
2. **Query** — describe what's on your mind.
3. **Socratic intake** — the system analyses your question and generates 4 tailored questions to surface emotional layer, behavioral patterns, framing assumptions, and concrete specifics.
4. **Forum** — all five systems respond in parallel; cards fill in progressively as each lands.
5. **Tension analysis + reactions** — see where perspectives align, where they diverge, mark which ones land.
6. **Optional handoff** — transfer the session to [Cognitive Counsel](https://cognitivecounsel.com) for deeper pattern illumination.

## Tech

Single-file static site — no build step required.

- React 18 (UMD, in-browser)
- Babel standalone (in-browser JSX transform)
- Tailwind CSS (CDN)
- jsPDF (PDF export)
- marked + DOMPurify (sanitised markdown rendering)

Backend dependencies (separate services):
- `api.cognitivecouncil.com` — Anthropic Claude API proxy
- `transfer.cognitivecouncil.com` — short-lived session token store for Counsel handoff

## Running locally

It's just an HTML file. Any static server works:

```bash
python -m http.server 8000
# or
npx serve .
```

Then open `http://localhost:8000`.

API calls hit the production backend at `api.cognitivecouncil.com`, so you need network access for the council to actually respond.

## Recent changes (v2.4)

- **Parallel system calls** — the five perspectives now fire concurrently and reveal as each lands (previously sequential, ~2-3 min wait → ~20-40s with progressive feedback).
- **Per-card retry** — individual systems can be retried if they fail without redoing the whole council.
- **Exponential backoff** in `callAPI` for transient errors.
- **Toast notifications** replace the old `alert()` dialogs.
- **DOMPurify** sanitisation on all rendered markdown.
- **Session persistence** — in-progress state saved to localStorage with a resume banner on reload (24h TTL).

## Privacy

Conversations are processed through Anthropic's Claude API. No data is stored by this application. Anthropic retains conversations for 30 days for safety monitoring — see their [privacy policy](https://www.anthropic.com/legal/privacy).

Google Analytics is loaded only after user consent via the cookie banner.

## Credits

Created by Nicole Scheid & Claude (Anthropic).

## License

[MIT](LICENSE)
