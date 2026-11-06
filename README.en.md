# netzkino-dl

*[Deutsch](README.md) | English*

Download movies from **Netzkino.de** (free AVOD streaming) from the command
line — **always maximum quality**, output is an **MKV** (lossless remux with
embedded metadata and cover art).

## Windows EXE (no installation)

Grab the latest `netzkino-windows.exe` from the
[Releases](https://github.com/FLOKHOME/netzkino-dl/releases) page and run it —
**yt-dlp and ffmpeg are bundled** (no Python, no pip, no ffmpeg needed).

> Note: The EXE is unsigned — Windows SmartScreen will warn on first start
> ("Unknown publisher"). Click "More info → Run anyway". A `netzkino-linux`
> binary is built as well; macOS users can run the tool from source.

## Commands

### Search
```bash
netzkino-dl search "mondmann"
netzkino-dl search "krieg"
```

### Show details (quality, size, DRM)
```bash
netzkino-dl info man-on-the-moon
netzkino-dl info https://www.netzkino.de/details/man-on-the-moon?ref=shelfd.com
```

### Download
```bash
netzkino-dl dl man-on-the-moon
netzkino-dl dl https://www.netzkino.de/details/man-on-the-moon
netzkino-dl dl man-on-the-moon --out ~/Videos/netzkino

# Quality check: only the first 60 seconds
netzkino-dl dl man-on-the-moon --test

# Skip bitrate probing (faster, uses the CMS URL as-is)
netzkino-dl dl man-on-the-moon --no-sub
```

## Options

| Option | Meaning |
|---|---|
| `--out DIR` | target folder (default: `~/Videos/netzkino`) |
| `--test` | download only the first 60 seconds |
| `--no-sub` | skip probing for higher bitrates |

## How it works

1. **Search/details**: direct queries against the Netzkino GraphQL API
   (`data.netzkino.de/netzkino/graphql`) — title (DE+original), year, FSK
   rating, runtime, DRM status, license end.
2. **Maximum quality**: the CMS stream URL (e.g. `Der_Mondmann_3000000.mp4`)
   is HEAD-probed against higher bitrates (10M/8M/6M/5M/4M/3.5M); the best
   reachable variant is used (usually 3 Mbit/s ≈ 18 GB per feature film).
3. **Download**: yt-dlp fetches the MP4 over HTTP (resumable), typically at
   25–30 MB/s, then **losslessly remuxes to MKV**, embedding metadata and
   cover art.
4. **DRM-protected titles** are detected and rejected with a clear message
   (most Netzkino titles are DRM-free).

## Requirements (script usage only)

- Python 3 (no pip needed) — the EXE needs none of this
- **ffmpeg** on PATH (remux to MKV)
- yt-dlp is auto-fetched from PyPI

## Disclaimer

Only use for films Netzkino offers legally and for free. Private use only —
do not redistribute downloaded movies.
