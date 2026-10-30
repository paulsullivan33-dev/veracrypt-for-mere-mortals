# Limits

What VeraCrypt cannot do, collected in
one place — several of these have been
flagged on the way through, and a
reader deciding whether to trust the
tool deserves them together.

- **System encryption is Windows-only.**
  VeraCrypt cannot encrypt the drive a
  Linux or macOS system boots from.
  On those platforms it covers file
  containers and non-system partitions;
  whole-disk protection is LUKS's and
  FileVault's job (chapter 14).
- **A mounted volume is an open book**
  to the machine it is mounted on —
  to you, to your programs, and to any
  malware running as you. There is no
  mode in which files are usable but
  hidden from the operating system
  that uses them (chapter 3).
- **No recovery, ever.** No forgotten-
  password reset, no escrow, no master
  password, no support desk that can
  help. Password, PIM and keyfiles are
  the whole of the law (chapters 8–10).
- **Container size is fixed at
  creation.** A volume does not grow
  with its contents. A bigger box
  means a new volume and a copy.
- **The master key never changes.**
  Changing the password rewrites both
  embedded headers — the old password
  dies inside the file — but it only
  re-wraps the same master key, so an
  old *external* header backup file
  plus its old password, restored,
  opens the volume again. Rotating a
  compromised password completely
  means a new volume (chapters 2, 10).
- **It does not hide that a file is a
  container-shaped object** — a large
  file of pure randomness invites
  questions, and hidden volumes answer
  only some of them, at real cost in
  care (chapter 7).
- **TrueCrypt is gone.** Current
  VeraCrypt refuses TrueCrypt-format
  volumes (the lab's own error message
  lists the fact). An old TrueCrypt
  volume must be converted with an old
  VeraCrypt release, kept somewhere
  safe, or opened by the last TrueCrypt
  — plan the migration before the old
  tools become unavailable.
- **No network or multi-user life.** A
  volume is mounted by one machine at a
  time; mounting the same container
  read-write from two machines (say,
  through a shared folder) will corrupt
  its filesystem. Sharing the *files*
  means sharing them after dismount, or
  through ordinary network sharing of a
  folder on the one mounting machine.
- **Phones are third-party territory.**
  No official VeraCrypt application
  exists for Android or iOS (chapter 11).
- **Encryption is not integrity.**
  VeraCrypt authenticates the header,
  but the data area has no per-block
  authentication: someone who can write
  to your container can corrupt or
  tamper with its contents block by
  block, and XTS-mode encryption will
  faithfully decrypt the tampered
  blocks into garbage or subtly altered
  data. Against an attacker with
  repeated write access to your storage,
  you need authenticated tools (the
  file-encryption neighbours of chapter
  14 authenticate what they encrypt)
  and good backups.
- **Performance has a price list.**
  Cascaded ciphers multiply the CPU
  cost; Argon2id makes every mount —
  and every failed mount — pay in
  memory and seconds (chapters 8, 12).
  Neither is a reason to avoid them;
  both are reasons to choose with eyes
  open.
