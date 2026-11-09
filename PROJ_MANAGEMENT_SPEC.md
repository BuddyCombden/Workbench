# proj_management — Format Spec (v1)

This is the standard that lets any project be tracked by **Workbench** (`Workbench.html`).
Give this file (or the `proj-management-testing` skill) to the agent working on a project.

## Folder layout

```
<project root>/
  proj_management/
    FEATURES.md      shared: agent + human (via the tool)
    TESTS.md         agent writes, tool reads
    FEEDBACK.md      tool writes, agent reads (never edit)
    CHANGES.md       the human's requests (changes, bugs, questions, idea/research requests); agent replies
    DECISIONS.md     agent asks, human answers in the app
    IDEAS.md         the human's not-yet-committed ideas (shared)
    attachments/     screenshots referenced from FEEDBACK.md
    history/         Workbench's backups and activity.json (the History tab). Leave it alone.
    AGENT_SETUP.md   (temporary) one-time setup task for the agent, see below
```

The **project folder is always the parent of `proj_management/`**: that's where the project's code and docs live, and where Workbench's "Open folder" goes.

Every file starts with an `<!-- PROJ-MGMT:... -->` comment telling agents how to use it. Keep that comment at the top. If the folder doesn't exist yet, create it. The tool also creates it when the human adds the project.

> **Never overwrite a Workbench file.** Workbench creates all of these files as soon as a project is added, and the human may already have written in them. This applies even if you have never seen a file before.
> - **Check before creating.** Before writing any file in `proj_management/`, check whether it already exists.
> - **If it exists, edit it in place.** Read it and change only the lines you mean to change. Never replace it with a fresh copy.
> - **Create a file only if it is truly missing.** Start it with the header comment shown in this spec.
>
> When an outside edit removes entries, Workbench saves the previous version to `proj_management/history/` and shows a warning. That is a safety net, not permission.

## When a project is added

Workbench creates `proj_management/` with TESTS.md, FEATURES.md and FEEDBACK.md automatically, both for brand-new projects and for folders that already exist.

For a folder that already existed, Workbench also:
- scans the project root and any `docs/` folder for older test, planning and notes files;
- offers to import a checklist-style test list (`## <version>` headings with `- [ ] item` / `- [x] item` lines and optional `Feedback:` sub-lines) into TESTS.md. Ticked items become PASS results and Feedback lines become notes. The original file is never changed.
- writes `AGENT_SETUP.md`, which lists the docs it found.

**Agent:** when you see `AGENT_SETUP.md`, do its steps (move the existing plans into FEATURES.md, tidy the imported tests, point your own notes at `proj_management/`), then delete it. Workbench shows "waiting for agent setup" until the file is gone.

---

## FEATURES.md (shared)

```markdown
<!-- PROJ-MGMT:FEATURES v1 ... -->
# Features — Valheim Glider Tweaks

Project: Valheim Glider Tweaks
Status: Active
Summary: Creature-wing gliders with unique flight models.

## Flight
Optional area description.

### F-FLIGHT-01 · Creature flight models [Iterating]
Description lines.
- [Completed] Base stats per creature
- [Iterating] Hover behaviour

### F-FLIGHT-02 · Attitude indicator [Testing]

## Audio
### F-AUDIO-01 · Wind whoosh loop [Testing]
Source: user
```

| Element | Rule |
|---|---|
| `Status:` | One of Idea, Active, Paused, Maintenance, Completed or Archived. **The human owns it.** |
| `Color:` | Optional `#RRGGBB`. The human sets it in the app to give the project its own accent color. Leave it alone. |
| `Icon:` | Optional path to the project's icon image, relative to the project folder (e.g. `Assets/icon.png`). Set by the human in the app. If it's missing, Workbench falls back to `proj_management/icon.png`. |
| `## <Area>` | An area or subsection (one mod in a pack, one module). Optional: features placed above the first `##` belong to the whole project. |
| `### <ID> · <Title> [Stage]` | One feature. Use IDs like `F-AREA-01`. They are unique and never reused. |
| Stage | Ideated 10% · Designed 25% · Testing 50% · Iterating 75% · Completed 100% |
| `- [Stage] text` | A part of the feature. When parts exist, the feature's completeness is the average of its parts. |
| `Source: user` | The human requested it. Treat it as a request: design it, plan it, and advance its stage. |

