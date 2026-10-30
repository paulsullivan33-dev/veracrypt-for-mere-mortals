# What It Protects, and What It Doesn't

Encryption tools fail their users most
often through a wrong mental model, not
wrong mathematics. This chapter states
plainly what a VeraCrypt volume does,
what it does not do, and where the
responsibility stays yours. Read it
before you trust a volume with anything
that matters.

## What it protects against

A dismounted VeraCrypt volume protects
its contents against anyone who gets
the storage but not the password:

- **A stolen or lost machine or drive.**
  The container is random noise; a
  laptop thief gets the file, not the
  files.
- **A drive sent for repair, sold, or
  thrown away.** Whatever you kept only
  inside volumes does not go with it.
- **Curiosity and casual access.** A
  borrowed machine, a shared family
  computer, a colleague at your desk:
  without the password there is nothing
  to browse.
- **Seizure or copying of the storage
  itself.** A forensic image of the
  disk contains the volume as noise.
  Chapter 7 takes up the special case
  where you may be forced to give up a
  password, and is honest about how far
  VeraCrypt's answer to that goes.

The strength of all four is the
strength of the password and the key
derivation together: an attacker with
the file can guess passwords offline,
at their leisure, on their hardware.
The slow derivation (chapter 8) raises
the price of each guess; it cannot make
a weak password a strong one.

## What it does not protect against

- **Anything, while the volume is
  mounted.** A mounted volume is a
  normal drive. Any program running as
  you — including malware — can read
  it. VeraCrypt guards the box at rest,
  not the room while the box is open.
  Dismount volumes you are not using.
- **A compromised machine.** A keylogger
  or screen recorder on the computer
  where you type the password takes the
  password, and with it everything.
  Encryption cannot fix an untrustworthy
  computer.
- **Copies outside the box.** Files you
  open from a volume are routinely
  copied elsewhere by the system and by
  applications: temporary files, recent
  document lists, thumbnails, search
  indexes, swap and hibernation files
  can all hold pieces of what you
  opened. VeraCrypt's own documentation
  lists these data leaks in detail;
  whole-disk encryption is the blunt
  answer, and it is a different tool's
  job (on Windows VeraCrypt itself can
  encrypt the system drive; on Linux
  that job belongs to LUKS).
- **A forgotten password.** There is no
  reset, no recovery service, no back
  door — that is the design working as
  intended. A forgotten password, PIM
  or lost keyfile means the data is
  gone, by mathematics rather than by
  policy. Chapters 8–10 are about
  managing that risk without weakening
  the protection.
- **Damage to the header without a
  backup.** The data can be perfectly
  intact and perfectly unreachable if
  both headers and your external header
  backup are lost. Chapter 10.
- **Being made to reveal that a volume
  exists.** A container's contents are
  hidden, but a 2 GB file of pure
  randomness on your disk is itself a
  fact an examiner can point at.
  Chapter 7 explains what hidden
  volumes change about that, and what
  they do not.

## The ethics line, plainly

VeraCrypt is a privacy tool: it keeps
your own information yours on your own
machines. That is this guide's subject
— protecting your tax records, your
journals, your family documents, your
work files, against theft and loss.
The same mathematics can of course be
used to hide wrongdoing, and nothing in
this guide is written for that purpose;
the techniques are taught for defence
of your own data, in the open, the same
way lock-making is taught. If the laws
where you live restrict encryption or
compel password disclosure, that is a
legal question this guide cannot answer
for you — chapter 7 flags where it
bites, and a lawyer outranks a manual.

## A sensible default posture

For most readers the pattern that gets
the benefit without the drama is:
containers for the files that need them,
a long password you can actually
remember (chapter 5), the container
copied into your normal backups
(chapter 11), a header backup stored
separately (chapter 10), and volumes
dismounted when not in use. Everything
after this chapter is how to do those
things well.
