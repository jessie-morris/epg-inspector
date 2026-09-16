# EPG Inspector

A single-file, local-only tool for debugging XMLTV guide feeds — built out of chasing down a "no information" bug in [ErsatzTV](https://github.com/ErsatzTV/legacy), but useful for any XMLTV-backed EPG (Plex, Jellyfin Live TV, Tvheadend, etc.).

It renders a raw XMLTV feed the way a real guide client would — a scrollable, cable-box-style grid with channel logos, a live "now" line, and season/episode detail on click — and automatically flags the two things that don't show up until a client is already choking on them:

- **Blank titles** — a `<programme>` entry with start/stop times but no `<title>`, which is exactly what makes a guide show "no information"
- **Schedule gaps** — stretches of a channel's timeline with no programme data at all

## Usage

Just open `epg-inspector.html` in a browser. No build step, no dependencies, nothing to install.

1. It boots with a small example feed so you can see how it renders immediately.
2. Paste your own feed's XML into the text box (or try "Fetch from URL" — this works well from a local file, but will usually get blocked if the page itself is served over `https://` while your guide server is plain `http://`, which most self-hosted setups are).
3. Click "Parse & render."
4. Click any programme block to see its full detail — show name, episode title, season/episode number, description, and the raw XMLTV timestamps.
5. Scroll right for a 24+ hour forward view of the schedule.

Anything flagged as an anomaly (blank title or gap) is also listed in the panel below the grid, with the exact time range and channel it affects.

## Why it exists

Diagnosing a broken guide against a live client (a TV, a media server's Live TV page) means the bug is only visible after the fact, mixed in with buffering, caching, and rendering behavior that has nothing to do with the actual feed. This tool parses the feed directly and shows you exactly what's in it — nothing cached, nothing client-side to second-guess.

## Notes

- Runs entirely client-side. Nothing is uploaded anywhere.
- Channel logos are fetched and their real file signature is sniffed byte-for-byte rather than trusting the server's declared `Content-Type` — some guide servers (including ErsatzTV) mislabel WebP logos as `audio/x-wav`, since WebP and WAV share the same RIFF container prefix.
