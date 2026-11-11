# qgroundcontrol-CI-dependencies

Mirror of third-party packages that the Arace-GroundControl CI builds download.

The builds used to fetch GStreamer straight from gstreamer.freedesktop.org. That server is
often very slow, and the downloads timed out and failed the builds (November 2026). The
workflows now download from this repository first and fall back to the original server.

## Where the files are

The packages are too large for git, so they are stored as assets of a release, not in the
repository tree.

Release `gstreamer`:

| File | Used by | Original location |
|---|---|---|
| `gstreamer-1.0-android-universal-1.18.6.tar.xz` | Android, Skydroid H16 and G16 builds | `https://gstreamer.freedesktop.org/data/pkg/android/1.18.6/` |
| `gstreamer-1.0-android-universal-1.16.3.tar.bz2` | "GST 1.16.3 Skydroid H16" builds | `https://gstreamer.freedesktop.org/data/pkg/android/1.16.3/` |
| `gstreamer-1.0-msvc-x86_64-1.22.12.msi` | Windows builds | `https://gstreamer.freedesktop.org/data/pkg/windows/1.22.12/msvc/` |
| `gstreamer-1.0-devel-msvc-x86_64-1.22.12.msi` | Windows builds | `https://gstreamer.freedesktop.org/data/pkg/windows/1.22.12/msvc/` |

Download URL pattern:

```
https://github.com/ARACE-Kft/qgroundcontrol-CI-dependencies/releases/download/gstreamer/<file>
```

The files are unmodified copies of the official GStreamer binaries. `SHA256SUMS` lists the
checksums published by the GStreamer project, and the workflows verify every download
against them. To check a file by hand:

```
sha256sum -c SHA256SUMS --ignore-missing
```

## Adding or updating a package

1. Download the file from its original location and check it against the checksum published
   next to it (`<file>.sha256sum`).
2. Upload it to the release: `gh release upload gstreamer <file> --repo ARACE-Kft/qgroundcontrol-CI-dependencies`
3. Add its checksum to `SHA256SUMS` and a row to the table above.
4. Update the file name and checksum in the Arace-GroundControl workflows (`.github/workflows/`).

Do not replace a file under the same name with different content: the workflows would fail
the checksum. Do not delete a file while a maintained branch still builds with it.

## License

GStreamer is distributed under the LGPL. Source code and license texts are available from
https://gstreamer.freedesktop.org/.
