<!--
This is the org-wide default. A repo with its own .github/PULL_REQUEST_TEMPLATE.md overrides it
entirely, and most of the app repos do — theirs ask about the specific ways that app can hurt
someone (deleting the wrong file, leaving hardware disabled after exit). If you're adding a
template to a repo, start from this one and add those questions rather than dropping them.

Keep it short. Delete any section that doesn't apply rather than writing "N/A" in it.
The diff says what changed; this should say why, and what you actually checked.
-->

## What and why

<!-- What this changes, and the problem it solves. Link the issue if there is one: Fixes #123 -->

## How it was verified

<!--
Be specific about what you ran, not what you intended to run. "It builds" and "I watched it work
on a real machine" are different claims — say which one is true. These are desktop apps driving
OS APIs, so CI proves less here than it does in most projects.
-->

- [ ] The repo's build command
- [ ] The repo's tests
- [ ] Ran the app and exercised the change by hand
- [ ] Checked light and dark themes (any UI change)

Machine tested on: <!-- e.g. Windows 11 24H2 on a Legion 5 Pro / macOS 15.2 / not run — CI only -->

## Does this leave anything changed after the app exits?

<!--
Delete this section unless the change touches the user's system — files, registry, devices,
display modes, power settings, startup entries.

The rule across these tools is that they clean up after themselves: anything the app switches
off gets switched back on when it exits, and anything it deletes is either regenerable or goes
to the Recycle Bin. If this PR touches that, say what you did to convince yourself it holds.
-->

## UI changes

<!--
Delete this section if there are none. Otherwise:
- Does it follow the repo's DESIGN.md — tokens rather than literal values, one accent colour,
  danger styling only on destructive actions?
- Is there a designed empty state, or does the screen just go blank?
- If progress is shown, is it honest, or is it a fake percentage?
- Screenshots of both themes are worth more than a description.
-->

## Notes for the reviewer

<!-- Anything you're unsure about, deliberately left out, or want a second opinion on. -->
