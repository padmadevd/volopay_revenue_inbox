# Revenue Intelligence Inbox

## Deliverables
- `index.html` — the app in the submission bundle; a single HTML file containing all CSS and JavaScript. The same file is also provided separately as `revenue_intelligence_inbox.html`. No build step or API key is required.
- `Revenue_Intelligence_Inbox_Findings.pptx` — five editable slides with source-linked findings and trade-offs.
- `voiceover_walkthrough.txt` — timed narration/click guide for a roughly three-minute continuous screen recording.
- `voiceover_tts_clean.txt` — narration text with section labels removed, ready to paste into TTS.
- `Revenue_Intelligence_Export.zip` — sample organised Markdown outputs plus `manifest.json` and `manifest.csv`.

## Run
1. Open `index.html` (or the separately downloaded `revenue_intelligence_inbox.html`) in a recent Chrome, Edge, Firefox, or Safari browser.
2. The demo opens in final post-update state. Local storage persists route edits and replay state for that browser origin. Use the “Imports & QA” screen → “Reset to initial” for a clean replay.
3. The app is entirely local/offline. It has no runtime LLM. For a web deployment, upload this single HTML file, named `index.html`, to a static host that serves HTML as the site root; verify its access/privacy settings before sharing. No hosted URL has been provisioned by this build process.

## Reproduce the replay test
1. Select **Imports & QA** and click **Reset to initial**: expected 44 unique records (30 calls, 14 meetings) and 17 usable texts.
2. Click **Repeat initial (shuffled)**. Identity is `(source_type, source_id)`, so counts should remain 44 records / 17 texts.
3. Open the M07 row in the Inbox while in revision 1. Give it a manual route (suggested for the demo: Customer Success) and save the correction.
4. Return to **Imports & QA** and click **Apply update**. M07 is upserted to revision 2, text coverage becomes 18, and the manual route remains in effect. Its analysis is labelled pending review because new text arrived.
5. Click **Replay old initial**. Revision 1 must not overwrite M07 revision 2.
6. Refresh the browser and confirm the route correction and update remain. Run **Run replay checks** for the checks panel. “Clear local state” reloads the final-state demo; use reset-to-initial to start the sequence again.

## Metrics
- Unique records: count of distinct `(source_type, source_id)` after revision rules apply; never sum batch sizes.
- Usable supplied text: nonblank call transcript OR at least one nonblank meeting turn. Coverage = covered unique records divided by selected unique records. It is not a connection rate or quality score.
- Activity day: calls use `started_at`; meetings use `starts_at`; date rendering is Asia/Kolkata. An organiser is not assumed to be the seller or an attendee. Missing dates are not inferred from batch delivery time.
- Route assignments: route labels summed across unique records; can exceed record count because multiple routes are permitted.
- Needs attention: records with missing text, pending review, or human-review routing. Missing text does not prove that no conversation happened.
- No revenue, pipeline-value, win-rate, or cross-rep performance metric is calculated because the provided scope does not support it.

## Routing and analysis choices
The application uses deterministic local keyword rules plus conversation-specific prepared draft analyses for the 18 adapted text records. It does not call an LLM, CRM, or other network service. The analysis includes a reason/next step/evidence quote/limits. Updated text is marked `pending_review` rather than showing the old analysis as if it were fresh. Routes include Customer Success, Sales Follow-up, Sales Coaching, Partner / Vendor, Product / Engineering, Contact Restraint, Internal / Operations, and Needs Human Review. Manual route overrides are stored separately from source revisions.

## Export structure
The app can download a ZIP containing `manager-brief.md`, `manifest.json`, `manifest.csv`, `README.txt`, and one Markdown file per active route assignment under `outputs/<route-slug>/`. The generated manifest stores source identity, revision, routes, paths, review state, manual-override flag and analysis freshness. Multi-route records create one file per route with the same source identity. The included ZIP is a sample export in the uncorrected final-state baseline; regenerate from the app after making manual corrections.

## Three findings and product impact
1. **Contact boundary (C01):** call source ID `2ec4404c-beee-5377-a63c-aab701da08ed`, exact adapted quote: “I didn't ask for this call, and I'm not interested.” This routes to Contact Restraint + Sales Coaching and prevents generic follow-up logic from overriding explicit disinterest.
2. **Saved description defect symptom (M07):** meeting source ID `4f304fff-fd40-5cfc-a366-0a134c445c2a`, Turn 2 describes editing and saving the event-payment description; Turn 3 confirms the old wording still displays. This routes to Customer Success + Product / Engineering, with follow-up captured separately. The root cause is not established; new revision analysis is pending review.
3. **Expired reset token (M09):** meeting source ID `4b5b9192-f3fc-505a-8e77-4d2ffaa54594`, Turn 21: “It says the token has expired. This is the fresh message we just received.” This supports separate access/reset triage rather than merging every account setup symptom into one issue.

## Deliberately rejected decision
I rejected a cross-person conversion leaderboard or opportunity-dollar ranking. The input package has no authoritative CRM opportunity values or outcomes, no verified staff-role directory, and it is a selected extract rather than a representative full-team day. The accepted trade-off is smaller, traceable metrics with stated denominators rather than spurious precision.

## Important data limitation
The uploads provided the brief, dictionary, conversation library, and reading-pack HTML, but not the referenced `data/01_initial.json` and `data/02_update.json`. The app therefore uses the 18 supplied conversation texts from `CONVERSATIONS.md`, stages M07's supplied text only in revision 2, and uses 26 explicitly flagged reconstructed metadata-only fixture rows to reach the stated 44 unique records (30 calls + 14 meetings). The app supports importing the official JSON envelope files and will use them to replace the demo fixture. Treat metadata claims from those 26 rows as fixture-only until the official JSON files are imported.

## Hosting and recording
This file is static-host-ready but has **not** been published to a public URL from this environment. Upload the HTML to your chosen static host and check it in an incognito window before sharing. Record one continuous three-minute screen capture while performing the steps in `voiceover_walkthrough.txt`; render the supplied script with your own TTS voice, mix it in your screen-recording tool, and save the final video. No screen recording or voice track is fabricated here.

## Time spent
Work time was not tracked precisely. Known limitations: missing official JSON delivery files; reconstructed metadata fixture rows; no remote persistence beyond browser storage; deterministic routing and prewritten analysis rather than live AI; no public deployment URL or recorded video generated in this session.
