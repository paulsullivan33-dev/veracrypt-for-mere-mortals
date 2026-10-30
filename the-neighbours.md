# The Neighbours

VeraCrypt is not the only way to keep
files encrypted, and for some jobs it
is not the right one. The neighbours,
characterised honestly — most were not
run by this guide's lab, and the
chapter does not pretend their
descriptions are measurements.

## Whole-disk encryption: the blunt answer

- **LUKS** (Linux Unified Key Setup) is
  Linux's native disk encryption, the
  usual way a Linux system encrypts its
  whole drive, swap included. If your
  worry is *everything on this Linux
  machine*, including the leaks chapter
  3 described (temp files, hibernation),
  LUKS — set up at install time — is the
  answer, and VeraCrypt on Linux is the
  tool for the containers and sticks
  that travel.
- **BitLocker** is Windows' built-in
  whole-disk encryption (Pro editions
  and up), and **FileVault** is macOS's.
  Both are the right default for "my
  laptop was stolen" on their platforms.
  VeraCrypt's Windows system encryption
  still exists for cases BitLocker
  doesn't fit — older editions,
  distrust of recovery-key escrow, or
  hidden operating systems — but it is
  the specialist's choice now, not the
  default.

## Encrypting files, not drives

- **age** — covered by
  [age for Mere Mortals](https://paulsullivan33-dev.github.io/age-for-mere-mortals/) —
  encrypts individual files for sending
  and archiving: small, modern, no
  drives to mount. If your job is
  "encrypt this backup file before it
  leaves", age is simpler than a
  container. VeraCrypt wins when you
  want a *place* files live encrypted
  and are used in place.
- **GnuPG** is the elder statesman of
  file and email encryption, with a
  web-of-trust model age deliberately
  dropped. Powerful, sprawling, and the
  right tool mainly where its ecosystem
  already rules.
- **7-Zip with a password** makes
  AES-encrypted archives anyone can
  open with a ubiquitous tool — fine for
  sending a bundle, weak as a home for
  working files: every edit means
  re-archiving, and the plaintext tends
  to linger.
- **Cryptomator** encrypts files one by
  one inside a folder made to sit in
  cloud storage, so a synced vault never
  exposes filenames and partial changes
  sync partially. For "my documents
  folder, in a cloud drive, encrypted",
  it is arguably a better shape than a
  VeraCrypt container — recall chapter
  11's torn-sync warning, which is the
  problem Cryptomator was designed
  around.

## Password keepers are a different species

[KeePassXC for Mere Mortals](https://paulsullivan33-dev.github.io/keepassxc-for-mere-mortals/)
and its cousins encrypt a *database of
small secrets* — passwords, notes,
keys. A VeraCrypt container can hold
such a database as a file (and a
keyfile can even live in one), but a
container is not a password manager and
a password manager is not a drive.
Most readers end up with both, each at
its own job.

## Backups are the other half

Encryption decides who can read your
files; backups decide whether there are
files. [Backups for Mere Mortals](https://paulsullivan33-dev.github.io/backups-for-mere-mortals/)
and [Restic for Mere Mortals](https://paulsullivan33-dev.github.io/restic-for-mere-mortals/)
cover the discipline; chapter 11 of
this guide covers the container-specific
rules (copy the file dismounted, keep
the header backup separate).
