# Frontstep Operations Dashboard (showcase)

Public showcase for a private project. The source code is not published here.

A read-only operations view for Frontstep, a website-building pipeline for small businesses. It answers: how many leads, how many messages, how many previews, how many conversions, what does it cost, and what is broken. It is built so that it does not report numbers it cannot back up.

## The problem

The pipeline spans a CRM sheet, an automation platform, a messaging gateway and a build worker. Each reports its own counts, and naive dashboards add them up, count a send as a delivery, or turn a missing value into zero. I wanted one view where each number carries its provenance and gaps stay visible.

## How it works

- The server reads a **private aggregate snapshot** (schema v1) from a local file. It binds only to localhost.
- An optional separate **automation-platform aggregate** is shown in its own panel and never merged with the CRM panel.
- The UI marks a snapshot older than 24 hours as stale. That is a recency warning only, not a health check.
- If the source is missing or unreadable, the dashboard shows "disconnected" and unknown metrics instead of zeros.
- The public deployment of the API intentionally returns the disconnected state until a verified private access layer exists.

## Design notes

- **`null` is unknown, zero is verified none.** The contract says so explicitly.
- **Events are deduplicated by a stable `(source, id)` pair.** Conflicting payloads for the same id produce a collision warning instead of silently picking one. Different id namespaces are kept apart.
- **Sent, delivered, preview built and preview sent are separate facts.**
- **Cash needs evidence.** Actual cash requires a settled transaction. Forecasts live in a different field and never enter cash.
- **Money is integer minor units plus ISO currency.** No floats, no cross-currency sums.
- **No polling** of the automation platform, because the plan has a monthly execution cap.
- **A shared overview contract** lets a holding-level dashboard read one record per factory without combining missing values into zero.
- A privacy test checks that private fields do not leak into the API output.

## Architecture

Dependency-free Node server plus static front end; adapters for the snapshot, an event roll-up module, a ledger module and an automation-aggregate module; a serverless entry point for deployment; unit tests with `node --test`.

## Stack

Node.js 20+ (no packages), plain JavaScript, HTML/CSS, Vercel.

## Status

Early prototype from October 2026 (two commits). It runs locally on an observed snapshot. Live credentials, unattended refresh and production deployment are not set up. It is the precursor to the holding-level console.

## What is shown and what is kept private

Only this README is public. The repository is private because it is operations tooling for a live business and handles lead and message data, even though the dashboard works on aggregates.
