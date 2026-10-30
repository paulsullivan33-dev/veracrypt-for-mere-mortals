# Performance and Hardware Acceleration

**Outcome: realistic expectations — a
mounted volume is nearly as fast as the
drive under it, the unlock is supposed
to be slow, and you know which choices
move which number.**

## The drive-speed side: ciphers in hardware

Decrypting on the fly means every byte
you read or write passes through the
cipher. On any modern CPU that is close
to free for **AES**: processors have
carried dedicated AES instructions
(AES-NI on Intel, the AES instructions
on AMD and ARM) for well over a decade,
and VeraCrypt uses them automatically
when present — there is no setting to
find. With hardware AES, a single core
encrypts faster than most storage can
deliver data, so a VeraCrypt volume on
an SSD feels like the SSD. The lab's
machine reports AES instructions in
its CPU (an AMD EPYC; `grep aes
/proc/cpuinfo`), which is the normal
case — check yours the same way, or
with the graphical program's benchmark
below.

The other ciphers have no such
universal accelerator. Serpent, Twofish,
Camellia and Kuznyechik run in software:
still fast in absolute terms, but a
**cascade** (chapter 2) pays that cost
two or three times over, forever, on
every byte. That is the real price of a
cascade — not the creation time. Choose
one because your threat model asks for
it, not as a default garnish.

What the lab could verify of the
machinery, it did
(`veracrypt-lab/t12-selftest.txt`):

```text
$ veracrypt --text --test
Self-tests of all algorithms passed
```

The console's `--test` runs the
internal known-answer tests for every
algorithm and KDF — a genuine
"the cryptography in this build is
intact" check, worth running after any
install. What the console build does
*not* have is a speed benchmark:
scanning the full help surface
(transcript t01) shows no benchmark
switch. The speedometer lives in the
graphical program (Tools → Benchmark),
which reports per-cipher throughput on
your machine; this guide did not run
it, and quotes no numbers it didn't
measure.

## The unlock side: slowness is the product

Mounting pays the KDF once (chapters 2
and 8). The lab's measured shape of
that cost, on a small shared machine:

- Correct credentials: about **two
  seconds** to acceptance.
- Every failed attempt: **about a
  minute** of exhaustive search at the
  default PIM — and far longer at a
  high PIM, where Argon2id readings of
  the search pushed past ten minutes
  under load (chapter 16 tells the
  whole story).

Read those numbers as the feature
working. An attacker testing guesses
against a stolen container pays the
full search *per guess*, in memory as
well as time when Argon2id is used;
you pay it once per mount. If your own
unlock feels slow, the honest levers
are a lower PIM (a deliberate security
trade, chapter 8) or a faster machine —
not a weaker password, which is the
one variable the attacker gets to
exploit at leisure.

## Practical notes

- **Creation time** is dominated by
  the random fill for non-quick
  formats; a large partition volume
  created without `--quick` takes as
  long as writing the whole device
  with randomness takes. Containers
  are usually small enough that this
  is minutes, not hours.
- **Many small files** inside a volume
  are filesystem business, not cipher
  business — the inside filesystem
  (chapter 11) matters more than the
  cipher choice for that workload.
- **Antivirus and indexing** on
  Windows see a mounted volume as a
  new drive and may scan it; that cost
  is theirs, not VeraCrypt's.
