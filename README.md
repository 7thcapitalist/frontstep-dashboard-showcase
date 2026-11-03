# Frontstep Operations Dashboard (showcase)

Public showcase for a private project. The source code is not published here.

A read-only operations view for Frontstep, a website-building pipeline for small businesses. It answers: how many leads, how many messages, how many previews, how many conversions, what does it cost, and what is broken. It is built so that it does not report numbers it cannot back up.

## The problem

The pipeline spans several tools, and each reports its own counts. Naive dashboards add them up, count a send as a delivery, or turn a missing value into zero. I wanted one view where each number says where it came from and gaps stay visible.

## How it works

- It reads a private summary of the business and shows it in one place.
- A second source from the automation side is shown in its own panel and never merged into the first.
- A summary older than 24 hours is marked stale, as a recency warning only.
- If the source is missing, the dashboard says "disconnected" and shows unknown instead of zeros.

## Principles

- Unknown is not zero. Zero means there were verifiably none.
- Each event counts once, and conflicting records for the same event raise a warning instead of silently picking one.
- Sent, delivered, preview built and preview sent are separate facts.
- Cash needs evidence: only settled transactions count. Forecasts are kept apart and never enter cash.
- Money is never summed across currencies.
- Private details do not leak into what the dashboard shows.

## Status

Early prototype from October 2026. Live data, automatic refresh and production use are not set up. It is the precursor to the holding-level console.

## What is shown and what is kept private

Only this README is public. The source is private because it is operations tooling for a live business.
