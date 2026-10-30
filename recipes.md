# Recipes

Four complete jobs, assembled from the
chapters before them. Passwords shown
are placeholders — yours are yours.

## 1. A vault for the records drawer

The tax, insurance, identity-document
folder, as a container on your main
machine:

```bash
veracrypt --text --non-interactive --create records.hc \
  --size 2147483648 --volume-type normal \
  --encryption AES --hash SHA-512 --filesystem FAT \
  --password '<a long passphrase>' \
  --random-source /dev/urandom
```

2 GB, default PIM, FAT because it may
one day need opening on a borrowed
Windows machine. Then: header backup to
a file kept on different media
(chapter 10), a copy of `records.hc`
in your ordinary backups (dismounted,
chapter 11), and a note in your
password manager recording that the
volume exists and where its header
backup lives — not the password itself,
unless the manager is where you keep
passwords, in which case it is exactly
the place.

## 2. A USB stick that shrugs off loss

A container filling most of a stick,
with a keyfile kept on the machine, so
the stick alone is useless and the
machine alone is useless:

```bash
veracrypt --text --non-interactive --create-keyfile ~/keys/stick.key \
  --random-source /dev/urandom
veracrypt --text --non-interactive --create /media/stick/travel.hc \
  --size 15032385536 --volume-type normal \
  --encryption AES --hash Argon2id --filesystem exFAT \
  --password '<its own passphrase>' --keyfiles ~/keys/stick.key \
  --random-source /dev/urandom
```

Argon2id here on purpose: this volume
faces the world, and its unlock cost is
paid by you once per trip and by a
finder's guessing rig forever. Mount it
read-only when you only need to look
(`-m ro`), and format exFAT because the
stick will meet Windows machines.

## 3. Two people, one volume

A club or household volume two officers
must both be able to open, each with
their own password: that is the token
pattern of chapter 9 — the *same*
keyfile held by both, wrapped in each
person's own token or kept on each
person's own stick, and each holder's
own password. The volume cannot tell
the holders apart; the arrangement is
social, the cryptography just makes it
possible. Documented-only in the lab —
the pieces were each proven (keyfile
creation, keyfile volumes) but the
two-person ceremony was not rehearsed.

If instead the rule is that *both* must
agree to open it, use two keyfiles from
two holders, both required at mount:
`--keyfiles officer-a.key,officer-b.key`.
Nobody opens it alone.

## 4. Retire a volume properly

When a container's job ends:

1. Mount it, copy out anything still
   needed, dismount.
2. Decide whether the *file* must become
   unreadable or merely gone. Deleting
   the file is enough against casual
   recovery of the volume as a whole,
   but the encrypted bytes in unallocated
   space stay exactly as readable as
   they ever were — which is to say,
   sealed without the password. If the
   password is retired and destroyed
   too, the bytes are noise in practice.
3. Destroy the password, the header
   backups and the keyfiles with the
   same intent — a retirement that keeps
   a header backup and a written-down
   password has retired nothing.
4. If the volume ever held a hidden
   volume, retire it as the hidden
   volume deserved: the outer password
   alone is not the whole secret.
