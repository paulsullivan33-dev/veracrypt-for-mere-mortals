# VeraCrypt for Mere Mortals

Some files should not be readable just
because someone is holding the drive.
**VeraCrypt** makes them unreadable: it
turns a file — or a partition — into an
encrypted **volume** that opens with a
password and mounts as an ordinary
drive. Inside, your files are files.
Outside, the volume is random noise
with no signature, no list of contents,
and no way in that does not go through
its password. No account, no cloud, no
recovery service — which is the point,
and also the danger this guide keeps
in plain sight.

This guide explains how a volume is
put together, how to install VeraCrypt
with the download *proven* genuine, and
how to create, open and maintain
containers of your own. It covers the
parts most guides rush: the **PIM**
that prices password guesses, **keyfiles**
that add a second factor, **hidden
volumes** and what their plausible
deniability really covers, and the
**header backup** that decides whether
a damaged volume is an inconvenience or
a funeral.

The lab behind this guide ran the real
thing: **VeraCrypt 1.26.30**, the
official console build for Ubuntu
24.04, its PGP signature verified
against the fingerprint the project
publishes and its SHA-256 sum checked
against the signed sums file. Against
it the lab created containers, changed
a password and a PIM and proved both
changes, made a volume answer to a
keyfile as well as a password, built a
hidden volume inside an ordinary one,
backed up a volume header, destroyed
both headers of a copy, and restored
them from the backup.

What the lab's machine could not do, it
did not fake. The lab is a shared
sandbox with no disk devices — no loop
device, no device mapper, no FUSE
device — so no mount could complete
there. Instead the lab proved every
credential by *where its failure
stops*: a wrong password, PIM or
keyfile is refused by the volume
itself, while a right one is accepted
and only then stopped by the missing
device. That two-signature pattern runs
through the whole guide as its
measuring instrument, and the steps
that genuinely need a mounted drive
are taught from the documentation and
labelled as such. `criteria.md` keeps
the full ledger of what was run and
what was only read.

## What this guide covers

- [**What VeraCrypt is**](what-veracrypt-is.md) —
  the mental model: a file that mounts
  as a drive, on-the-fly encryption,
  and the TrueCrypt lineage stated
  plainly
- [**The moving parts**](the-moving-parts.md) —
  container, header and its embedded
  backup, master key versus header key,
  ciphers, hashes, and the program's
  two faces
- [**What it protects, and what it doesn't**](threat-model.md) —
  theft, loss and snooping versus
  malware, mounted volumes, data leaks
  and forgotten passwords
- [**Installing VeraCrypt**](installing-veracrypt.md) —
  the official download and the full
  verification chain: fingerprint,
  signed sums, package signature
- [**Your first volume**](your-first-volume.md) —
  creating a container for real, and
  every choice in it explained
- [**Mounting and dismounting**](mounting-and-dismounting.md) —
  the daily ritual, favourites and
  automount, and how to tell a wrong
  password from a right one
- [**Hidden volumes and plausible deniability**](hidden-volumes.md) —
  one file with two doors, built live,
  with the danger to the hidden volume
  and the limits of deniability stated
  honestly
- [**PIM — the unlock-cost knob**](pim.md) —
  the iterations arithmetic, Argon2id,
  the PIM as a second secret, and
  choosing a value
- [**Keyfiles**](keyfiles.md) —
  a file as a second factor: what it
  buys, what its loss costs, and the
  house rules
- [**Changing passwords and header backups**](changing-passwords-and-header-backups.md) —
  changing password, PIM and keyfiles,
  backing up the header, and the
  destroy-and-restore round trip
- [**Travelling with a container**](travelling-with-a-container.md) —
  filesystems that cross platforms,
  carrying the program, and the three
  different things called backup
- [**Performance and hardware acceleration**](performance.md) —
  AES in hardware, what cascades cost,
  and the program's own speed test
- [**Recipes**](recipes.md) — a records
  vault, a loss-proof USB stick, two
  people one volume, and retiring a
  volume properly
- [**The neighbours**](the-neighbours.md) —
  LUKS, BitLocker and FileVault for
  whole disks; age, GnuPG and
  Cryptomator for files; and why
  password managers are a different
  species
- [**Limits**](limits.md) — Windows-only
  system encryption, no recovery ever,
  fixed sizes, an unchanging master
  key, and no integrity for the data
  area
- [**Troubleshooting**](troubleshooting.md) —
  the all-causes error message, headers
  damaged and restored, the FUSE error
  the lab met, and the slow failure of
  a wrong password
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Encrypting the disk a Linux or macOS
machine boots from — VeraCrypt cannot
do it, and chapter 14 points at the
tools that can. Covert or deniable
operations beyond the documented
mechanics of hidden volumes: chapter 7
explains the feature and its limits
and stops there, deliberately. Legal
advice about compelled disclosure —
chapters 3 and 7 flag where the
question bites, and a lawyer outranks
a manual. Exotic key storage (security
tokens and smart cards) is described
from the documentation, not run.

## How to use this guide

Read chapters 1 to 3 before creating
anything — the model, the parts, and
the honest threat picture are what
make the rest safe to follow. Chapters
4 and 5 get you a verified install and
a practice volume in one sitting.
Chapter 6's two failure signatures are
worth learning by heart: they are how
you will always know whether a
credential problem is yours or the
machine's. The lab's transcripts live
in `veracrypt-lab/`; every claim the
chapters make about the program's
behaviour names the transcript that
proves it.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [KeePassXC for Mere Mortals](https://paulsullivan33-dev.github.io/keepassxc-for-mere-mortals/)
- [age for Mere Mortals](https://paulsullivan33-dev.github.io/age-for-mere-mortals/)
- [Backups for Mere Mortals](https://paulsullivan33-dev.github.io/backups-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/veracrypt-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/veracrypt-for-mere-mortals/blob/main/LICENSE-CODE).
