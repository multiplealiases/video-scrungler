# video-scrungler

A POSIX `sh` script and KDE Plasma Dolphin service menu entry to
transcode videos to a Web-safe[^1] format at a file size target.
This is useful for quick-and-dirty transcodes of social media video posts
that refuse to embed/preview/unfurl
on instant messaging apps such as Discord, Slack, or Teams.

Also available in 10-bit AV1[^2] flavor!
You'll want the scripts `video-scrungler-av1`
and the servicemenu `video-scrungler-av1.desktop`.
Install and usage are identical to the H.264 counterpart.

## Installation

Copy the file `video-scrungler` to somewhere in PATH;
`$HOME/.local/bin` is likely usable.

If you intend to use the Dolphin service menu entry,
*also* copy the file `video-scrungler.desktop`
to somewhere in the KDE Plasma service menu path;
`$HOME/.local/share/kio/servicemenus` will probably work.
Make the directory if it doesn't already exist.

Both files must be marked executable.

It is possible that KDE Plasma will say that it can't find
`video-scrungler` when you go to run it from the service menu entry.
Either place `video-scrungler` at `/usr/local/bin`, or
write this script to
`$HOME/.config/plasma-workspace/env/path-local-bin.sh`
and restart the Plasma session:

```
export PATH=$PATH:$HOME/.local/bin
```

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
with sub-menus denoting the target file size.

## Dependencies

### Script

* A POSIX `sh` shell and common tools.
  The following environments should work (file an issue if it doesn't):

    * GNU/Linux (Bash/coreutils)

    * Alpine Linux (ash/BusyBox)

    * Chimera Linux (FreeBSD userland)

* FFmpeg 4.4.6+ with

    * `libx264` and `aac` for `video-scrungler`, or

    * `libsvtav1` and `libopus` for `video-scrungler-av1`

    * (and decoders and demuxers for your input files,
       which I'd hope your FFmpeg has.)

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
or [cobalt.tools](https://cobalt.tools).

The Dolphin service menu only has options for
target file sizes of 10 MiB, 50 MiB, and 100 MiB.
I believe this should be fine,
as it reflects typical upload limits for video files
on instant messaging services.
The script itself will accept any
positive integer value, denoted in binary megabytes.

The service menu does not display progress nor
display any indication that it is working,
and I'm not sure how best to solve it.

I also had to lie a bit and write "99 MB" in the submenu
even though it actually runs a file size target of 100 MB.
This is because service submenus are sorted ASCIIbetically.

[^1]: Specifically `yuv420p` High Profile H.264 video/AAC audio
      contained in MP4 with `-movflags faststart`.

[^2]: `yuv420p10le` AV1 video/Opus audio,
      contained in MP4 with `-movflags faststart`.
      To my knowledge, this is compatible with the recent
      (as of this writing) Discord update that added AV1 video embed support.
