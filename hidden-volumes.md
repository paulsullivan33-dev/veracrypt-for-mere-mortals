# Hidden Volumes and Plausible Deniability

**Outcome: you understand what a hidden
volume is, how one is built — the lab
built one — and, just as clearly, what
its protection does not cover.**

This is the chapter to read slowly.
Hidden volumes are VeraCrypt's most
famous feature and its most
oversold one.

## The idea

A **hidden volume** lives inside
another VeraCrypt volume — the
**outer volume** — in space the outer
volume's own bookkeeping treats as
unused. Recall chapter 2: a volume is
random-filled, and used and unused
regions look identical. The hidden
volume occupies what looks like some of
that unused randomness at the end of
the outer volume, with its own header
hidden in the outer header area, its
own password, PIM and keyfiles, its own
cipher if you wish.

One file, two doors:

- The **outer password** opens the outer
  volume: real files, which you keep
  plausible but expendable.
- The **hidden password** opens the
  hidden volume: the files you actually
  care about.

There is no third state and no marker.
Given only the file, an examiner cannot
tell whether a hidden volume exists at
all — the hidden header is encrypted
under the hidden password and the rest
is indistinguishable from the outer
volume's random fill. That is the whole
mechanism of **plausible deniability**:
if you are forced to surrender a
password, you surrender the outer one.
It opens. It contains real, mildly
private things. And there is no
evidence anything else is there.

## The lab built one

The console help text prints the
official procedure and warns that
inexperienced users should use the
graphical wizard; the lab followed the
procedure exactly
(`veracrypt-lab/t07-hidden-volume.txt`):

1. Create the outer volume with **no
   filesystem** and **without** quick
   format — a full random fill, nothing
   written inside it yet:

   ```bash
   veracrypt --text --non-interactive --create outer.hc \
     --size 31457280 --volume-type normal \
     --encryption AES --hash SHA-512 --filesystem none \
     --password <outer password> --random-source /dev/urandom
   ```

2. Create the hidden volume inside it —
   same file, `--volume-type hidden`,
   its own smaller size and password:

   ```bash
   veracrypt --text --non-interactive --create outer.hc \
     --volume-type hidden --size 10485760 \
     --encryption AES --hash SHA-512 --filesystem FAT \
     --password <hidden password> --random-source /dev/urandom
   ```

   ```text
   The hidden VeraCrypt volume has been successfully created
   and is ready for use. … it should be impossible to prove
   that the hidden volume exists, even when the outer
   volume is mounted.

   WARNING: IF YOU DO NOT PROTECT THE HIDDEN VOLUME … DO NOT
   WRITE TO THE OUTER VOLUME. OTHERWISE, YOU MAY OVERWRITE
   AND DAMAGE THE HIDDEN VOLUME!
   ```

3. The remaining steps — mount the outer
   volume **with hidden volume
   protection**, format it, fill it with
   the decoy files, dismount — need a
   mounted drive, so on the lab's
   deviceless machine they are
   documented-only (see below). The two
   passwords were still proven the
   chapter 6 way: each is *accepted* by
   the file, a junk password is refused
   (`veracrypt-lab/t11-hidden-volume-attempts.txt`).

## The danger the warning is about

The outer volume does not know the
hidden volume exists. Write enough to
the outer volume and its filesystem
will eventually write *over* the hidden
volume's home at the end of the file —
destroying it, silently.

**Hidden volume protection** is the
guard: when you mount the outer volume
and supply the hidden volume's
credentials as well (`--protect-hidden yes`
and the protection password/PIM/keyfiles
on the console; a checkbox in the GUI),
VeraCrypt computes where the hidden
volume lives and refuses any write that
would touch it. Rule of life: **never
write to an outer volume without
protection enabled.** Reading without
protection is safe; the one destructive
act is an unprotected write.

## What deniability does not cover

Honesty, as promised:

- **A container file alone gives almost
  no deniability.** A lone file of pure
  randomness begs the question of what
  it is. The plausible story for random
  data is stronger for a wiped
  *partition* than for a file; the
  hidden volume is what gives a
  container its answer to "open it" —
  and an examiner who knows VeraCrypt
  also knows hidden volumes exist.
  Deniability raises the cost of
  *proving*; it does not make suspicion
  impossible.
- **The outer volume must be believable
  and used.** A decoy created yesterday,
  never modified, containing three stock
  documents, fools nobody. The
  documentation's own security
  requirements for hidden volumes run
  to pages — timestamps, filesystem
  traces, and the behaviour of the
  operating system all leak. This guide
  cannot make you an expert in them;
  read the project's *Security
  Requirements for Hidden Volumes*
  before relying on this feature with
  anything real at stake.
- **Version matters.** The 1.26.30
  release notes warn that hidden
  volumes inside file containers
  created by VeraCrypt 1.26.6 through
  1.26.28 carry a plausible-deniability
  weakness fixed in 1.26.29 — if you
  rely on deniability and built yours
  with one of those versions, recreate
  outer and hidden volumes on a current
  build and securely erase the old
  container. The lab's volumes were
  made with 1.26.30, after the fix.
- **Deniability is not a legal
  strategy.** In some jurisdictions
  refusing or "failing" to produce a
  second password is itself an offence;
  in others the outer password ends the
  matter. Which world you live in is a
  question for a lawyer, not a manual
  (chapter 3).
- **Backups multiply doors.** Every
  header backup of the outer file holds
  the hidden header too (chapter 10's
  backup transcript says so, in the
  program's own words) — a backup made
  carelessly is a second copy of the
  secret's shape, if not of the secret.

Used with open eyes, a hidden volume is
a genuinely strong answer to a narrow
question: *coerced disclosure of a
password*. It is not invisibility, and
this guide would rather lose the magic
than oversell it.
