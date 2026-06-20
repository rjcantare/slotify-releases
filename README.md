# Slotify — OTA update host

Hosts Slotify's over-the-air update artifacts so shop devices update from a
**stable internet URL**, independent of any shop's local server IP.

- **Binaries** are attached to GitHub **Releases** (one tag per version):
  `RentalServer.exe`, `RentalClient.Windows.exe`, `RentalAndroid.apk`.
- **`update.json`** (this repo, `main`) is the manifest: latest version + each
  artifact's download URL + SHA-256. Stable raw URL:
  `https://raw.githubusercontent.com/rjcantare/slotify-releases/main/update.json`

## Flow
1. Publish from anywhere: `scripts/publish-online.ps1` (in the RentalSystem repo)
   builds the artifacts, creates a GitHub Release, and updates `update.json`.
2. The **shop server** polls `update.json` every few minutes and re-publishes the
   `rental/config/update` message to its **local** MQTT broker with these URLs.
3. PCs and tablets download from the GitHub URLs and **verify the SHA-256** before
   installing — a tampered file never applies.

No source is stored here — binaries + manifest only. Public so devices fetch
without a token; integrity is guaranteed by the per-file hash in `update.json`.
