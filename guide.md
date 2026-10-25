---
layout: default
title: Install guide
permalink: /guide/
---

[Back to Barchan]({{ '/' | relative_url }})

# Download Barchan

Barchan is a viewer for HiRISE JPEG 2000 products that also opens PNG, TIFF, GeoTIFF, and JPEG images. This page
covers installing a released build. Each release also carries its [source code](#source-code), to build from instead.

## Requirements

- A Mac with **Apple Silicon** (M1 or later). Intel Macs are not supported.
- **macOS 15 or later.**

## Download

Releases are on the [releases page](https://github.com/venriq/barchan-releases/releases). The current release is
[v1.2.0](https://github.com/venriq/barchan-releases/releases/tag/v1.2.0), and these are the two of its files that
install it. What changed in each release is in its notes on the releases page.

| File | What it is |
|------|------------|
| [`Barchan-1.2.0-arm64.dmg`](https://github.com/venriq/barchan-releases/releases/download/v1.2.0/Barchan-1.2.0-arm64.dmg) | the disk image holding the application |
| [`Barchan-1.2.0-arm64.dmg.sha256`](https://github.com/venriq/barchan-releases/releases/download/v1.2.0/Barchan-1.2.0-arm64.dmg.sha256) | its SHA-256 checksum |

Download both into the same folder.

## Verify the download

The checksum file names the disk image beside it, so with both in `~/Downloads`:

```sh
cd ~/Downloads
shasum -a 256 -c Barchan-1.2.0-arm64.dmg.sha256
```

It should print:

```
Barchan-1.2.0-arm64.dmg: OK
```

Anything else means the download did not arrive intact, and it should not be opened.

Check against the `.sha256` published beside that exact disk image on its own release, and not against a digest
copied from anywhere else. No two builds produce an identical disk image, because the build time is written into the
application and `hdiutil` writes filesystem timestamps into the image, so a perfectly sound build of the same version
would disagree with a digest taken from a different one. A checksum here identifies one build, not one version.

## Install

Install the current release, [Barchan 1.2.0](https://github.com/venriq/barchan-releases/releases/download/v1.2.0/Barchan-1.2.0-arm64.dmg).
Every release from 1.0.1 on is signed with a Developer ID and notarized by Apple, so it installs in three steps.
Releases up to 1.0.0 came before that: macOS blocks them until they are approved twice in System Settings, and they
miss what later releases fixed. If you have one, install the current release over it instead of approving it.

### Releases from 1.0.1 on (recommended)

These releases are signed with a Developer ID belonging to **Fractional Vector LLC**, and Apple has notarized both the
disk image and the application, so macOS opens them like any other application from an identified developer:

1. Double-click the disk image. It opens to a window holding **Barchan** beside an **Applications** shortcut.
2. Drag **Barchan** onto the **Applications** shortcut, then eject the disk image.
3. Open **Barchan** from Applications.

The first time, macOS says that Barchan is an application downloaded from the internet and asks whether you are sure
you want to open it. That is the question macOS asks about any downloaded application; click **Open**. Nothing needs
approving in System Settings, and every later launch opens with no dialog. The disk image has nothing left in it and
can be deleted.

macOS checks the notarization against a ticket carried inside the disk image and the application, so this works
offline too. If macOS instead says Barchan **Not Opened** and that Apple could not verify it, the copy is not the one
that was published: check it against its checksum again, and download it afresh if it does not match.

### Releases up to 1.0.0

**Not recommended.** These releases are no longer on the releases page. [Download Barchan 1.2.0](#download)
instead. The steps below are kept for a copy of 1.0.0 or earlier that you already have and need to open.

<details markdown="1">
<summary>Show the steps for releases up to 1.0.0</summary>

Barchan 1.0.0 and the releases before it are signed, but ad-hoc rather than with a Developer ID, and they are not
notarized. macOS therefore refuses anything it has marked as downloaded until you approve it, and since macOS 15 the
right-click and choose Open shortcut no longer answers that. The approval goes through System Settings, and it is given
twice: once for the disk image, and once for the application copied out of it. The copy carries the image's mark but
not its approval, because macOS records an approval against one item, and the application is a separate item that is
checked again the first time it is opened.

1. Double-click the disk image. It does not open. A dialog says
   **"Barchan-1.0.0-arm64.dmg" Not Opened**, and that Apple could not verify it is free of malware.

   Click **Done**. Do not click **Move to Trash**, which is the blue button and the one the Return key presses; it
   deletes the download and you would have to fetch it again.

2. Open **System Settings**, then **Privacy & Security**, and scroll to the **Security** section. Just below the
   Gatekeeper setting it says **"Barchan-1.0.0-arm64.dmg" was blocked to protect your Mac**. Click
   **Open Anyway** beside it.

   The button appears only after macOS has blocked the image, and it does not wait indefinitely. If it is not there,
   double-click the disk image again to bring it back, then return here.

3. Authenticate with Touch ID or your password. The disk image opens.

4. Drag **Barchan** onto the **Applications** shortcut beside it, then eject the disk image.

5. Open **Barchan** from Applications. It does not open either. The same dialog appears, this time saying
   **"Barchan" Not Opened**. Click **Done** again, and again not **Move to Trash**.

6. Go back to **Privacy & Security**. The Security section now says **"Barchan" was blocked to protect your Mac**.
   Click **Open Anyway** beside it, then authenticate with Touch ID or your password. Barchan opens, with nothing
   further to confirm.

That is both approvals given, one for each item, and every later launch opens with no dialog at all. The disk image
has nothing left in it and can be deleted.

Step 5 and step 6 are there because the application is not notarized. If you prefer, removing the download mark from
the installed application instead does the same thing and skips both steps:

```sh
xattr -dr com.apple.quarantine /Applications/Barchan.app
```

`-r` is there because the mark is on every file in the application, not only on the application itself. Removing it
throws away a record macOS keeps on purpose, so it is worth doing only for a download already checked against the
checksum published beside it.

These steps were checked on macOS 15.7 and macOS 27.0. The wording moves between macOS releases, so on another
version the buttons may read a little differently while the path through System Settings stays the same.

</details>

## Opening products

Barchan opens JPEG 2000 products (`.jp2`), PNG, TIFF, GeoTIFF, and JPEG images. It tells them apart by their contents,
not by their names, and a file it cannot read gets a message naming the formats it does. A GeoTIFF's georeferencing
gives the readout latitude and longitude, as a HiRISE product's does. It also opens a PDS4 label, the `.xml` file that
describes a product in the HiRISE archive's newer `/PDS4/` tree, such as `ESP_094199_1825_RED.xml`: Barchan opens the
JPEG 2000 product the label lists beside it, rather than the much larger uncompressed `.tif` the label also describes,
and shows the label in the Metadata section. A browse label opens its browse image. The window and the recent
locations list keep the label's name.

There are several ways to open one:

- **File > Open** (Cmd+O) picks files on your Mac.
- **File > Open Location** (Cmd+L) takes a path or an `http` or `https` address, and reads the image over the network
  without downloading it first.
- Drag files from Finder onto any Barchan window.
- Drag a link to an image from a browser, such as Safari, Chrome, or Firefox, onto any Barchan window. It opens as
  File > Open Location would open it. While a file or a link is over a window, the window says **Drop to open**.
- In Finder, Control-click an image, or a PDS4 label's `.xml`, and choose Barchan under **Open With**.

A HiRISE product's label is a separate file of the same name: a PDS4 `.xml`, such as `ESP_094199_1825_RED.xml` for
`ESP_094199_1825_RED.JP2`, or a PDS3 `.LBL`, such as `ESP_043651_1590_RED.LBL` for `ESP_043651_1590_RED.JP2`. Download
one too and keep it in the same folder as the product, where Barchan looks for it, the `.xml` first. Without it the
product still opens, but the Metadata section has nothing to show and the readout gives raw values only, without the
calibrated one.

To read a HiRISE product over HTTPS, open its address as the HiRISE website or the archive gives it. The archive serves
its products under `https://hirise-pds.lpl.arizona.edu/PDS/` and, for the products described by PDS4 labels, under
`https://hirise-pds.lpl.arizona.edu/PDS4/`, which the website also links to as `https://www.uahirise.org/PDS4/`. It lets
through only about one request a second under `/PDS/`, while Barchan reads a remote product in many small requests, so
Barchan reads the same files through the archive's unthrottled `/download/PDS/` tree instead, and those under `/PDS4/`
or on `www.uahirise.org` through `/download/PDS4/`, whenever the two name the same version of a file, and keeps the
address you opened in the recent locations list. A label on the archive is found beside the product, with nothing to
download, and so is the reduced size `.QLOOK` product the navigator's overview may come from. For example,
`https://www.uahirise.org/PDS4/RDR/ESP/ORB_094100_094199/ESP_094199_1825/ESP_094199_1825_RED.xml` opens that
observation's RED product with its PDS4 label.

To measure distances, choose **Tools > Measure Distance** (Shift+Cmd+D) and click on the image to add each vertex of a
path. A double click or Return ends the path, Escape clears it, and dragging still pans. The **Measurement** section of
the sidebar lists the total, each leg's length and azimuth, and each vertex's position, and **Copy Measurement**, in
the menu a right-click opens on that section or on the path, copies it all as tab-separated text. On a map projected
product the lengths are in metres or kilometres on the body's surface, true anywhere in the image; on any other image
they are in pixels.

To see what Barchan is doing while it browses, choose **Tools > Show Activity** (Option+Cmd+A). The Activity window
shows, as figures and as sparklines of the last two minutes, the processor cores Barchan keeps busy, its decode threads
and what they wait for, the bytes and requests it reads over the network, what its caches hold and how often they serve
a lookup, the memory the GPU has allocated and how full each window's tile atlas is, and the memory Barchan has
allocated. It is sampled once a second while the window is open and not at all while it is closed, and it reopens where
it was at the next launch, so a slow view can be traced to the network, the decoder, or the caches.

Barchan registers as an alternate viewer for JPEG 2000, PNG, TIFF, and JPEG, and for the XML of a PDS4 label, so
Preview and your XML editor stay the default and a double-click still opens them, while Barchan appears under Finder's
**Open With** menu. To make Barchan the default
for one kind of file, select such a file in Finder, press Cmd+I, choose Barchan under **Open with**, then click
**Change All**.

## Source code

Barchan is free software, licensed under the GNU Affero General Public License, version 3 only (AGPL-3.0-only). Every
release carries the complete source of the build it offers as `Barchan-<version>-source.tar.gz` beside its disk image,
[`Barchan-1.2.0-source.tar.gz`](https://github.com/venriq/barchan-releases/releases/download/v1.2.0/Barchan-1.2.0-source.tar.gz)
for the current release. It holds the commit the disk image was built from, with the vendored Grok JPEG 2000 decoder,
also AGPL-3.0, and its submodules, at the commits that build used. The one submodule it leaves out is Grok's GPU
plugin, which Barchan never builds and whose repository is not public; the release's notes name it. Its `README.md`
covers building from source, and its `LICENSE` is the license's full text.

## Uninstall

Drag **Barchan** from Applications to the trash. It keeps its cache of downloaded image data in
`~/Library/Caches/Barchan`, which can be deleted too.
