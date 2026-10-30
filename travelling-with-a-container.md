# Travelling with a Container

**Outcome: your container opens on
another machine — another operating
system included — and survives the
journey backed up rather than stranded.**

A file container is the most portable
form of encrypted storage there is: it
is one file, copying it copies
everything, and VeraCrypt on Windows,
macOS and Linux all read the same
format. A container made on Linux opens
on a Windows laptop with nothing
converted. (The reverse journey is
equally true.) This chapter is the
checklist that keeps that true in
practice.

## Choose the filesystem for the journey

The container's *inside* filesystem is
the compatibility question:

- **FAT**: readable everywhere, but no
  file inside may exceed 4 GB, and it
  has no journaling — pull the plug at
  the wrong moment and the filesystem
  may need repair.
- **exFAT**: readable on Windows and
  macOS out of the box and on modern
  Linux, no 4 GB limit. The usual choice
  for a container that will cross
  systems — with the same no-journaling
  caveat.
- **NTFS** travels well to Windows,
  awkwardly elsewhere; **ext4** is
  at home only on Linux.

Make the choice at creation (chapter
5); changing it later means a new
container and a copy across while both
are mounted.

## Carry the program, not just the box

A borrowed or fresh machine has no
VeraCrypt on it. Two habits cover that:

- Keep the installers for your platforms
  beside the container (on the same USB
  stick, *outside* the volume, obviously),
  verified the chapter 4 way when you
  download them.
- Windows offers a **traveler mode**
  (`VeraCrypt Portable`): the program
  runs from the stick without installing.
  It still needs administrator rights on
  the machine, and it is documented-only
  for this guide — the lab's platform
  has no equivalent need, since Linux
  and macOS ask for the same privileges
  at mount time either way.

And the password, PIM and keyfiles
travel in your head and your pocket —
never in a file beside the container.
If the container is on a USB stick, the
keyfile (chapter 9) belongs on a
*different* stick or on the destination
machine, or the two-factor story
collapses into one lost bag.

## Backups: three different things

Travellers conflate them; keep them
separate:

1. **A copy of the container file.**
   Your normal backup tool copying the
   file is a complete backup of
   everything, headers included — restore
   the file and the volume is back,
   password unchanged since the copy was
   made. Note the corollary for synced
   folders (chapter 13): a copy synced
   *while the volume is mounted and
   being written* can be a torn,
   half-written volume. Sync and back up
   containers while they are dismounted.
2. **The header backup** (chapter 10).
   Small, separate, and the only cure
   for header damage. It belongs
   somewhere other than with the
   container — and it is itself
   sensitive: it holds the master key
   under the password of its day.
3. **Copies of keyfiles**, byte-for-byte
   (checksum them), if the volume uses
   any.

## On the road, briefly

- Dismount before the machine sleeps or
  the stick is pulled.
- Expect border-crossing questions to
  be a legal matter, not a technical
  one (chapters 3 and 7) — decide your
  answers before you are asked.
- Mobile phones: there is no official
  VeraCrypt app for phones; third-party
  apps exist that open VeraCrypt
  containers (on Android, EDS Lite is
  the long-standing name). Treat them as
  documented-only here: the lab did not
  run them, and handing your container
  password to any app deserves the same
  chapter 4 suspicion as any installer.
