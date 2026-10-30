# PIM — the Unlock-Cost Knob

**Outcome: you can set, change and
correctly use a PIM, and you know what
it buys and what it costs.**

The **PIM** — Personal Iterations
Multiplier — is a VeraCrypt invention,
added in version 1.12, with no
equivalent in the original TrueCrypt.
It sets how much work the key derivation
does to turn your password into a
header key (chapter 2). Every guess an
attacker makes at your password must
repeat that work, so more work means
slower guessing. The same work is also
done by *you*, once, at every mount:
the PIM is the dial that trades your
unlock time against the attacker's
guessing rate.

## The arithmetic

With the classic PBKDF2 derivation, the
official documentation gives the count
for file containers and non-system
volumes as:

```text
iterations = 15,000 + (PIM × 1,000)
```

Leave the PIM unset and VeraCrypt uses
its long-standing default of **500,000
iterations** with SHA-512 — equivalent
to a PIM of **485**. So a PIM of 505,
which the lab set below, means 520,000
iterations: a little more work than
default. A PIM of 1000 means 1,015,000:
roughly double the guessing cost, and
roughly double your wait at mount.

With the newer **Argon2id** derivation —
it only arrived in VeraCrypt 1.26.29,
so older installs cannot open an
Argon2id volume at all — the same dial
steers different machinery:
memory use rising from 64 MB by 32 MB
per PIM step up to a 1 GB cap, and a
time cost that grows with it. Its
default behaves like a PIM of 12 —
416 MB of memory and 6 iterations —
which is why an Argon2id volume can
take noticeably longer, and far more
RAM, to *fail* a wrong password than a
PBKDF2 one: every wrong guess pays full
price too. The lab met that cost while
timing a failed mount; chapter 16 tells
the story.

## The PIM is a second secret

The PIM is not stored in the volume.
Like the password, it must be supplied
at every mount — typed into the mount
dialog, or passed as `--pim` — and a
wrong PIM fails exactly like a wrong
password, with the same deliberately
unhelpful error (chapter 6). That has
two consequences:

- **It adds a real factor.** An attacker
  who somehow learns your password but
  not your PIM still cannot open the
  volume without finding the PIM too.
  The search space is small compared to
  a password's, but it multiplies the
  work of every attempt as well.
- **Forgetting it is fatal in the same
  way.** Password right, PIM forgotten:
  the box stays shut, and there is no
  recovery. If you use a non-default
  PIM, record it wherever you record
  the password — the pair travels
  together.

## The lab changed one, live

Chapter 5's volume was created with the
default. The lab then changed both its
password and its PIM in one operation
(`veracrypt-lab/t05-change-password-pim.txt`):

```bash
veracrypt --text --non-interactive --change vault.hc \
  --password <old> --new-password <new> --new-pim 505
```

```text
Password, PIM and/or keyfile(s) successfully changed.
```

and proved the new state the way
chapter 6 explains — by how mount
attempts fail:

- New password, **default PIM**: refused
  by the volume (the authentication
  error). Right password, wrong PIM —
  the volume cannot tell the difference,
  and neither can you.
- New password, **PIM 505**: accepted,
  and the attempt went on to the
  sandbox's missing-device error — the
  lab's signature of a successful
  unlock.

## Choosing a PIM

- **Leave it alone** if your password is
  long and unique. The default half
  million iterations is a serious price
  per guess, and every added secret is
  a thing to lose.
- **Raise it** when you want a shorter,
  more memorable password to carry more
  weight, when the volume will live
  somewhere hostile (a synced folder an
  attacker might copy), or when a
  multi-second unlock is a fair trade.
  Measure your own mount time; a PIM
  that makes *you* wait ten seconds
  makes a guessing rig wait ten seconds
  per guess too.
- **Never lower it below the default**
  to make mounting snappier. That is
  disarming the one mechanism that
  stands between a copied container and
  offline guessing.
- Note the floor VeraCrypt enforces:
  values that would drop the iteration
  count under the old TrueCrypt-era
  minimums for system volumes are
  rejected for boot volumes; for
  containers the program warns rather
  than forbids — the warning is the
  guide's advice too.
