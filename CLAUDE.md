# Workbench: notes for Claude sessions

Workbench is a single-file app (`Workbench.html`) for tracking testing, feedback, features and ideas across the user's projects. It runs in Chrome or Edge and uses the File System Access API; there is no server or build step.

## Testing and features are tracked in Workbench itself
`proj_management/` in this folder follows `PROJ_MANAGEMENT_SPEC.md`. The *proj-management-testing* skill covers the same rules.
- `FEATURES.md`: areas and features with stages. Shared with the user, who owns `Status:` and usually sets Completed.
- `TESTS.md`: add one `## Batch <version>` per release, newest first. Test IDs are `WB-NNN` and are never reused.
- `FEEDBACK.md`: the user's test results. Read it first, never edit it.
- `CHANGES.md`: the user's requests (GI-NNN): changes, bugs, questions, idea and research requests. Reply in a `Reply:` block and update the status, as the file's header explains.
- `DECISIONS.md`: put choices the user needs to make here (D-NNN), with research, options and a recommendation; act on the ones they've Decided.
- `IDEAS.md`: the user's not-yet-committed ideas (I-NNN). Only promote [Ready] ones into features or decisions, then mark them [Promoted] with `Became:`.

## Release checklist
1. Bump `APP_VERSION` in Workbench.html. It shows in the Settings footer.
2. Add a test batch for that version in `proj_management/TESTS.md`, using `Retests:` for fixed issues and `Feature:` links for new work.
3. Move feature stages forward in `proj_management/FEATURES.md`.
4. If the app is hosted (GitHub Pages), publish the new version there as well.

## Code layout (Workbench.html)
- CSS: theme tokens at the top, then palettes, typography and Shoelace styles.
- JS sections:
  - format parsers and serializers (FEATURES, TESTS, FEEDBACK)
  - file helpers and IndexedDB
  - onboarding
  - rendering: home, ideas, project, overview, testing, features
  - actions (`A`) and change handlers (`CHG`)
  - polling, demo file system, boot
- Add `?demo` to the URL to test with in-memory sample data, for example with Playwright.