**Moving through the stages:** Designed once it's specced. Testing once a test batch covers it. Iterating while you fix feedback. The human normally sets Completed.

The tool rewrites this file in this exact layout when the human edits it, so keep to it.

---

## TESTS.md (agent writes)

```markdown
<!-- PROJ-MGMT:TESTS v1 ... -->
# Tests — Valheim Glider Tweaks

## Batch 1.30.0
Date: 2026-10-30
Free text: setup for the whole batch.

### VGT-101 · Whoosh stays on near fires
Area: Audio
Feature: F-AUDIO-01
Priority: must
Setup: Night, near a base with torches
Steps:
1. Glide for a minute near torches/fires
Expected: Wind sound stays on with no 1–2 s dropouts.
Change: a1b2c3d (audio source pooling)
Retests: VGT-090
```

Rules:

- Add one `## Batch <version>` per build you hand over. **Put the newest batch first.** Never delete old batches, because they hold the result history.
- Test IDs use the form `PREFIX-NNN` in capitals. They are **unique across the whole file and never reused or renumbered**, because results are keyed by ID.
- Only `Expected` is really needed; every other field is optional. Lines that aren't fields become the test's description. A `Log: ...` line is shown as a description line.
- `Area` must match an area name in FEATURES.md. Leave it out if the project has no areas.
- `Feature` links the test to a feature ID. The tool shows linked test pass counts on each feature.
- **Verifying a fix:** don't edit the failed test. Add a **new** test in the new batch with `Retests: <old ID>`. The old issue shows as "fix pending" until the new test passes, then it's "Resolved".
- If you edit the title, setup, steps or expected text of a test that already has a result, the tool flags it as **needs retest**.

---

## DECISIONS.md (agent asks, human answers)

Use this for anything the human needs to choose. Include your research so they can decide without asking you.

```markdown
<!-- PROJ-MGMT:DECISIONS v1 ... -->
# Decisions — Valheim Glider Tweaks

## D-001 · How should stamina drain while hovering? [Open]
Raised: 2026-11-04
Area: Flight
Feature: F-FLIGHT-01

Context, research, trade-offs, links and tables, all in Markdown.

Options:
- A: Flat drain of 5/s for every glider
- B: Per-glider drain tuned to the table above
Recommendation: B - keeps each glider distinct
```

- Status is one of Open, Decided, Deferred or Dropped. New decisions start as Open.
- IDs look like `D-001`. They are unique and never reused.
- Workbench writes the human's answer back into the same decision, in this canonical layout:
  ```
  Decision: B - Per-glider drain tuned to the table above
  Decided: 2026-11-05
  Notes:
  > cap Seeker at 25 s
  ```
- Act on Decided items: update FEATURES.md, your plans and the code. Never change the human's answer.
- Follow-up questions become new decisions.

---

## IDEAS.md (shared: ideas not committed to yet)

This is a parking spot for ideas. The human writes them in the app, and you add ideas only when asked to brainstorm.

```markdown
<!-- PROJ-MGMT:IDEAS v1 ... -->
# Ideas — Valheim Glider Tweaks

## I-004 · Seasonal events [Ready]
Added: 2026-11-06
By: you
Area: Flight

Rough notes in Markdown.
```

- Status is one of New, Ready, Shelved, Promoted or Dismissed (older files may say Parked; it means Shelved). Workbench adds an `Updated:` line when the human edits an idea. IDs look like `I-001` and are never reused. `By:` is `you` or `agent`.
- When you brainstorm because the human asked, add ideas with `By: agent` and `[New]`.
- Don't act on ideas on your own. When an idea is **[Ready]**, or the human asks, turn it into a feature in FEATURES.md (stage Ideated, `Source: user`). If a choice has to be made first, turn it into a decision in DECISIONS.md instead.
- After turning an idea into a feature or decision, set it to `[Promoted]` and add `Became: <new ID>`.
- Never delete ideas.

---

## CHANGES.md (the human asks, the agent replies)

The human writes requests in Workbench's **Changes** tab. Each one is an entry:

