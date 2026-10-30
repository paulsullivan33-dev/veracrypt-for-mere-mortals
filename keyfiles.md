# Keyfiles

**Outcome: a volume that needs a file
as well as a password, and a clear head
about what that does and doesn't buy.**

A **keyfile** is an ordinary file whose
contents VeraCrypt mixes into the key
derivation along with your password.
From then on, mounting needs both: the
password you know *and* the file you
have. It is the software version of a
key you carry separately from the
combination.

## What a keyfile buys

Per the project's own documentation:

- **Brute-force resistance.** Even a
  mediocre password becomes hard to
  attack offline when the attacker also
  needs a file they don't have.
- **A physical factor.** Keep the
  keyfile on a USB stick and the volume
  opens only when the stick is present.
  VeraCrypt also supports keyfiles held
  on security tokens and smart cards
  (documented-only in this guide; the
  console help lists the token commands).
- **Shared access.** Several people can
  each hold a copy of the same keyfile
  with *different* personal passwords on
  their tokens — the volume cares about
  the combination, not the person.

And what it costs, in the documentation's
own capital letters: **if you lose a
keyfile, or any bit of its first
1,024 kilobytes changes, the volume can
never be mounted again.** VeraCrypt uses
up to the first megabyte of each keyfile;
edit the file — even opening and
re-saving it in a program that rewrites
it — and it is a different key. VeraCrypt
itself never modifies a keyfile.

## Making one

Any file works — a photo, a song, a text
file — which is itself a mild security
feature: a keyfile need not look like
one. But a file that can *change* is a
liability (a photo your tools
recompress, a document you might edit),
so a dedicated, untouched file is the
sane choice. VeraCrypt will generate
one: in the graphical program, Tools →
Keyfile Generator; on the console, the
lab ran (`veracrypt-lab/t06-keyfile-volume.txt`):

```bash
$ veracrypt --text --non-interactive --create-keyfile lab-keyfile \
    --random-source /dev/urandom
Keyfiles have been successfully created.
```

The result is 128 bytes of randomness
in a file with owner-only permissions.
Treat a generated keyfile like a
password you cannot memorise: back it
up (chapter 10), because it cannot be
re-derived from anything.

## A volume that requires it

The lab created its second container
with both factors:

```bash
veracrypt --text --non-interactive --create keyvault.hc \
  --size 20971520 --volume-type normal \
  --encryption AES --hash SHA-512 --filesystem FAT \
  --password <its password> --keyfiles lab-keyfile \
  --random-source /dev/urandom
```

```text
The VeraCrypt volume has been successfully created.
```

Mounting then needs the same pair:
`--password … --keyfiles lab-keyfile`.
Chapter 6's discrimination applies
unchanged: with the password but
*without* the keyfile, the volume
itself refuses the attempt; with both,
it is accepted and only the sandbox's
missing device stops it. The lab ran
both attempts against `keyvault.hc`;
they are in
`veracrypt-lab/t10-keyfile-mount-attempts.txt`.

## House rules for keyfiles

- **More than one is allowed.** Several
  keyfiles can be required together;
  their order doesn't matter.
- **Never store the only copy with the
  volume.** A keyfile in the same folder
  as the container is a second lock with
  the key taped to it.
- **Never "tidy" a keyfile.** No
  renaming worry — names don't matter —
  but no editing, re-saving, converting
  or "fixing" it, ever. Copy it
  byte-for-byte when you back it up,
  and check the copy's checksum against
  the original.
- **Changing keyfiles is a header
  operation** like changing the password
  (chapter 10): `--change` with
  `--new-keyfiles`. It re-wraps the same
  master key — and an old *external*
  header backup still pairs with the
  old keyfiles, so rotate your backups
  when you rotate factors.
- Keyfiles do not apply to Windows
  system encryption — VeraCrypt does not
  support them there.
