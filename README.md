# Revenue Intelligence Inbox

**Live application:** https://padmadevd.github.io/volopay_revenue_inbox/

## Overview

Revenue Intelligence Inbox is a single-page browser application for reviewing call and meeting activity, inspecting conversation evidence, deciding where each conversation's analysis belongs, managing manual route corrections, and exporting organised review outputs.

The application is built as one HTML file with internal CSS and JavaScript. It requires no build step, API key, or runtime AI service. Records, manual routing decisions, and replay state are saved in the browser's local storage.

## Features

- **Manager dashboard:** Displays activity and coverage metrics for the selected records, with source-type and person filters. Activity dates use the Asia/Kolkata timezone. Call representatives and meeting organisers are treated as different kinds of metadata.
- **Conversation inbox:** Search and filter records by source and review status. Open a record to inspect its metadata, supplied conversation text, suggested routes, analysis, supporting evidence, limitations, and next-step guidance.
- **Evidence-aware routing:** Supports routes including Customer Success, Sales Follow-up, Sales Coaching, Partner / Vendor, Product / Engineering, Contact Restraint, Internal / Operations, and Needs Human Review. A record may have more than one route.
- **Manual corrections:** Managers can override suggested routes. Manual choices are stored separately from source revisions so a later data update does not silently remove the correction.
- **Manager brief:** Includes a concise brief for 21 August 2026, with activity and review priorities.
- **Import and QA tools:** Simulate the initial import, repeated imports in shuffled order, a later update, and replay of an older batch. The app also provides controls for importing official JSON delivery files.
- **Organised exports:** Download a ZIP containing the manager brief, a manifest in JSON and CSV, a short export README, and Markdown outputs grouped by route. Multi-route records produce separate route-specific files while retaining the same source identity.
- **Responsive layout:** The workspace is designed for desktop and smaller browser windows.

## Getting started

1. Open the [live application](https://padmadevd.github.io/volopay_revenue_inbox/) in a recent Chrome, Edge, Firefox, or Safari browser.
2. Use the left-side navigation to open the dashboard, inbox, manager brief, exports, or **Imports & QA**.
3. Select a dashboard filter or click a route/activity area to move into the relevant records.
4. In the inbox, search for a record such as **C01** or **M07**, select it, inspect the evidence, and adjust its route when needed.
5. Open **Exports & manifest** and download the ZIP or a standalone manifest.

## Reproduce the import/update test

Open **Imports & QA** and follow these steps:

1. Click **Reset to initial**. The demo should show 44 unique records and 17 usable conversation texts.
2. Click **Repeat initial (shuffled)**. The unique record count should remain 44, with no duplicate activity added.
3. Open **Inbox**, select **M07**, and save a manual route correction. For a simple demonstration, choose **Customer Success**.
4. Return to **Imports & QA** and click **Apply update**. M07's newer text should be applied without creating another record; usable text coverage should rise from 17 to 18.
5. Click **Re-import old initial**. Revision 1 should not overwrite M07's revision 2 update, and the manual route should remain in effect.
6. Refresh the browser and confirm the update and manual choice persist.
7. Click **Run all replay checks** to display the built-in QA results. Use **Clear local state & reset** to restore the final demo state; use **Reset to initial** to replay from the beginning.

### Importing official JSON deliveries

In **Imports & QA**, select the initial delivery JSON under **Initial delivery · revision 1** and click **Import official initial JSON**. Then select the update JSON under **Update delivery · revision 2** and click **Import official update JSON**.

The files should use the envelope format defined by the data contract and include a `records` array. The app identifies records by `(source_type, source_id)`, applies newer revisions, ignores the same or older revisions, and keeps manual route decisions separate from source data.

## Metrics and review states

- **Unique records:** distinct `(source_type, source_id)` pairs after the revision rules have been applied. Batch sizes are not added together.
- **Usable supplied text:** a nonblank call transcript or at least one nonblank meeting transcript turn.
- **Transcript coverage:** records with usable supplied text divided by selected unique records. Coverage indicates text availability, not transcript quality or call connection rate.
- **Route assignments:** the number of route labels across records. This may be greater than the number of records because multi-route assignments are allowed.
- **Needs attention:** includes records with missing supplied text, pending review, or a route requiring human judgment. Missing text does not prove that a conversation never happened.
- **Analysis freshness:** when new source text arrives, the app marks the analysis as pending review rather than presenting an old draft as if it reflected the new text.

## Data and processing notes

Routing suggestions use deterministic local rules, and the app includes prepared draft analyses for the supplied adapted conversation texts. The app does not call a live large language model, CRM, or other network service to analyse conversations. Review the cited evidence before acting on a recommendation.

The original `data/01_initial.json` and `data/02_update.json` files were not included in the provided upload package. The demo uses the 18 supplied adapted conversation texts and explicitly flags 26 metadata-only rows as reconstructed fixtures to demonstrate the 44-record workflow. Import the official JSON deliveries before relying on the fixture metadata as authoritative source data.

## Data storage and privacy

The app saves application state in the current browser's local storage. Changes are local to that browser and site origin; they are not synchronised to another device or user. Clearing the browser's site data may remove saved corrections and replay state. Use a hosting and sharing setup appropriate for the sensitivity of the data.

## Export contents

The generated ZIP includes:

- `manager-brief.md` — the daily manager brief.
- `manifest.json` and `manifest.csv` — record identity, applied revision, routes, output paths, review state, manual-override flag, and analysis freshness.
- `README.txt` — a brief description of the exported structure.
- `outputs/<route-slug>/...md` — one Markdown output per active route assignment.

Export paths are relative paths within the ZIP. If one record has multiple routes, each route-specific output retains the same source record identity.
