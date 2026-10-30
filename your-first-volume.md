# Your First Volume

**Outcome: a small encrypted container
of your own, created for real, whose
password the next chapter will prove
against the box itself.**

The lab created its practice volume
with the console face, every choice
stated on the command line so the whole
act fits in one transcript
(`veracrypt-lab/t02-create-volume.txt`):

```bash
veracrypt --text --non-interactive --create vault.hc \
  --size 20971520 --volume-type normal \
  --encryption AES --hash SHA-512 --filesystem FAT \
  --password <the lab password> --pim 0 \
  --random-source /dev/urandom
```

In the graphical program the same act
is the **Create Volume Wizard**: choose
"Create an encrypted file container",
choose a standard volume, name the
file, pick the same options below, move
your mouse when it asks for randomness,
and press Format. The wizard is the
better teacher the first time; this
chapter explains the choices either
way.

## The choices, one by one

- **The file and its name.** Any name,
  any extension — or none. The lab used
  `vault.hc` (`.hc` is a common
  convention); nothing in the file
  identifies it as VeraCrypt. Pick a
  name and place that make sense to
  you, remembering chapter 3's point:
  the file's *existence* is not hidden,
  only its contents.
- **Size.** Fixed forever. The lab's
  20 MB is a toy for practice; a real
  container should be sized for its job
  with room to spare. There is a small
  overhead (the two header areas and
  filesystem slack), so the usable
  space is a little less than the file
  size.
- **Volume type: standard.** The other
  choice, hidden, is chapter 7 — and
  needs an outer volume made first,
  in a particular way.
- **Encryption algorithm: AES.** The
  default, and the one with hardware
  acceleration on nearly every modern
  processor (chapter 12). Serpent,
  Twofish, Camellia, Kuznyechik and the
  cascades are there for people with a
  reason; "AES is broken" is not a
  reason that exists.
- **Hash: SHA-512.** The PRF for the
  password derivation (chapter 2). The
  default, a sound choice; Argon2id is
  the newer memory-hard alternative
  chapter 8 discusses.
- **Filesystem: FAT** for a first,
  small volume — every operating system
  reads it, which matters for
  chapter 11's travelling. For larger
  containers you will use on Linux
  only, ext4 is the better citizen;
  Windows-only containers usually take
  NTFS or exFAT. FAT cannot hold a
  single file larger than 4 GB.
- **The password.** The lab's was
  `bramble-lantern-47` — published here,
  so the lab volume is a practice box
  anyone can open, and you should treat
  it that way. Yours should be long
  rather than clever: several unrelated
  words beat one mangled word, because
  the attacker of chapter 3 guesses
  offline and length is what costs them.
  VeraCrypt's own guidance asks for at
  least 20 characters; it warns — and
  refuses nothing — below that, down to
  its minimum. **Use a password you do
  not use anywhere else**: this one will
  be typed on possibly several machines.
- **PIM: left at the default.** `--pim 0`
  means "the standard amount of
  derivation work". Chapter 8 is about
  when to change it.
- **Randomness.** Before the master key
  can be made, VeraCrypt needs genuine
  randomness. The wizard collects it
  from your mouse movements; the console
  run took it from the system random
  source. This is not ceremony — a
  predictable master key would quietly
  undo every other choice on this page.

## What happens while it creates

The run took a few seconds. VeraCrypt
writes the header, generates the master
key, then fills the whole container
with encrypted random data — the
progress bar in the transcript,
crawling at a few MB/s on the lab's
shared machine — and formats the
filesystem inside. Then:

```text
The VeraCrypt volume has been successfully created.
```

That full random fill is why a fresh
volume is already its final size, and
why used and unused space look alike
from outside (chapter 2). Choosing
**quick format** in the wizard (or
`--quick` here) skips the fill for big
volumes: faster, but the unfilled
regions then contain whatever was on
the disk before — which can reveal
which parts of the volume are in use.
On a fresh container file it also skips
overwriting the file's previous
contents, if it had any. For anything
that matters, let it fill.

## Check what you have

The file is there, the right size, and
— with `file vault.hc` — reported as
plain "data": no signature, exactly as
chapter 1 promised. Whether the
password truly opens it is the next
chapter's experiment.
