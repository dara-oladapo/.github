# Contributing

Org-wide defaults. A repo with its own `CONTRIBUTING.md` overrides this; where the two disagree,
the repo wins, because it knows what its own build needs.

These are small tools with one maintainer. That shapes most of what follows: the aim is that a
pull request either lands quickly or gets a clear answer about why it won't, and that neither of
us spends a week on something that was never going to be merged.

## Before you write code

For anything beyond a typo or an obvious one-line fix, **open an issue first**. Not bureaucracy —
several of these tools have deliberate non-features (no paid tier, no telemetry, no "clean more
by upgrading", no Linux build where the UI framework has no Linux target), and it's a bad
afternoon for both of us if you find that out in review.

If an issue already exists, say on it that you're picking it up.

## The shape of the repos

Broadly the same everywhere, so once you've found your way around one you've found your way
around all of them:

```
src/<Name>.Core/          Platform-agnostic logic. No OS APIs. This is what gets unit-tested.
src/<Name>.App/           The UI. MAUI or native Windows depending on the repo.
  Platforms/<OS>/         Registry, Recycle Bin, launchctl, WMI, P/Invoke — everything OS-specific,
                          behind an interface from Core and wired up in DI.
tests/<Name>.Core.Tests/  xUnit, against Core.
docs/prototype/           A clickable HTML prototype of every screen, using the app's real tokens.
DESIGN.md                 The design system. Read it before touching XAML.
global.json               Pins the .NET SDK. Match it rather than bumping it in a feature PR.
```

The dividing line is the one worth internalising: **anything that could in principle be tested on
any OS belongs in `Core`**, and anything that calls into the operating system goes under
`Platforms/` behind an interface. It isn't tidiness — it's what makes the logic testable at all,
because CI can run `Core` on all three platforms and cannot meaningfully run the rest anywhere.

## Local setup

Each repo's README has its exact commands. In general:

- .NET 10 SDK, at the version in `global.json`.
- MAUI repos need workloads: `dotnet workload install maui-windows` on Windows,
  `maui-maccatalyst` on a Mac with Xcode.
- Build the app project with an explicit `-f <target-framework>`. A plain solution build tries
  every target, including ones your machine has no toolchain for, and the failure looks like a
  code problem when it isn't.

## Branches and commits

Branch off `main`, one topic per branch. Present-tense commit subjects that say what changed and
why (`Restore the user's brightness on AC instead of a fixed default`), not `fix bug`. History
gets squashed on merge, so the PR title is what survives — make that one good.

Don't commit binaries, installers, or anything from `bin/`/`obj/`.

## Pull requests

Fill in the template. The one section people skip is the one that matters most: **say what you
actually ran**, not what you meant to run. "It builds" and "I ran it on a real laptop and watched
it work" are different claims and reviewers can't tell them apart from the diff.

What gets checked in review:

- **Does it undo itself?** These tools change real machines — deleting files, disabling devices,
  switching display modes, writing startup entries. Anything the app turns off, it turns back on
  when it exits. Anything it deletes is either regenerable or goes to the Recycle Bin. A change
  that quietly breaks that is the most expensive kind of bug here.
- **Does the UI follow `DESIGN.md`?** Tokens rather than literal colours and sizes, one accent,
  danger styling reserved for destructive actions, real empty states, honest progress. If the
  repo has a prototype under `docs/prototype/`, the built UI is expected to match it.
- **Is the platform-specific code behind an interface?** A P/Invoke in `Core` will be sent back.
- **Do the two surfaces stay in step?** Where a tool has both a tray menu and a settings window,
  they're two views of one settings object, and it's easy to update one and forget the other.
- **CI green.** Ubuntu, Windows and macOS. It builds XAML as C#, so a mistyped resource key or a
  bad binding path is a compile error there — which is exactly the kind of thing human review
  misses.

Warnings are errors in CI in most repos. If a new SDK introduces an unrelated warning, suppress
that specific code with a comment saying why, rather than dropping `-warnaserror` — a gate that's
been switched off stops catching the real ones.

## Reviews and merging

Most repos run a `pr-guardian` workflow that comments on the PR with exactly what's blocking it
and squash-merges once nothing is. If it says it's holding, the table says why. It will not merge
a PR that no CI has run on.

Fork PRs are reviewed and merged by hand instead — the guardian skips them deliberately, because
it would otherwise be running against a branch that could rewrite it.

## Things that will be declined

Not to be discouraging — better to say it here than in review:

- Adding a paid tier, a nag, telemetry, analytics, or bundled anything.
- Broadening a tool's scope well past its README. Small is a feature.
- A dependency that could be twenty lines of `System.IO`.
- Reformatting a file wholesale alongside a real change, which buries the change.
- Dropping or weakening a CI gate to get a build green.
