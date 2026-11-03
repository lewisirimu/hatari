# Hatari: one-pager

> **Status:** early development. Hatari (Swahili for "danger") is a fault-injection tool for payment integrations.

## Problem

Payment integrations break when providers send duplicate, late, out-of-order or ambiguous responses: a webhook arrives twice, a "pending" event lands after "succeeded", or a request times out even though the money moved. These failures cause double credits, lost payments and ledgers that don't balance, and they usually surface in production, where they cost real money. Unit tests and provider test environments rarely reproduce them, because events there tend to arrive once, in order and on time.

## What Hatari does

Hatari runs your payment integration against an adversarial fake provider, with card-style and M-Pesa-style APIs, that misbehaves on purpose while simulated customers make payments, retries and refunds. It records the full history of every run and checks it against the rules money must obey: no double credits, no lost payments, and books that always balance. Every violation comes with a plain-language explanation and a seed that replays the run, shrunk to the smallest sequence of events that still triggers the bug.

## Who it is for

- **Teams integrating M-Pesa (Daraja) or card processors** who want to know how their code behaves when the provider misbehaves, before their customers find out.
- **Maintainers of open-source payment, billing and e-commerce software** who want reproducible evidence of money bugs, with a minimal reproduction attached.
- **Engineers learning to build correct payment systems.** The bundled `naive-shop` and `careful-shop` apps show the common mistakes and their fixes side by side.

## Non-goals

- **Not a load-testing tool.** Hatari looks for correctness bugs, not throughput limits.
- **Never run against production traffic.** It targets systems you run yourself, locally or in a test environment.
- **Never touches real money or live keys.** It only ever talks to its own fake provider.

## Success measures for v0.1

- Finds every planted bug in `naive-shop`.
- Zero false alarms on `careful-shop` across 150 runs.
- Real bugs reported responsibly in at least 2 open-source projects.
- v0.1 published on PyPI.