# video-scrungler

A POSIX `sh` script and KDE Plasma Dolphin service menu entry to
transcode videos to a Web-safe[^1] format at a file size target.
This is useful for quick-and-dirty transcodes of social media video posts
that refuse to embed/preview/unfurl
on instant messaging apps such as Discord, Slack, or Teams.

## Installation

Copy the file `video-scrungler` to somewhere in PATH;
`$HOME/.local/bin` is likely a good place.

If you intend to use the Dolphin service menu entry,
*also* copy the file `video-scrungler.desktop`
to somewhere in the KDE Plasma service menu path;
`$HOME/.local/share/kio/servicemenus` is a good place to put it.
Make the directory if it doesn't already exist.

Both files must be marked executable.

## Usage

### Script

The script takes exactly 2 arguments,
the target file size (in binary megabytes)
and the video to transcode.
That is to say, to transcode `dingus.mkv`
to a target of 25 MiB:

```console
$ video-scrungler 25 dingus.mkv
```

Please note that non-integer file sizes are not accepted by the script.
This is partially a shell scripting limitation
and partially a warning that VBR rate control
is not usually accurate enough.

### KDE service menu

Right-click on a video
and it should appear in the context menu as "Transcode video for web",
with sub-menus

* "to 10 MB"

* "to 50 MB"

* "to 100 MB"

## Dependencies

### Script

* A POSIX `sh` shell and common tools.
  The following environments have been tested to work correctly:

    * GNU/Linux (Bash/coreutils)

    * Alpine Linux (ash/BusyBox)

    * Chimera Linux (FreeBSD userland)

* FFmpeg

### Dolphin service menu entry

* KDE Plasma 6 (I have no idea whether it works with 5)

## Limitations

This script is not magic.
Aiming for too low a bitrate
may either produce poor-quality output or
output larger than the target file size.
Consider using FFmpeg directly
if you find yourself needing more control over the output.

Obtaining a video file for this script to work with is out-of-scope.
For KDE Plasma users, consider
[Download with yt-dlp here](https://store.kde.org/p/2012539/).
Generically, use [yt-dlp](https://github.com/yt-dlp/yt-dlp/)
or [https://cobalt.tools/](cobalt.tools).

While the Dolphin service menu only has options for
target file sizes of 10 MiB, 50 MiB, and 100 MiB,
the script itself will accept any
positive integer value, denoted in binary megabytes.

[^1]: (specifically `yuv420p` High Profile H.264 video/AAC audio
      contained in MP4 with `-movflags faststart`)
