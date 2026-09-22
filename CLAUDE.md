# Security of AI Agents — AI Engineer 2026

Conference talk repo. Theme: red-teaming AI agents. The deliverable is an HTML slide deck
plus a live red-team run against a deployed orq agent (`clarabelle-cow`).

Forked with full history from the ADC (Amsterdam Data Conference) edition,
[Security-of-AI-Agents-ADC-Consulting](https://github.com/Baukebrenninkmeijer/Security-of-AI-Agents-ADC-Consulting).
Anything still branded ADC is inherited and fair game to rewrite for this talk.

## What we're working on (current)

Reworking the ADC material into the AI Engineer 2026 talk. The active artifact is the
**HTML slide deck in `deck/`**:

- `deck/security-of-ai-agents.html` — the deck. **This is the file we edit.**
- `deck/security-of-ai-agents.bundle.html` — self-contained build (assets inlined via
  `deck/inline_assets.py`) for presenting offline. Regenerate after editing the deck.
- `deck/assets/` — images, video, and baked transcript/result JSON the slides pull from.
- `deck/assets/readme/` — slide exports used by `README.md`, generated from
  `deck/security-of-ai-agents.pdf` with `pdftoppm -png -r 60 -f N -l N -singlefile`.
- `deck/DEMO_RUNBOOK.md` — how the live demo is run on stage (primary / backup / fallback paths).

Supporting the deck is the live red-team demo:

- `adc_demo_redteam.py` — goal-hijacking red-team run against `agent:clarabelle-cow`
  via evaluatorq. Backup path when the `orq-red-team` skill stalls. (See runbook.)
- `provision.py` — idempotently provisions the `clarabelle-cow` agent + `clarabelle-still-a-cow`
  evaluator on orq (research workspace, ADC project). Safe to re-run.
- `agents/clarabelle_*.txt` — the cow persona prompt, attacker instructions, judge prompt.
- `data/redteam-runs/` — sanitized run reports, regenerated from `.evaluatorq/runs/` with
  `sanitize_runs.py`. The dashboard in `hf-space/` serves them.
- `thoughts.md` — talk narrative / content planning notes (still the ADC narrative).
- `results/` — git-ignored run output; the script writes `results/03_summary_report.json`.

## Conventions

- Edit `deck/security-of-ai-agents.html` directly; rebuild the bundle with
  `uv run python deck/inline_assets.py` (check the script for exact in/out paths).
- `.env` holds the `ORQ_API_KEY`; it is git-ignored, copy it in from the ADC checkout or
  your orq workspace. The red-team script force-pops `OPENAI_API_KEY` so calls route
  through the ORQ router.
- Core deps are `evaluatorq[orq,redteam]` + `orq-ai-sdk`. Python 3.12+, managed with `uv`.
- `index.html` at the repo root is a redirect to the deck, for GitHub Pages. Pages is not
  enabled on this repo yet; the README still links at the ADC Pages URL.
