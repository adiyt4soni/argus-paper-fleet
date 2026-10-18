# Argus paper fleet (Codex)

Codex version of the Argus paper-trading run: refresh price data, verify the hash chains, or list the queued paper orders, without ever trading.

**Type:** Codex skill · **Area:** Investing research (paper money)

## What it does

- Uses `fleet plan` as the single source of the data calls to make, imports each result, then rebuilds, steps and verifies the normal and stress chains.
- For the open report, lists queued paper orders as observations, not recommendations.
- Stops on any command error, a rebuild that doesn't advance, or a non-zero `broken` count.

## Guardrails

- Paper money only. Read-only historical-data calls; no brokerage or account-changing action, ever.

## When it triggers

> Run or report the scheduled Argus 2.0 paper fleet: refresh data, verify chains, or list queued paper orders without trading or changing accounts.

## What's in this repo

- `SKILL.md`: the skill itself, the file Claude or Codex reads.
- Not included: Argus 2.0 itself (a Python standard-library paper-trading research engine) is private and not published.
- Written to be generic: where the skill relies on the user's own files, it names them by role, not by content.

## Install

```bash
mkdir -p ~/.codex/skills/argus-paper-fleet
curl -fsSL https://raw.githubusercontent.com/adiyt4soni/argus-paper-fleet/main/SKILL.md -o ~/.codex/skills/argus-paper-fleet/SKILL.md
```