```markdown
<!-- PROJ-MGMT:CHANGES v1 ... -->
# Changes — Valheim Glider Tweaks

## GI-003 · Why does stamina drain while hovering? [Answered]
Kind: Question
Added: 2026-11-08 10:12
Updated: 2026-11-08 11:40

The human's notes. Never edit them.

Reply:
> Your answer, each line starting with "> ".
```

- Status: Open, In progress, Answered, Done or Won't do. Kind: Change, Bug, Question, Idea request, Research or Other. `Severity:` is only used for bugs.
- **Changes and bugs:** set In progress while you work. When it ships, add a test with `Retests: GI-003` in TESTS.md. Workbench marks the entry Done when that test passes.
- **Questions, research and idea requests:** answer in a `Reply:` block at the end of the entry and set the status to Answered. For idea requests, add the ideas to IDEAS.md (`By: agent`) and list their IDs in the reply.
- If the human has to choose something, raise it in DECISIONS.md and mention the D-ID in the reply.
- IDs are unique and never reused. Add entries yourself only when asked. Workbench merges your edits with the human's, field by field.

---

## FEEDBACK.md (tool writes, read-only for agents)

Sections, in order:

1. **Summary**: a per-batch table of Pass / Partial / Fail / Blocked / Skipped / Needs retest / Untested.
2. **Open issues**: unresolved FAIL and PARTIAL results, sorted by severity (Critical > Major > Minor > Cosmetic). Each has the tester's notes as a quote and attachment links (`attachments/<file>`, relative to FEEDBACK.md). A "Fix pending" line appears when a `Retests:` test exists.
3. **Changes**: a short summary of the open entries in CHANGES.md (`GI-001`…). Read the full entries, and reply, in CHANGES.md.
4. **Needs retest**: tests the tester flagged, or tests whose text changed after they were tested.
5. **Blocked / skipped**: with the reason the tester gave.
6. **Feature requests**: features with `Source: user`.
7. **Results by batch**: every recorded result, including passes with notes.
8. A `<!-- PROJ-MGMT-DATA ... -->` JSON block. This is the tool's storage. Never edit it.

## Workspace notebook (`<workspace>/notebook/*.md`)

Workbench's **Notebook** keeps one Markdown file per topic in the workspace folder (not in a project). Agents may read these, and add tasks, links or notes **only when asked**, editing the file in place.

```markdown
<!-- PROJ-MGMT:NOTEBOOK v1 ... -->
# Mod release checklist

Status: Open            (Open | Waiting | Done)
Tags: valheim, release
Created: 2026-11-07
Updated: 2026-11-07

## Tasks
### Before upload
- [ ] Write changelog
- [x] Bump version
- [2/10] Screenshots taken      (a counter: current/target)

## Links
- [Thunderstore page](https://thunderstore.io/...)
- [Glider Tweaks › VGT-101](#project/<id>/testing/batches?hl=t-VGT-101)

## Notes
Free Markdown.
```

`### Name` lines under Tasks group the tasks. Images live in `notebook/attachments/` and are listed on an `Attachments:` line written by Workbench.

A project's icon comes from the `Icon:` path in FEATURES.md (relative to the project folder, so a mod can use its real icon file), falling back to a square `proj_management/icon.png` (or `.webp`, `.jpg`, `.svg`). Workbench shows it on the project card, header and tab.

---

## Agent workflow (each iteration)

1. Read `FEEDBACK.md` and `CHANGES.md`. Start with Open issues and open Changes (Critical first), answer questions and research/idea requests, then Needs retest, then Feature requests.
2. Read `DECISIONS.md`. Act on newly Decided items, and add new decisions for anything you need the human to choose.
   Check `IDEAS.md` for ideas marked [Ready] and promote them.
3. Read `FEATURES.md`. Plan any `Source: user` features and update their stages.
4. Make the changes.
5. Add a new `## Batch <version>` at the top of `TESTS.md`:
   - a `Retests:` test for each fixed issue;
   - tests for new or changed features, linked with `Feature:`.
6. Advance feature stages in `FEATURES.md`.
7. Tell the human the batch is ready. The tool picks it up automatically.
