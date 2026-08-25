Run the Sahajta AI daily Wispr Flow → Notion meeting-filing workflow. This automation starts on 3 August 2026; never backfill meetings before 2026-08-03 Asia/Kolkata unless Pranamya explicitly requests it.

SOURCE AND WINDOW
- Use the connected wispr-flow MCP. No Wispr writes are needed.
- Determine the current calendar date in Asia/Kolkata.
- Search all Wispr Flow meetings whose meeting start time falls from 00:00 Asia/Kolkata today through the current run time. Convert bounds to UTC for search_meetings.
- Use stable Wispr meeting IDs for deduplication.
- Fetch each qualifying meeting's summary/content and full transcript. If transcript data is paginated or truncated, fetch successive character ranges until complete.

EMPTY RECORDING SAFEGUARD
- After completing all transcript pagination, trim whitespace and inspect both the Flow Summary and transcript.
- If both the Flow Summary and transcript are empty, do not create or update any Notion entry for that meeting.
- Report that meeting in the run summary using exactly: "Skipped — no recording or notes available."
- Never delete, rename, or otherwise modify the source meeting in Wispr Flow.
- If only the Flow Summary is empty but a transcript exists, file the meeting normally and state inside the Flow Summary area that no Flow Summary was generated.
- If a Flow Summary exists but the transcript is empty, file the available summary normally and leave the transcript section empty.

CLASSIFICATION
Perform case-insensitive substring matching in this exact table order; first match wins. Keywords match the meeting title unless a row explicitly says to inspect fetched Flow Summary/content:
- Durable business context: Scantrix is an app Sahajta AI is building for client Saurabh, who is the founder of Savetrix. Therefore Scantrix-related meetings belong in the Savetrix/Saurabh client destination.
1. "Kunj" → Notion parent 3b005b3d-f0d4-80d7-82d9-e8665b1b97bf (Kunj Shah)
2. "Daksh" → 3b005b3d-f0d4-804c-be3e-dabd2ab62e23 (Daksh Shrivastav)
3. "Saiyash" → 3b005b3d-f0d4-80ce-97e0-d2b856e992a5 (Saiyash Poojari)
4. "Savetrix" or "Saurabh" in the title, OR "Scantrix" in the title or fetched Flow Summary/content → 3b005b3d-f0d4-8007-a00b-f407dbf65102 (Meeting with Savetrix Client- Saurabh)
5. "Renuka" → 3b005b3d-f0d4-80a4-90ad-f2c6edbea356 (Meetings with Renuka)
6. "ThinkAIWork", "Rahul", or "CFO Club" → 3b005b3d-f0d4-8077-9ad5-e2aa9313284e (ThinkAIWork Content & CFO Club Launch)
7. No match → 3b005b3d-f0d4-813c-adc6-f817438004d2 (Random Meetings)
If more than one table row matches, use the first row in table order and flag the conflict in the run summary. Do not guess from partial or semantic similarity. An unmatched meeting is safely filed in Random Meetings and is not an error.

NOTION FILING
- Use the connected Notion app.
- Convert each meeting's start time to Asia/Kolkata and use date-page title "D Month YYYY", for example "3 August 2026". No ordinal suffix or abbreviated month.
- Under the classified parent, find a direct child page whose title exactly matches that date title.
- If it exists, inspect current content and append the next unused "Meet N" block. Never overwrite an existing block.
- If it does not exist, create it under the classified parent with one "Meet 1" block.
- Before writing, prevent duplicates by checking whether the same Wispr meeting ID or unmistakably identical title/start time is already filed on that destination date page. Skip duplicates and report them.
- Preserve all historical pages and their older naming.
- Format each meeting exactly:

<details>
<summary>Meet N</summary>

## **Flow Summary** {toggle="true"}

[Flow Summary content, retaining its headed bullet-section structure]

<details>
<summary>Ts</summary>

[Full speaker-labeled transcript, one turn per line]

</details>

</details>

FAILURE AND HUMAN-IN-THE-LOOP
- On a Notion write failure, refetch current state and retry once safely. If it fails again, report the meeting ID, destination, and error here; do not silently drop or duplicate it.
- Involve Pranamya only for genuine structural ambiguity, inability to create or append safely, repeated tool/write failure, or an unresolved rule conflict.
- Do not ask about routine unmatched meetings; file them in Random Meetings.
- Never change unrelated Notion or Wispr content.

RUN SUMMARY
After every run, post: local date/window; counts found, filed, duplicate-skipped, and empty-recording-skipped; destinations and meeting titles; each "Skipped — no recording or notes available." item; Random Meetings items; multi-keyword conflicts; and failures/questions. If none are found, say so clearly.
