# Reporting a security problem

Please don't open a public issue for a vulnerability. Everything in this org is a desktop app that
runs on someone's own machine, several of them elevated, and a public issue is a working exploit
handed to anyone reading before there's a fix to install.

## How to report

**Preferred:** open a private security advisory on the affected repository —
*Security* → *Report a vulnerability*. It's a private thread with me, it keeps the whole
discussion attached to the repo, and it can issue a CVE and a published advisory at the end if
the finding warrants one.

If that isn't available on the repo you're looking at, email **daraoladapo@outlook.com** with the
repository name in the subject line.

Either way, useful things to include: what an attacker gets, the version and OS you found it on,
and the smallest set of steps that shows it. A proof of concept is welcome but not required —
don't sit on a report because you haven't finished weaponising it.

## What happens next

These are single-maintainer projects, so this is a statement of intent rather than a support
contract: I'll acknowledge within a week, tell you whether I agree it's a vulnerability and what
I plan to do, and credit you in the release notes and advisory unless you'd rather I didn't.

If I've gone quiet for two weeks, chase me — silence here is me being busy, not a decision.

## What counts

Worth reporting:

- Anything that gets code running as another user, or as admin/root, that shouldn't.
- A path that writes or deletes outside where the tool says it operates, or that can be steered
  there by a crafted filename, symlink, junction or config value.
- Anything that leaves a machine less secure than it found it — a permission loosened, a
  protection disabled, an elevated task left registered after uninstall.
- An update or installer path that can be made to fetch or run something it shouldn't.
- A secret, token or credential committed to any of these repos. Report it even if it looks
  expired or scoped to nothing.

Not really security, but still worth an ordinary issue:

- The app needing admin rights to do something and failing without them. That's usually a known
  gap, documented in the repo's README.
- A tool deleting or changing something it shouldn't as a plain bug rather than something an
  attacker can steer. File it as a bug — those forms are triaged first anyway.

## Supported versions

The latest release of each tool. These are small projects on a rolling release; there are no
maintained older branches to backport to, so the fix ships as a new version.
