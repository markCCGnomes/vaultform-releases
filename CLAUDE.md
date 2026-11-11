# vaultform-releases


<!-- happs-coordination-kit:start -->
@../_coordination/SHARED.md

## Cross-happ coordination protocol

- `../_coordination/SHARED.md` (imported above) is the shared source of truth for all happs in this
  family. It loads once at session start but OTHER SESSIONS UPDATE IT — re-read the file before
  starting any new feature work, before acting on test feedback, and whenever the user says
  "check the shared file". Its `## Family rules` section is binding — read it.
- **Stay in your lane:** work ONLY inside this project's folder. If work you're asked to do or
  discover belongs to another project (see the Ownership map in SHARED.md), do NOT do it or edit
  their files — add a `todo` row for the OWNING project in `## Assignments`, broadcast it, and stop.
- **Route, don't spawn:** never launch, role-play, or dispatch another worker/agent to do project
  work. Record that the work is waiting and tell Mark which worker to run (`.\worker.ps1 <project>`).
- **Version ceiling:** hold every version at **1.0** (`1.0.0` max — no 1.0.1/1.1/2.0). Stage work
  but don't bump past or publish a >1.0 release until Mark confirms the public 1.0 launch is done.
- **⚠ BUILD LAST, THEN STAGE (family rule 7; publish gate rewritten by Mark 2026-08-30).** Finish
  every requested fix FIRST, cut the final artifact LAST, then **stage it**. An image or
  installer cut *before* the stacked fixes land is a wasted build — Mark wakes to an artifact
  missing exactly what he asked for. That is not hypothetical: on 07-27 vault-node cut rc.23 and
  gate-passed it twice overnight with the claim-contract work still stacked behind it, so the
  artifact was superseded by its own remaining task list before Mark ever saw it.
  - **Order your session:** all code → build → gate → stage (install locally + record shas). If a requested fix cannot land,
    cut the build anyway but state in your handoff **which requested fixes the artifact does NOT
    contain**, so nobody flashes it expecting them.
  - **⚖ AUTO-PUBLISH IS RETIRED (Mark, 2026-08-30).** Nothing publishes to the mirrors (any
    `*-releases` repo / `releases/latest` / winget / website bumps) until (a) the build is
    confirmed STABLE (gate-passed + live-verified on real hardware), (b) Mark has EXPLICITLY
    cleared that artifact, and (c) the Discord/socials post is ready so release + announcement
    land together. Workers STAGE (local install on Mark's desktop + shas in the handoff) and
    stop — "staged, awaiting Mark's clearance + socials" is rule 7's done-state now.
  - **Overnight/dispatched work:** the deliverable is a *staged, locally-installed* artifact
    carrying every requested fix, with shas recorded — ready for Mark to test and clear.
- **⚠ ANDROID VERIFICATION RUNS ON AN EMULATOR BY DEFAULT (Mark, 2026-07-27 — family rule 8).**
  **Never queue Android work behind the S22+.** An **x86_64 API-36 emulator runs our ARM64-ONLY
  APKs unmodified** — no ABI change, no second build (`adb shell getprop ro.product.cpu.abilist`
  → `x86_64,arm64-v8a`; `/system/lib64/libndk_translation.so` present). Spin up your OWN instance,
  don't share one; pass `-s` if `adb devices` shows more than one target.
  - **The emulator covers most of it:** layout, insets, system bars, back button, keyboard/IME,
    persistence across restart, and **both nav modes** (it can toggle gesture vs 3-button, which
    One UI's `cmd overlay --navbar.threebutton` cannot). It also *enforces* edge-to-edge, catching
    the Android 15+ class that the Android 14 phone never can.
  - **Only these need the real phone:** carrier SMS/MMS, camera/sensors, real network conditions,
    One UI-specific behaviour, and a final pre-release smoke.
  - **Three traps, each already paid for:** assert `innerHeight > 0` before believing any inset
    reading; **screenshot before trusting an IME probe** (a floating keyboard legitimately reports
    height 0); and `.click()` succeeds on an OFF-SCREEN element — compare `getBoundingClientRect()`
    against `innerWidth` at shipped widths.
  - Report `PASS / FIXED / UNVERIFIED` **and name the target** (emulator API level, or the S22+).
    Never write "emulator-verified" as "device-verified" — and never let "no phone" mean "unverified".
- **Broadcast:** whenever you make a decision or change that affects other happs (zome call
  signatures, entry types, signal payloads, DNA properties, the integration test flow), append a
  dated entry to the `## Broadcasts` section of SHARED.md naming the affected happs. Use
  `../_coordination/scripts/broadcast.ps1 "message" -From <this-project>` or edit the file directly.
- **⚠ WHERE EVIDENCE GOES — the rule that keeps SHARED.md usable (2026-07-26).** SHARED.md is
  imported into EVERY project's CLAUDE.md at session start, so every byte in it is paid **ten times
  over**, by every worker, every session — before any work starts. At ~150k it broke every
  session's reads (07-23) and it has since regrown twice. There is no system character limit; the
  limit is that context budget.
  So: **the detailed evidence trail belongs in `handoffs/<your-project>.md`, NOT in SHARED.md.**
  A handoff is read on demand by one session; a SHARED.md row is loaded by all of them.
  - An **Assignments row** carries: what to do · dependency · a **one-line** status ·
    a pointer (`see handoffs/X.md` or a commit hash). Not the narrative.
  - A **broadcast** carries what OTHER projects must change or check, and why, in a few
    sentences. Your reasoning, your method notes, your gotchas and your rule-5 transcript go in
    your handoff. If a broadcast is over ~1,200 characters, the surplus is almost always evidence
    that belongs in your handoff.
  - **Finishing a row: put the word `done` at the START of the status cell.** `archive-shared.ps1`
    only auto-archives rows whose status *starts* with a finished-keyword — evidence written ahead
    of that word makes the row unrotatable forever, which is how the file grew.
  This is not about being terse for its own sake. Long rows are how a real finding gets missed:
  the signal is in there, but nobody reads 6 KB of a table cell.
- **Assignments — "check the shared file and run your assignments" (or "…start your assignment",
  or any similar kickoff) means: work EVERY row for this project, to completion, in ONE session.**
  Not one row. **Your rows are enumerated for you in `../_coordination/rows/<this-project>.md`**
  (regenerated from SHARED.md at every session start and injected into your context) — that is the
  AUTHORITATIVE, COMPLETE list of what you own, so you do not have to find your rows by scanning the
  460 KB board. Treat it as your checklist. For each row, re-read its full detail in the
  `## Assignments` table, check its dependencies, take the highest-impact runnable one, **finish it,
  then take the next** — repeat until every row on your manifest is accounted for. Update each row's
  status (in-progress / blocked / done) as you go. **A row missing from your manifest is a row you
  no longer own; a row present on it is yours until its status says otherwise — you cannot quietly
  skip it.**
  - **"Finish" is defined by rule 7: land the fixes, cut the artifact LAST, then STAGE it**
    (gate + local install on Mark's desktop + shas recorded). Publishing needs Mark's explicit
    per-artifact clearance + confirmed stability + the socials post (2026-08-30 rule). If a fix
    genuinely cannot land, stage anyway and state which requested fixes the artifact lacks.
  - **Rule 7 is now enforced by a PreToolUse gate, not just this text.** A heavy build (APK/AAB,
    `tauri build`, `cargo build --release`, publish) is BLOCKED while you still own an open row on
    the board. To clear it: finish the rows. If one genuinely cannot be worked this session, set its
    SHARED.md status to START with `done — …`, `blocked — …`, or `deferred — …` (accounting word
    first, no leading emoji). The gate reads SHARED.md live, so an honest status clears it — and the
    skip is now a visible board entry Mark can see, not a silent cherry-pick.
  - **Do not stop early.** Only wrap up when the board has nothing runnable for you — then state
    precisely what you are waiting on and who owns it. A row you skipped because it looked like
    someone else's problem is still your row until you have said so on the board.
- **Re-check on completion:** when you finish your task list, RE-READ SHARED.md (Broadcasts
  newest-last, then Assignments) before wrapping up — work lands mid-session. If a runnable row
  for this project appears, CONTINUE; only wrap up when a re-read shows nothing runnable for you.
- **Watch your dependencies, do not wrap up on them (family rule 13, Mark 2026-09-02):** if a row
  of yours depends on another lane's work, do every non-dependent item first, then WATCH for the
  signal named in the row's deps cell (a broadcast phrase, a commit on a named repo/branch, a file
  path, a posted hash) — poll SHARED.md Broadcasts and/or `git fetch` the upstream repo every few
  minutes; the moment it lands, START the dependent leg in this same session. Wrap up only when
  nothing is runnable AND every watched dependency has landed-and-been-built-on or its owner posted
  `blocked —`/`deferred —`. When YOUR leg is someone else's dependency, post the signal EXACTLY as
  their row names it. Mark launches all workers at once; sequencing lives on rows, never with him.
- **Waiting costs ZERO tokens (Mark 2026-09-02, credits):** never poll with model turns. To watch for a
  signal, run ONE background bash wait and let its exit re-invoke you:
  `until grep -qE '\[<OwnerLane>\] SIGNAL: THE SIGNAL PHRASE' ../_coordination/SHARED.md; do sleep 120; done`
  (run_in_background). The `SIGNAL:` marker is written ONLY by `post.ps1 -Signal`; never type
  "SIGNAL:" in prose, rows or handoffs. Plain or tag-anchored greps fired falsely twice on 2026-09-03
  (prose quoting the phrase; a lane's own "I will post X" line),
  or `git fetch` + `git log origin/<branch>` in the same loop. No re-reading the board every few minutes.
- **Stuck twice? Say so, do not grind (model ladder, Mark 2026-09-02):** sessions start on sonnet. If the
  same slice fails twice, write `stuck after 2 tries — <what failed>` on the row status and move to the
  next runnable item; master relaunches that slice on Opus 4.8 (then Fable 5.1 only after two more).
- **Everything on D:, purge unused cache (family rule 15, Mark 2026-09-02):** build artifacts, caches,
  temp, profile copies, emulators, containers all go under D: (`D:\DevCache\<tool>`, `D:\Temp`) — never
  new writes under `C:\Users\…`. Keep a cache only if it speeds the next build; at wrap-up delete what
  your session created that no future build needs (throwaway worktree `target/`, fixture copies after
  their cert, superseded emulator images, exited containers) and say so in one line. Never touch
  another lane's live fixture or `~/.vault-secrets`.
- **Context is the cost (Mark 2026-09-02, credits):** a session that has run for hours re-reads its whole
  history on EVERY turn, so long sessions are the expensive ones. When your slice lands, WRAP UP (handoff
  with an exact resume point, rule-15 purge, one ≤300-char board line) — do not linger to "watch"; master
  relaunches a FRESH sonnet session for the next slice from your handoff. Message master only with a
  milestone signal or a blocker; never status narration, never per-step updates.
- **Board edits go through the helpers, never ad-hoc (master, 2026-09-03):** post a broadcast with
  `powershell -File ../_coordination/scripts/post.ps1 -Lane <You> -Text "..." [-Signal]`; update your row
  with `../_coordination/scripts/row.ps1 -Id R-nnnn -Status "..."` (the ID is at the end of the row's
  first cell); file a row with `row.ps1 -Add ...`. Line numbers shift; IDs do not. Cite `R-nnnn` in posts.
- **Signals are verbatim (signals.md):** the phrase a dependent watches is registered when the row is
  filed; post it EXACTLY with `post.ps1 -Lane <You> -Signal -Text "PHRASE <hash/commit> ..."` (adds the `SIGNAL:` marker). Wrap-up checklist: every signal I owe is POSTED verbatim,
  handoff written, rule-15 purge, one ≤300-char board line.
- **Heartbeat (master sweep, 2026-09-03):** master measures progress, not promises. While you work,
  append one line to your handoff (`progress HH:MM: <step>, log <path>`) at least every 20 minutes and
  whenever you start a background job (name its log path in backticks). No post AND no handoff change
  for 20 min reads as STALLED and gets you kicked, then relaunched. When a background job finishes,
  act on it in the same turn — check its log's EXIT line yourself; do not wait to be told.
- **Report terse:** a broadcast is ≤ 600 characters — the signal phrase, commit/sha/path, the one-line
  result, and `(affects: …)`. Evidence, traces and narrative go in your handoff, not the board and not
  in messages to master. One message to master per milestone, not per step.
- **You may close Mark's desktop app and open the new build (family rule 17, Mark 2026-09-02):** when a
  task needs the new build running, close the app's OWN process (by PID + command line, graceful first,
  never a box-wide kill by name), `Start-Process` the installed/staged build, confirm it is up, note it in
  your handoff. Never file "blocked on Mark's open window".
- **Nothing external is ever posted for Mark (family rule 19, Mark 2026-09-03):** no worker and not master files or comments on any third-party GitHub issue/PR/discussion, forum, Discord, mailing list, support ticket or any other external channel under Mark's accounts or name. If something needs asking upstream, write the draft into `evidence/` and hand it to Mark on the board as a Mark-only item; he posts it himself if he judges it right. (Origin: iroh #4502, filed by a worker 2026-09-02 and closed by the maintainer with a complaint about model-written text.)
- **One session per lane (family rule 18, master 2026-09-03 after two VaultDrive sessions raced the same worktree):** before touching a lane's repo, run `ListAgents`; if another live session for the same lane exists (any name — registry tab or a hand-opened one), message it and STAND DOWN unless it hands over. Two sessions in one worktree overwrote each other's edits and handoffs today. Master launches at most one session per lane; a hand-opened second session yields to the registered one.
- **Read deny rules are GONE (Mark 2026-09-03): a permission prompt is now a bug.** Every kit's
  `permissions.deny` is empty and sessions run in bypass mode, so nothing should ever stop to ask.
  Still avoid `cd X && grep/cat` chains (absolute paths are clearer). If a prompt appears anyway,
  post its exact text in one board line and continue with the next row; master clears it.
- **Never park on a prompt; blocked posts name your next row (family rule 16, Mark 2026-09-02):** if a
  step hits a permission prompt or needs something you do not have, post the exact prompt/ask in ONE
  line and move to the next runnable row now — master does the step or tells you to skip it. Every
  "blocked on Mark" line ends with `→ now on: <next row>`. A cached login (Credential Manager), the
  wp-admin browser, and the installed app on this box are yours to use — they are never a Mark ask.
- **Blocked on Mark? Write the exact ask, then keep going (family rule 14, Mark 2026-09-02):** if a
  row needs something only Mark can give (a publish go-ahead, a ruling, a key not on this box, a
  physical step), put the ONE-LINE ask on the row's status — what, why it gates, what unlocks when he
  does it — and move to your next runnable item. Master batches those asks to Mark. Check
  Credential Manager / `~/.vault-secrets` before calling a credential Mark-only.
- **Handoff:** maintain `../_coordination/handoffs/<this-project-folder-name>.md` as a concise
  "where we left off": current task, what's done, what's in progress, next steps, open questions,
  and any gotchas. OVERWRITE the file (never append). Update it after completing significant work,
  and always write a final version when the user says "wrap up" or indicates the session is ending.
<!-- happs-coordination-kit:end -->

