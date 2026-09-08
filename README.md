# `.github`

Org-wide defaults for [`dara-oladapo`](https://github.com/dara-oladapo). Nothing here is an
application — it's the profile page, the fallback issue and PR templates, the community health
files, and the shared CI that the tool repos call.

## What's in here

```
profile/README.md                     The org profile page shown at github.com/dara-oladapo
.github/PULL_REQUEST_TEMPLATE.md      Default PR template
.github/ISSUE_TEMPLATE/               Default issue forms + config
.github/workflows/pr-guardian.yml     Reusable workflow (workflow_call), called by the tool repos
workflow-templates/                   Starter workflows offered when adding Actions to a repo
CONTRIBUTING.md  SECURITY.md  SUPPORT.md  CODE_OF_CONDUCT.md
```

## How each part reaches the other repos

They work in three different ways, which is worth knowing before wondering why an edit here
didn't show up somewhere:

**Community health files and templates are inherited, and a repo's own copy wins.** A repo with
its own `CONTRIBUTING.md` or `.github/PULL_REQUEST_TEMPLATE.md` ignores the one here entirely —
there is no merging. Most of the app repos override the PR template and the issue forms on
purpose, because theirs ask about the specific way that app can hurt someone. The files here are
the floor, not the ceiling.

Note the sharp edge on `ISSUE_TEMPLATE/config.yml`: a repo that adds its own config to point at
its own Discussions replaces this one wholesale, contact links included, so the security link has
to be carried across by hand.

**The reusable workflow is called, not inherited.** A repo gets `pr-guardian` by having its own
thin `.github/workflows/pr-guardian.yml` that does `uses: dara-oladapo/.github/.github/workflows/pr-guardian.yml@main`
— the triggers and `permissions:` have to be declared by the caller, so that file can't be
avoided. The starter workflow of the same name is exactly that caller, ready to commit. Because
this repo is public, private repos in the org can call it.

**Starter workflows are offered, once.** `workflow-templates/` shows up under *Actions → New
workflow* for repos in the org. They're a starting point that gets copied and edited — a later
change here does not reach a repo that already copied one.

## The private-repo caveat

Default community health files only reach **public** repos. Most of this org is private, so
`CONTRIBUTING.md`, `SECURITY.md`, `SUPPORT.md`, `CODE_OF_CONDUCT.md` and the templates above
currently apply to `pc-cleaner` and `power-helper` and nothing else.

To cover the private repos, GitHub wants a **private** repository named `.github-private` in the
same org, holding the same files. It's a second copy to keep in step, which is why it isn't done
yet rather than an oversight. The reusable workflow is unaffected — that reaches every repo
regardless of visibility.

Also worth knowing: the "Question or general discussion" contact link points at org-level
Discussions, which need turning on in org settings before it resolves.

## Starter workflows

| | For |
| --- | --- |
| `.NET desktop CI` | The three-platform build-and-test used by the MAUI apps, with one aggregate check to require in branch protection. Set the project paths at the top. |
| `.NET desktop release` | Tag-driven Velopack packaging and GitHub Release, including the manifest files the in-app updater reads back. |
| `PR Guardian` | The thin caller for the shared merge workflow. |

The CI and release templates carry the project paths as `env:` at the top with placeholder values.
They will not run until those are filled in — deliberately, since a template that silently builds
the wrong thing is worse than one that fails immediately.
