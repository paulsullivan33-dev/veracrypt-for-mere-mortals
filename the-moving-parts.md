# The Moving Parts

A VeraCrypt volume is a handful of
ideas stacked neatly. This chapter
names each part and says which part
holds what, because almost every
confusion later — a password that
stopped working, a backup that revives
an old password, a file that is safe to
copy — is one of these parts being
mistaken for another.

## The container

The **volume** itself: a file (or a
partition) of fixed size, filled end to
end with encrypted data. A fresh volume
is written full of random bytes before
you store anything, so its used and
unused regions look identical from
outside. Its size never grows or
shrinks afterwards: a 2 GB container is
2 GB on disk from the day it is made,
full or empty. Choose the size when you
create it (chapter 5); changing it
later means making a new volume and
copying across.

## The header

The first 128 KB of the volume is the
**header area**. Buried in it — itself
encrypted — is the small block that
says how the volume is encrypted and,
most importantly, holds the **master
key**: the random key that actually
encrypts your data. The header is what
your password opens.

A second, **embedded backup header**
lives in the last 128 KB of the volume.
It holds the same master key, encrypted
the same way, and VeraCrypt falls back
to it automatically if the main header
is damaged. Chapter 10 adds the third
copy: an external **header backup
file** you keep somewhere safe.

## Password, PIM and keyfiles — the header key

Your password never encrypts your data
directly. It is fed, with a random
**salt** stored in the header, through
a **key derivation function** (KDF) —
a deliberately slow calculation — to
produce a **header key**, and the
header key decrypts the header. Three
inputs can feed that derivation:

- The **password** itself.
- The **PIM** (Personal Iterations
  Multiplier): a number that sets how
  much work the derivation does.
  Chapter 8 gives it a chapter because
  it is both a security control and a
  second secret.
- One or more **keyfiles**: files whose
  contents are mixed into the
  derivation alongside the password.
  Chapter 9.

Change any of the three and the header
key changes — so changing a password
means re-encrypting the *header* with a
new header key, a job of milliseconds.
The master key, and all your data, are
untouched. (Both embedded headers are
rewritten, so the old password dies
everywhere inside the file — the lab
proved that in chapter 10.) But a
header *backup file* made before the
change still pairs with the old
password, and restoring from it brings
the old password back to life, because
the master key it holds never changed.
Chapter 10 demonstrates both halves.

## The master key and the cipher

The data itself is encrypted with a
**cipher** — VeraCrypt offers AES,
Serpent, Twofish, Camellia and
Kuznyechik, alone or in cascades of
two or three — using the master key,
in the XTS mode designed for disks.
The master key is generated randomly
when the volume is created and never
changes for the life of the volume.

## The hash (PRF)

The KDF needs a **hash function** —
VeraCrypt calls it the PRF: SHA-512,
SHA-256, BLAKE2s-256, Whirlpool or
Streebog, used inside HMAC-PBKDF2, or
the newer memory-hard **Argon2id**.
This choice is stored in the header and
affects only how passwords are
processed, not how data is encrypted.

## The program, in two faces

Everything above is manipulated by the
program, which comes in two faces over
one engine:

- The **graphical program** (named just
  `veracrypt` in the GUI packages), with
  a volume-creation wizard, a list of
  drive slots, and dialogs for every
  operation in this guide.
- The **console program** (the
  `veracrypt-console` package, driven
  with `--text`), which does the same
  work from the command line. This is
  the face the lab used, because its
  every step can be shown, checked and
  repeated. Some operations — header
  backup and restore among them — stay
  stubbornly interactive even here, and
  chapter 10 shows what that looks like.

On Linux the mounted drive is presented
through the kernel's device mapper and
FUSE, which is why mounting needs
administrator rights (VeraCrypt asks
through `sudo`) and why the lab's
deviceless sandbox could authenticate a
volume but never present it — the story
chapter 6 tells with the error messages
to prove it.
