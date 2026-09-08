# Dara Oladapo

Small desktop tools, built because I wanted them on my own machine and then finished properly
so other people could use them too.

Everything here is free. Not free-with-an-upgrade-prompt, not "clean up to 500 MB and then pay" —
free. If a tool in this org does something useful, it does all of it in the version you can
download today. There is no paid tier to protect, which is why the feature templates ask what
problem you have rather than what you would pay for.

I'm a cloud and DevOps engineer in the UK. I also make videos and write about this stuff at
[daraoladapo.com](https://daraoladapo.com).

## The tools

| | What it does | Platform |
| --- | --- | --- |
| **[PC Cleaner](https://github.com/dara-oladapo/pc-cleaner)** | Junk cleanup, duplicate and large-file finder, startup manager. Nothing is deleted without telling you whether it can be recovered. | Windows, macOS |
| **[Power Helper](https://github.com/dara-oladapo/power-helper)** | Tray utility for Optimus laptops — hard-disables the dGPU on battery, honest battery time, power plans and refresh rate matched to the power source. | Windows |

Several more are still private while they take shape: a desktop dashboard, a Stream Deck plugin
set, a Logitech peripheral controller, a remote terminal, and the homelab that runs the whole
thing. They land here when they're worth someone else's time.

## How the tools are built

Broadly the same shape every time, so the CI, the templates and the release path can be shared
across all of them from this repository:

- **C# on .NET 10.** MAUI where the app has to run on Windows and macOS; WinUI-flavoured native
  Windows where it doesn't. The SDK version is pinned per repo in `global.json`.
- **A `Core` project that touches no OS API**, unit-tested on every platform, with the registry,
  Recycle Bin, `launchctl` and WMI calls quarantined behind interfaces. It is the part most
  likely to break differently per platform, so it's the part that gets tested everywhere.
- **A `DESIGN.md` and a clickable HTML prototype** before the XAML. The prototype uses the same
  tokens as the app, so it is the reference the built UI is checked against.
- **CI on every pull request**, on Ubuntu, Windows and macOS, with one aggregate check to require.
- **Releases from a `v*` tag**, packaged with [Velopack](https://velopack.io) so the apps can
  update themselves without a browser round-trip.

## Filing something

Bugs and feature requests are welcome on any repo, including for hardware I don't own — a
compatibility report for a laptop I can't test on is genuinely useful. Each repo has issue forms;
if what you have doesn't fit one, open a blank issue or a discussion.

Read [SECURITY.md](https://github.com/dara-oladapo/.github/blob/main/SECURITY.md) first if the
thing you found is a vulnerability rather than a bug.
