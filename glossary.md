# Glossary

**Argon2id** — the memory-hard key
derivation function VeraCrypt offers as
an alternative to PBKDF2. Makes each
password guess cost memory as well as
time (chapter 8).

**Automount** — mounting remembered
volumes automatically at sign-in,
asking for their passwords then. The
password itself is never stored
(chapter 6).

**Cascade** — encrypting with two or
three ciphers in sequence (for example
AES–Twofish–Serpent). Costs CPU in
proportion; chapter 12.

**Cipher** — the algorithm that
encrypts the data itself: AES, Serpent,
Twofish, Camellia or Kuznyechik.

**Container** (file-hosted volume) — a
single file that VeraCrypt mounts as a
drive; the most common form of volume.

**Dismount** — closing a mounted
volume, after which it is encrypted
noise again. Also called unmounting.

**Favourite volume** — a remembered
volume with a saved mount point and
options, for quick mounting (chapter 6).

**Header** — the encrypted block at the
start of a volume holding the master
key and the volume's settings. What
your password opens (chapter 2).

**Header backup** — a small file
holding a copy of a volume's headers,
for restoring them after damage. Made
and restored in chapter 10.

**Header key** — the key derived from
your password (plus PIM and keyfiles),
used to decrypt the header.

**Hidden volume** — a second volume
living undetectably inside an outer
volume's unused space, opened by its
own password (chapter 7).

**Hidden volume protection** — the
guard that stops writes to an outer
volume from overwriting the hidden
volume inside it (chapter 7).

**KDF** (key derivation function) —
the deliberately slow calculation that
turns a password into a header key:
PBKDF2 with a hash, or Argon2id.

**Keyfile** — a file whose contents are
mixed into the key derivation alongside
the password (chapter 9).

**Master key** — the random key that
actually encrypts the volume's data.
Created with the volume, never changed,
held inside the header.

**Mount** — opening a volume so it
appears as a drive, after presenting
its password and other factors.

**On-the-fly encryption** — decrypting
data as it is read and encrypting it as
it is written, so the volume is never
decrypted wholesale.

**Outer volume** — the visible volume
that hosts a hidden volume.

**PIM** (Personal Iterations
Multiplier) — the number that sets how
much work the key derivation does, and
a second secret beside the password
(chapter 8).

**Plausible deniability** — the
property that, having surrendered the
outer password, you can plausibly deny
a hidden volume exists, because its
existence cannot be proven (chapter 7).

**PRF** — VeraCrypt's name for the hash
function used by the KDF: SHA-512,
SHA-256, BLAKE2s-256, Whirlpool or
Streebog.

**Salt** — random data stored in the
header and mixed into the derivation so
identical passwords on different
volumes derive different keys.

**Slot** — one of the numbered places
in the graphical program where a
mounted volume sits.

**System encryption** — encrypting the
partition the operating system boots
from. VeraCrypt offers it on Windows
only (chapters 1, 15).

**Traveler mode** — running VeraCrypt
on Windows from a USB stick without
installing it (chapter 11).

**Volume** — the encrypted unit itself,
in either form: a file container or an
encrypted partition/device.
