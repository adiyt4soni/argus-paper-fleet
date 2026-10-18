---
name: argus-paper-fleet
description: Run or report the scheduled Argus 2.0 paper fleet: refresh data, verify chains, or list queued paper orders without trading or changing accounts.
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Bash
---

# Argus paper fleet

## When to use

Use for a scheduled daily/intraday paper-fleet refresh or a queued-order report in the Argus 2.0 project folder. Do not use it to place trades, call any order or account-changing brokerage tool, alter accounts, delete or overwrite data, or bypass an explicit stop condition.

## Inputs / context to gather

1. Confirm whether the request is daily refresh, intraday refresh, open-order report, or a combination, and whether US markets are open.
2. Read the task's frozen/Addendum H constraints and reporting requirements. Preserve paper-money-only status.

## Procedure

1. Work from the project folder and run `python3 -m argus.cli ...`.
2. For daily refresh, run `fleet plan`; fetch only the planned historical data with `get_equity_historicals`; import each saved result using the planned `none` or `split` adjustment; then run rebuild, normal/stress step, and normal/stress verify.
3. For intraday refresh, follow the generated plan exactly. Do not substitute symbols, interval, adjustment, or dates. Current unfinished sessions during market hours may remain unimported.
4. For an open-order report, run `fleet orders` and `fleet verify`. Report queued symbols as current observations, not recommendations or trade instructions.
5. Stop and report on a command error, non-advancing rebuild when that is a stated stop condition, a nonzero `broken` count, or an unexpected missing artifact. Otherwise give a short plain-English, sectioned paper-money report and update local session memory only if the task explicitly asks.

## Efficiency plan

- Use `fleet plan` as the single source of planned daily calls and retain concise import/rebuild/verify status lines.
- Treat `processed []` and `unfinished sessions not imported (not closed yet)` as expected only when they match live market-hours context; do not spend calls trying to force incomplete data.
- Run verification once per applicable normal, stress, or intraday chain and stop on the first unexpected failure.

## Pitfalls and fixes

- `fleet_prices rebuilt through ...` did not advance -> stop and report; do not explain it away.
- `broken` is nonzero or `fleet verify` errors -> stop and report; do not continue to performance conclusions.
- `(eval):1: =STEP not found` in zsh -> quote any `echo ==STEP` label, then rerun only the remaining command.
- An L account's decision artifact is unexpectedly absent -> report it as an unexpected condition without guessing the cause.

## Verification checklist

- Only allowed read-only historical-data operations were used; no brokerage/account-changing action occurred.
- Every applicable verification command exited successfully with `broken: 0`.
- Market-hours exclusions are reported as such, not treated as a successful completed session.
- Any report includes paper-money/not-recommendation framing and only compares controls within the same section.
