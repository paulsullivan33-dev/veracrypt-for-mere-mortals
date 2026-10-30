# Installing VeraCrypt

**Outcome: VeraCrypt installed from the
project's own download, with the
download proven genuine before it runs.**

Encryption software is the worst
possible thing to install on trust. A
trojaned VeraCrypt would simply be
handed every password you type. So this
chapter's real subject is not the
install — five minutes of package
management — but the **verification
chain** that comes first. The lab ran
every link of it; the transcript is
`veracrypt-lab/t00-download-verify.txt`.

## Get it from the project, nowhere else

Downloads live on the project's site,
`veracrypt.io`, under Downloads. The
files themselves are served from the
project's GitHub releases. Do not take
VeraCrypt from a download portal, a
magazine cover disc, or a package
repository you cannot vouch for —
unofficial builds of encryption tools
are a classic hiding place for malware.

For Linux the project ships:

- Ready packages for Ubuntu and Debian
  (and RPMs for CentOS-family systems),
  in two flavours: the full **graphical**
  package (`veracrypt-…`) and the smaller
  **console** package
  (`veracrypt-console-…`), which is the
  text-only face this guide drives.
- A generic installer for other
  distributions.

The lab used
`veracrypt-console-1.26.30-Ubuntu-24.04-amd64.deb` —
VeraCrypt **1.26.30**, the current
release as the lab ran. On Windows and
macOS, take the installer / disk image
from the same page; the verification
below has equivalents in the project's
documentation for those systems.

## The verification chain

Alongside each release the project
publishes three things: a **SHA-256
sums file** listing the fingerprint of
every file, a **PGP signature** for that
sums file and for each package, and the
project's **PGP public key**, with its
fingerprint printed on the project's
Digital Signatures page. The chain of
checks:

1. **Import the project key and compare
   its fingerprint with the one printed
   on veracrypt.io.** The lab's key
   reported fingerprint
   `5069 A233 D55A 0EEB 174A 5FC3 821A CD02 680D 16DE`,
   matching the site exactly. This step
   is the anchor: a signature is only
   worth what the key is worth, and the
   key is only worth trusting because
   two independent pages of the project
   agree on its fingerprint.
2. **Verify the signature on the sums
   file.** `gpg --verify` answered
   *Good signature from "VeraCrypt Team"*.
3. **Check the package against the
   signed sums.** `sha256sum -c` answered
   `OK` for the console package.
4. **Verify the package's own
   signature** as a second lock — also
   *Good signature*.

GPG adds a warning worth understanding
rather than fearing: *"This key is not
certified with a trusted signature!"*
That means only that nobody in your own
web of trust has vouched for the key —
expected for a key you fetched ten
minutes ago. The fingerprint comparison
in step 1 is the vouching you did
yourself.

One honest note about the lab: rather
than installing the package system-wide
on its shared machine, the lab unpacked
the `.deb` into a folder (`dpkg-deb -x`)
and ran the binary from there. The
unpack showed exactly what an install
would place: the program, its mount
helper, and the full official user guide
as local HTML — a useful thing to know
exists on your own machine after a real
install. A normal install is simply:

```bash
sudo apt install ./veracrypt-console-1.26.30-Ubuntu-24.04-amd64.deb
```

(or the graphical package's filename).
The package declares three dependencies —
`libfuse2t64`, `dmsetup` and `sudo` —
which the package manager fetches.
Keep those names in mind: FUSE and the
device mapper are the machinery of
mounting, and chapter 6 meets them
again.

## Prove it runs

```bash
$ veracrypt --text --version
VeraCrypt 1.26.30
```

and `veracrypt --text --help` prints the
full command surface — the lab's copy
is `veracrypt-lab/t01-version-and-help.txt`.
Skim it once. It is the map of
everything the console face can do, and
this guide's later chapters are mostly
a guided tour of its entries: `--create`,
`--change`, `--backup-headers`,
`--restore-headers`, `--create-keyfile`,
`--test`.
