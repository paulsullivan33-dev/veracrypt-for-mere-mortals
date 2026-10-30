# Troubleshooting

The failures you will actually meet,
in the order you will probably meet
them.

## "Operation failed due to one or more of the following…"

The all-causes error (chapter 6). It
means the header did not open, and it
refuses to say why. Work the list in
order of likelihood:

1. **Caps lock / keyboard layout.**
   The classic. Type the password
   somewhere visible and look at it.
2. **Wrong PIM.** A volume with a
   non-default PIM mounted with the
   default (or vice versa) fails with
   exactly this message. If you set a
   PIM, it is part of the password now
   (chapter 8).
3. **Missing or changed keyfile.** A
   keyfile that was edited, re-saved,
   recompressed or restored from a
   lossy backup is a different keyfile
   (chapter 9).
4. **You are opening the wrong door.**
   A hidden volume's password typed at
   a volume with no hidden volume; the
   outer password typed when you meant
   the hidden one. Both fail or succeed
   *silently as far as the message
   goes* — a successful mount shows you
   which volume you got by its contents.
5. **The header is damaged.** If the
   password is certainly right and the
   file was truncated, badly copied or
   sat on failing storage: restore the
   header (chapter 10) — embedded
   backup first, external backup file
   second.

## The right password "doesn't work" — and takes forever to say so

A failed mount is not a quick no. The
lab measured the search
(`veracrypt-lab/t10-keyfile-mount-attempts.txt`,
`t11-hidden-volume-attempts.txt`,
`t05c-old-password-backup-header.txt`):

- A correct credential answers in
  about **two seconds** when it matches
  the first header tried, ~30 seconds
  when it sits deeper in the search.
- A wrong one takes **about a minute**
  of solid work before refusing: every
  PRF, against every header slot,
  outer and hidden, primary and
  backup, must be derived and tried.
- Add a **high PIM** and it gets far
  worse: attempts carrying PIM 505 were
  still searching after **ten minutes**
  on the lab's shared machine
  (`veracrypt-lab/t05b-old-password-timing.txt`,
  `t09-corrupt-and-restore.txt`).
  The search must also try Argon2id
  readings of your PIM, and Argon2id's
  price — the lab watched the
  derivation worker process running at
  a full gigabyte of memory — is paid
  per try.

So a slow failure is the design
working, not a hang. Two cautions
follow. First, don't kill a mount
attempt early and conclude the
password is wrong; you may have
murdered a success at second 40 of 50.
Second, the wait will not show in
`time`'s user column or a casual CPU
glance — the derivation runs in a
worker process while the main process
waits, so watch for a `veracrypt`
process with a very large memory
footprint before declaring anything
stuck.

## "Error: fuse: device not found, try 'modprobe fuse' first"

This was the lab's boundary, and it
can be yours on a real Linux machine.
It appears **after** authentication
succeeds — your password was right —
when the kernel side cannot present
the drive: FUSE or device mapper
unavailable, modules unloaded, or
(in a container or VM) the devices
simply not there. On a normal desktop
install the package's dependencies
(dmsetup, libfuse) prevent it; in
containers, WSL and minimal systems,
expect it and fix the environment,
not the volume. On Windows and macOS
the equivalent failures come from the
VeraCrypt driver not loading — a
reboot after install, and checking
that no security policy blocked the
driver, are the first moves.

## The volume mounts, but the files are garbage — or the drive "needs formatting"

Do not format it. The likeliest story:
the header was restored from a
*different volume's* backup. The
program warns about exactly this when
a backup is made
(`veracrypt-lab/t13-old-password-resurrected.txt`):
the volume mounts, because the backup
is a valid header — for another
volume's master key — and every block
then decrypts to noise. Restore the
correct backup, or the embedded one,
and dismount anything mounted this
way without writing to it.

The same symptom, milder: an outer
volume written to *without* hidden
volume protection can have eaten its
hidden volume (chapter 7). The hidden
volume then fails like a damaged one.
There is no undelete; the protection
checkbox was the whole cure.

## "No such volume is mounted"

`--volume-properties` and friends
describe *mounted* volumes only. Run
against a container file they answer
with this and nothing else
(`veracrypt-lab/t03-volume-properties.txt`).
There is no offline "inspect this
header" command — by design, since an
unopened header is supposed to be
indistinguishable from noise. What you
can check offline is the file itself:
its size (volumes don't grow), and
that it is not truncated against a
known-good copy or checksum.

## The volume opens on one machine but not another

- Different keyboard layout at the
  password prompt (the password is the
  characters, not the keys).
- An older VeraCrypt on the other
  machine meeting a newer feature —
  Argon2id, for one, only arrived in
  version 1.26.29 (the release notes
  say so); a machine running anything
  older cannot open an Argon2id
  volume. Keep versions in step
  (chapter 11).
- A TrueCrypt-era file: current
  VeraCrypt refuses TrueCrypt format
  outright; the error message's last
  bullet is the tell (chapter 15).
- The copy is incomplete: a container
  copied while mounted, or through a
  sync that caught it mid-write, can
  be torn (chapter 11). Compare size
  and checksum against the source.

## Forgotten password, forgotten PIM, lost keyfile

There is no procedure. That is not
this guide being coy: there is no
reset, no escrow and no back door, and
anyone who sells you one is selling
snake oil or a brute-force rig.
What remains is memory work — old
password managers, written records,
the combinations you were rotating
between — and the brute-force maths
of chapter 8, which is precisely as
kind as the strength of what you
chose. This is why chapter 5 made you
type the first password twice, and
why the custody rules in chapters 9
and 10 exist.
