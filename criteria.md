# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's sentences are stock sentences and pangrams.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 15 names what the tool
   cannot do: system encryption on Windows only, an open book
   while mounted, no recovery of any kind, a fixed container
   size, a master key that never changes so old external header
   backups keep their old passwords alive, a container file
   whose existence is not itself deniable, no support for
   TrueCrypt-format volumes, no network or multi-user mounting,
   no official phone apps, and no integrity protection for the
   data area.
8. **The ethics rule is in the text, early.** Encryption cuts
   both ways, and chapter 3 draws the line plainly before any
   procedure depends on it: this guide teaches protection for
   your own data; it does not teach evading lawful process, it
   flags compelled disclosure as a legal question for a lawyer
   rather than a technical one, and chapter 7 presents hidden
   volumes with their deniability limits stated as plainly as
   their mechanism.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a 2-CPU, ~7 GB Ubuntu 24.04 sandbox,
shared with other work while the lab ran (timings are therefore
ranges, and the guide says so). VeraCrypt **1.26.30** was
obtained as the official console package
`veracrypt-console-1.26.30-Ubuntu-24.04-amd64.deb` from the
project's GitHub releases (reached from veracrypt.io's
Downloads page) and proven genuine before use: the project's
PGP key fingerprint
`5069 A233 D55A 0EEB 174A 5FC3 821A CD02 680D 16DE` matches the
fingerprint published on veracrypt.io's Digital Signatures
page; that key verifies the signature on both the published
SHA-256 sums file and the package itself, and the package's
hash matches the signed sums (t00). The sandbox could not
system-install the package, so it was **extracted locally with
`dpkg-deb -x`** and run from the extracted tree — and one of
its declared dependencies, `libfuse2t64`, was fetched as a
distribution .deb from the Ubuntu noble archive and extracted
the same way, because mounting loads `libfuse.so.2` at runtime
even though `ldd` shows nothing missing. The package's own
bundled HTML user guide grounded the documented facts (PIM and
Argon2id formulas, keyfile rules, deniability wording, the
1.26.29 Argon2id addition and the 1.26.6–1.26.28 hidden-volume
deniability warning, both from the project's release notes).

The sandbox has **no block devices at all** — no loop device,
no `/dev/mapper`, no `/dev/fuse` — so no mount could complete.
The lab turned that boundary into its measuring instrument:
a wrong password, PIM or keyfile is refused by the volume
itself with VeraCrypt's all-causes error, while correct
credentials are accepted and stopped only by the missing
device, with the verbatim error
`Error: fuse: device not found, try 'modprobe fuse' first`.
Every credential claim in the guide rests on which of those
two messages the attempt produced. All transcripts live in
`veracrypt-lab/`.

Run for real, with transcripts as evidence:

- The full verification chain above, and version plus the
  complete console help surface (t00, t01).
- Creating a 20 MB container (AES, SHA-512, FAT) for real,
  and the discovery that `--volume-properties` only speaks
  about mounted volumes (t02, t03).
- The two failure signatures side by side: a wrong password
  refused by the volume; the correct password accepted and
  stopped only by the missing FUSE device (t04).
- Changing a volume's password **and** PIM (to 505) and
  proving the new combination in both directions — new
  password with the default PIM refused, with PIM 505
  accepted (t05). The old password refused against the
  changed volume, because a change rewrites both embedded
  headers (t05c) — after attempts that ran past 10 and 15
  minutes under load were diagnosed live as the exhaustive
  search working, derivation worker at ~1 GB, not a hang
  (t05b).
- Generating a keyfile with `--create-keyfile` and creating
  a volume that requires password plus keyfile (t06);
  proving both factors — together accepted in ~2 s, the
  password alone refused after the ~49 s search (t10).
- Creating an outer volume and a hidden volume inside it,
  with the program's own hidden-volume warning text (t07);
  proving the two doors — outer password accepted in ~2 s,
  hidden password accepted in ~28 s, a junk password refused
  (t11).
- Backing up a volume header to a 131,072-byte file,
  including the interactive prompt sequence (command-line
  credentials are ignored), the "contains a hidden volume"
  question and the demand for 320 randomly typed characters
  (t08).
- Destroying both headers of a copy (first and last 128 KiB
  zeroed): the correct password could not authenticate —
  the attempt was still searching when stopped at ten
  minutes, twice — then restoring the headers from the
  backup file and proving the restored copy accepts its
  password again in ~2 s (t09).
- The resurrection round trip: back up a header, change the
  password, prove the new password works, restore the
  pre-change backup, and watch the new password get refused
  while the original password and PIM are accepted again —
  an old header backup keeps its old password alive (t13).
- The internal algorithm self-tests, all passing (t12).

Documented only — described from the project's bundled
documentation, not run, and the text does not claim otherwise:

- Everything past a successful mount: using a mounted
  volume, dismounting, `--list` on real mounts, and the
  outer-volume formatting and decoy-filling steps that
  complete a hidden volume's setup — all blocked by the
  missing devices, all flagged where they appear.
- The graphical program's flows, including favourites,
  automount setup and the Tools → Benchmark speedometer
  (the console build has no benchmark switch at all; its
  `--test` runs self-tests only).
- Partition and device encryption, and Windows system
  encryption with its pre-boot authentication.
- Windows traveler (portable) mode, security tokens and
  smart-card keyfiles, and third-party phone apps that open
  VeraCrypt containers.
- The neighbour tools of chapter 14 (LUKS, BitLocker,
  FileVault, GnuPG, 7-Zip, Cryptomator) — characterised, not
  run.

Fixtures are this guide's own and deliberately generic:
stock passphrase shapes (`bramble-lantern-47` and kin, all
published openly in the transcripts because the lab volumes
they protected were destroyed with the lab), small round
sizes, and no names, organisations or real events. The lab
volumes themselves never held reader data of any kind.
