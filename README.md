# netzkino-dl

*Deutsch | [English](README.en.md)*

Filme von **Netzkino.de** (kostenloser AVOD-Stream) per Kommandozeile
herunterladen — **immer maximale Qualität**, Ergebnis ist eine **MKV**
(verlustfreies Remux, Metadaten + Cover eingebettet).

## Windows-EXE (ohne Installation)

Von [Releases](https://github.com/FLOKHOME/netzkino-dl/releases) die aktuelle
`netzkino-windows.exe` laden und direkt starten — **yt-dlp und ffmpeg sind
bereits eingebaut** (kein Python, kein pip, kein ffmpeg nötig).

> Hinweis: Die EXE ist nicht signiert — beim ersten Start meldet Windows
> SmartScreen „Unbekannter Herausgeber". Über „Weitere Informationen →
> Trotzdem ausführen" starten. Zusätzlich wird `netzkino-linux` mitgebuildet;
> macOS-Nutzer starten das Tool aus dem Quellcode.

## Kommandos

### Suche
```bash
netzkino-dl search "mondmann"
netzkino-dl search "krieg"
```

### Details anzeigen (Qualität, Größe, DRM)
```bash
netzkino-dl info man-on-the-moon
netzkino-dl info https://www.netzkino.de/details/man-on-the-moon?ref=shelfd.com
```

### Download
```bash
netzkino-dl dl man-on-the-moon
netzkino-dl dl https://www.netzkino.de/details/man-on-the-moon
netzkino-dl dl man-on-the-moon --out ~/Videos/netzkino

# Qualitätscheck: nur die ersten 60 Sekunden
netzkino-dl dl man-on-the-moon --test

# Bitraten-Prüfung überspringen (schneller, nimmt die CMS-URL)
netzkino-dl dl man-on-the-moon --no-sub
```

## Optionen

| Option | Bedeutung |
|---|---|
| `--out DIR` | Zielordner (Standard: `~/Videos/netzkino`, Windows: `%USERPROFILE%\Videos\netzkino`) |
| `--test` | nur die ersten 60 s laden |
| `--no-sub` | keine Prüfung höherer Bitraten |

## Wie es funktioniert

1. **Suche/Details**: direkte Abfrage der Netzkino-GraphQL-API
   (`data.netzkino.de/netzkino/graphql`) — Titel (DE+Original), Jahr, FSK,
   Laufzeit, DRM-Status, Lizenzende.
2. **Maximale Qualität**: die CMS-Stream-URL (z. B. `Der_Mondmann_3000000.mp4`)
   wird per HEAD-Request gegen höhere Bitraten (10M/8M/6M/5M/4M/3,5M)
   geprüft; die beste erreichbare Variante wird genommen (meist 3 Mbit/s
   ≈ 18 GB pro Spielfilm).
3. **Download**: yt-dlp lädt die MP4 (HTTP, resumable) — typischerweise
   25–30 MB/s — und remuxt **verlustfrei nach MKV**, bettet Metadaten und
   das Cover ein.
4. **DRM-geschützte Titel** werden erkannt und mit klarer Meldung
   abgelehnt (die meisten Netzkino-Titel sind DRM-frei).

## Voraussetzungen (nur bei Skript-Nutzung)

- Python 3 (kein pip nötig) — die EXE braucht nichts davon
- **ffmpeg** im PATH (Remux nach MKV)
- yt-dlp wird automatisch von PyPI geladen (`~/arte-dl/lib`)

## Haftungshinweis

Nur für Filme nutzen, die Netzkino legal und kostenlos anbietet.
Private Nutzung — keine Weiterverbreitung der heruntergeladenen Filme.
