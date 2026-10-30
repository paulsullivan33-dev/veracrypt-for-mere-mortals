# Changing Passwords and Header Backups

**Outcome: you can change a volume's
password, PIM and keyfiles — and you
can bring a volume back from destroyed
headers, which the lab did for real:
corrupted a copy past recognition,
then restored it from a 128 KB file.**

Recall the one fact that runs this
whole chapter (chapter 2): a password
change does not re-encrypt your data.
It decrypts the **master key** out of
the header with the old header key and
re-wraps it under a new header key
built from the new password, PIM and
keyfiles. Milliseconds of work, no data
touched — and consequences in both
directions, good and sharp.

## Changing the lock

```bash
veracrypt --text --change volume.hc
```

interactively, or non-interactively as
the lab did
(`veracrypt-lab/t05-change-password-pim.txt`):

```bash
veracrypt --text --non-interactive --change vault.hc \
  -p <old password> \
  --new-password <new password> --new-pim 505
```

One command can change any combination
of password (`--new-password`), PIM
(`--new-pim`) and keyfiles
(`--new-keyfiles`). The lab changed
vault.hc's password and set PIM 505,
then proved the change the chapter 6
way: the **new password with the old
(default) PIM was refused**, and the
**new password with PIM 505 was
accepted**. The credential is the whole
combination; change one factor and the
old combinations are void.

Two subtleties the lab measured,
because they are widely gotten wrong:

- **The old password dies everywhere
  in the file.** VeraCrypt rewrites
  *both* embedded headers — primary and
  backup — on a change. The old
  password, tried against the changed
  volume, was refused
  (`veracrypt-lab/t05c-old-password-backup-header.txt`).
  The embedded backup protects you
  against header *damage*, not against
  your own password change.
- **But the master key survives.** So a
  header backup *file* made before the
  change still holds the same master
  key under the old password — and
  restoring from it resurrects the old
  password. The restore section below
  shows the program saying so itself,
  and transcript
  `veracrypt-lab/t13-old-password-resurrected.txt`
  demonstrates it end to end. Practical
  meaning: **changing a password does
  not retire it** while an old header
  backup exists. If a password is
  compromised, its old backups must be
  destroyed too — or the volume
  replaced (chapter 13, recipe 4).

## Backing up the header

```bash
veracrypt --text --backup-headers volume.hc
```

The lab's run is transcript t08. Three
things about it surprise everyone the
first time:

1. **It ignores the command line.** The
   `-p` and `--pim` flags are not
   accepted here; the command prompts
   for the password, the PIM, keyfiles,
   and whether the volume contains a
   hidden volume, all interactively.
2. **It asks for entropy.** After the
   filename it demands *"Please type at
   least 320 randomly chosen characters
   and then press Enter"* — fresh
   randomness to re-encrypt the backup
   with. Wiggle-typing is genuinely the
   expected input.
3. **The result is tiny**: 131,072
   bytes — both headers, primary and
   backup, normal and hidden, in one
   file.

The warnings VeraCrypt prints while
making it deserve quoting, because they
are the custody rules: the backup
*"still contains the master key"*; a
backup made after a password change is
useless with the old password and vice
versa; and if the original volume had
a hidden volume, the backup restores
that too. Treat the file as a key: it
opens nothing without its password,
but it *is* half of the lock, and it
belongs on different media from the
volume (chapter 11).

## Destroying and restoring: the round trip

The lab's stress test
(`veracrypt-lab/t09-corrupt-and-restore.txt`):

1. Copied vault.hc to vault-copy.hc.
2. **Zeroed the first and last
   128 KiB** of the copy — both the
   primary header area and the embedded
   backup header area, gone.
3. Tried the *correct* password and
   PIM against the copy. It could not
   authenticate: with no header left to
   open, even the right password has
   nothing to unlock. The attempt was
   still searching when its ten-minute
   cap stopped it — twice (chapter 16
   explains why a failing search at
   PIM 505 is so slow). A dead volume
   with its data intact — the exact
   disaster the header backup exists
   for.
4. Restored from the backup file:

   ```bash
   veracrypt --text --restore-headers vault-copy.hc
   ```

   The restore flow: choose *2) Restore
   from an external backup file*,
   confirm through a warning that says
   in plain words *"Restoring a volume
   header also restores the volume
   password that was valid when the
   backup was created"*, give the
   backup's filename, its password and
   PIM, and another 320 random
   characters at the entropy prompt.
   Then:

   ```text
   The volume header has been successfully restored.
   ```

5. Tried the correct password and PIM
   on the restored copy: **accepted**
   (the sandbox's fuse error, chapter
   6) in about two seconds. The volume
   was back from the dead, with its
   data area never touched.

Note what the lab could *not* have
done: restore from the embedded backup
(choice 1 in the menu) — it had zeroed
that too, deliberately. On a real
volume with only the primary header
damaged — the common accident — the
embedded backup is the first thing to
try, and it needs no separate file.

## The custody rules, collected

- Make a header backup when you create
  a volume, and a **fresh one after
  every password, PIM or keyfile
  change** — the old one now pairs with
  credentials you have retired.
- Store it away from the volume; a
  backup in the same folder dies in the
  same accident.
- Record *which* password and PIM the
  backup pairs with (in your password
  manager, not on the file's name —
  chapter 9's rule that a thing need
  not look like a key applies here too).
- Never restore a header "to see what
  happens": a restore is a password
  change wearing a rescue's clothes,
  as the program's own warning says.
