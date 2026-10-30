# What VeraCrypt Is

**VeraCrypt** is a program that turns a
file, a partition, or a whole disk into a
locked box. Inside the box you keep
ordinary files. Outside, the box looks
like random noise — no filenames, no
folder structure, nothing that says what
it holds or even that it is a box at all.

You open the box by **mounting** it:
you give VeraCrypt the box's file, a
password, and a spare drive letter or
folder. VeraCrypt checks the password,
works out the box's key, and from then
on your computer sees a normal drive.
Files you read are decrypted as they
leave the box; files you write are
encrypted as they enter it. This is
called **on-the-fly encryption**, and it
means the box is never decrypted all at
once — only the pieces in use, only in
memory. When you **dismount**, the drive
disappears and the box is noise again.

Three properties follow from that
design, and they are the reason the
program exists:

- **The box is just a file** (in its
  most common form). You can copy it,
  back it up, put it on a USB stick or in
  a synced folder, and open it on
  another machine. Chapter 11 works
  through travelling with one.
- **The password is not stored
  anywhere.** There is no password
  database and no recovery key held by
  anyone else. VeraCrypt re-derives the
  key from your password every time, and
  if the password is wrong, the box
  simply does not open — chapter 3 is
  blunt about what that means for
  forgotten passwords.
- **Nothing about the box advertises
  itself.** A VeraCrypt container has no
  header you can recognise from outside,
  no signature, no magic bytes. Until it
  is decrypted it is indistinguishable
  from random data.

## Where it came from

VeraCrypt is the continuation of
**TrueCrypt**, for years the standard
free disk-encryption tool. TrueCrypt's
anonymous developers ended the project
abruptly in 2014, declaring it no longer
secure. Two independent security audits
then examined the TrueCrypt code and
found it broadly sound, with flaws worth
fixing rather than fatal. VeraCrypt,
maintained by IDRIX and lead developer
Mounir Idrassi, forked the code in 2013
and has developed it since: it fixed the
audit findings, greatly strengthened the
key derivation (chapter 2 explains the
**PIM**, a VeraCrypt addition), added
newer algorithms, and dropped some old
ones. Current VeraCrypt can still open
old VeraCrypt volumes, but — as this
guide's lab saw in its own error
messages — it no longer opens TrueCrypt
volumes; the format support was removed
in the 1.26 series.

VeraCrypt is free, open source (under
the Apache License 2.0 combined with the
TrueCrypt License 3.0), and runs on
Windows, macOS and Linux. This guide
works on Linux, using the program's
text interface so that every step can
be shown and checked; the Windows and
macOS programs present the same ideas
through windows and buttons, and the
chapter notes say where they differ.

## The two shapes of box

Everything in this guide is one of two
things:

- A **file container** (also called a
  file-hosted volume): one ordinary file
  on your disk, of a size you choose,
  that mounts as a drive. This is the
  form the lab builds, and the right
  starting point for almost everyone.
- A **partition or device volume**: a
  whole partition or disk encrypted in
  place. On Windows, VeraCrypt can also
  encrypt the **system partition** — the
  one Windows itself boots from — so the
  password is asked before Windows
  starts. System encryption is a
  Windows-only feature; Linux and macOS
  builds cannot do it, and this guide
  says so wherever the topic comes up.
  Linux has its own whole-disk tool
  (LUKS, named in chapter 14); VeraCrypt
  on Linux encrypts containers and
  non-system partitions.

## What the lab did

The lab behind this guide ran the real
thing: **VeraCrypt 1.26.30**, the
official console build for Ubuntu
24.04, downloaded from the project's
own release page, its PGP signature and
its SHA-256 sum both verified before
anything ran (chapter 4 shows the whole
chain). Against it the lab created
containers, changed their passwords and
PIMs, made one answer to a keyfile as
well as a password, built a hidden
volume inside an ordinary one, backed
up a volume header, destroyed both
headers of a copy, and restored them
from the backup.

What the lab's machine could not do is
mount: it is a shared sandbox with no
disk devices at all — no loop device,
no device mapper, no FUSE device — so
every mount in the lab ends at the
exact step where a real machine would
present the drive. The lab turned that
into a measuring tool instead. A wrong
password and a right password fail
*differently*: the wrong one is refused
by the box itself, the right one is
accepted and only then stopped by the
missing device. That difference, shown
in chapter 6, is how the lab proves its
passwords, PIMs and keyfiles are right
without ever seeing a mounted drive.
Where a step genuinely needs a mounted
drive, the chapter teaches it from the
documentation and labels it as such.
`criteria.md` keeps the full ledger.
