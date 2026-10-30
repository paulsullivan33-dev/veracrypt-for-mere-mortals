# Mounting and Dismounting

**Outcome: you can open a volume, use
it, and close it — and you can tell the
difference between "the password was
wrong" and "the password was right",
which is the whole of daily trust in
this tool.**

Mounting is the daily ritual:

```bash
veracrypt --text vault.hc /mnt/vault
```

In the graphical program: select a free
slot, choose the file, press Mount,
type the password (and PIM, and
keyfiles, if the volume has them). A
few seconds of derivation later the
drive appears — a letter on Windows, a
folder on Linux and macOS — and behaves
like any other storage until you
dismount it, from the same window, or:

```bash
veracrypt --text --dismount vault.hc     # one volume
veracrypt --text --dismount              # all of them
```

`veracrypt --text --list` shows what is
currently mounted. Three habits make
mounting a non-event: always dismount
when you finish (a mounted volume is an
open box, chapter 3), never leave a
volume mounted across sleep if you can
help it, and let VeraCrypt finish —
pulling a drive or killing power
mid-write can damage the *filesystem*
inside, though not the encryption.

## How the lab proved a password without mounting

The lab's sandbox has no disk devices,
so no mount could complete there. What
it could do is watch *where* attempts
stop — and VeraCrypt's two failure
modes are cleanly different
(`veracrypt-lab/t04-mount-attempts.txt`).

**A wrong password is refused by the
volume itself:**

```text
Error: Operation failed due to one or more of the following:
 - Incorrect password.
 - Incorrect Volume PIM number.
 - Incorrect PRF (hash).
 - Not a valid volume.
 - Volume uses an old algorithm that has been removed.
 - TrueCrypt format volumes are no longer supported.
```

Read that list again: it is deliberately
unhelpful. It will not say *which* thing
was wrong — password, PIM, hash — or
even whether the file is a VeraCrypt
volume at all. That ambiguity is a
security feature you will curse exactly
once, the day you mistype a PIM (chapter
16), and appreciate every other day.

**The right password is accepted**, the
header opens, the master key comes out —
and only then does the lab's machine
stop it, at the step where a drive
would be presented:

```text
Error: fuse: device not found, try 'modprobe fuse' first
```

On your machine that step succeeds
instead: the kernel's device mapper and
FUSE — the `dmsetup` and `libfuse2t64`
dependencies from chapter 4 — present
the decrypted drive at the mount point.
That one line of sandbox failure is
therefore this guide's certificate for
every credential in the lab: whenever a
later chapter says a password, PIM,
keyfile or restored header "was
accepted", this is the message that
proves it, and whenever one was wrong,
it is the first message instead. (One
more dead end the lab checked so you
don't have to: `--volume-properties`
sounds like it should describe a
container file, but it only reports on
*mounted* volumes — against an unmounted
file it answers `Error: No such volume
is mounted.`, transcript t03.)

## Favourites and automount

Two conveniences the graphical program
offers, worth knowing by name even if
you drive the console:

- **Favourite volumes** are remembered
  volumes with a saved mount point and
  options, so a daily volume is a click
  (plus its password) instead of a
  file hunt. The console equivalent is
  `--auto-mount favorites`.
- **Automount on login** (Windows) or
  `--auto-mount devices` mounts
  remembered volumes when you sign in,
  asking for the password then. The
  password is never stored — automount
  saves the *prompt*, not the secret.
  Anything that claims to mount your
  volumes without ever asking is
  something chapter 3 warned about.

Mount options worth knowing exist:
**read-only** mounting (`-m ro` on the
console, a checkbox in the GUI) for
looking without touching — the safe way
to open a volume you are about to
repair — and mounting with **hidden
volume protection**, which is important
enough to live in chapter 7.
