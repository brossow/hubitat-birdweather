# BirdWeather PUC — Hubitat Driver

If you have a [BirdWeather PUC](https://www.birdweather.com) station, this driver integrates it with Hubitat Elevation — exposing live bird detections as device attributes and events so you can use them in automations and dashboards.

---

## What it does

The driver polls the BirdWeather API on a configurable schedule and keeps a set of attributes current:

- **Latest detection** — common name, scientific name, confidence %, certainty level, time, species photo URL, and audio clip URL
- **Today's summary** — unique species count, total detection count, top species and its detection count, and a full JSON list of every species seen today
- **All-time totals** — cumulative species and detection counts
- **Trigger events** — `birdDetected` fires on every new detection; `newSpeciesDetected` fires the first time a species is seen each day (resets at midnight)

---

## Automation ideas

**Announce every detection on a speaker:**
> Rule Machine → Trigger: `birdDetected` changes → Speak "%lastSpecies% detected in the backyard"

**Push notification for a new species:**
> Rule Machine → Trigger: `newSpeciesDetected` changes → Send push "%value% (%lastSpeciesScientific%)"

**Flash a light on a high-confidence sighting:**
> Rule Machine → Trigger: `birdDetected` changes → Condition: `lastCertainty` = `Almost Certain` → Flash light 3×

**Dashboard tile:**
Add `lastSpecies`, `todaySpecies`, and `todayDetections` as Attribute tiles. Or use Tile Builder Grid for a richer display with the species photo, detection time, and today's stats in a single tile.

---

## Installation

**Via Hubitat Package Manager (recommended):**
Search HPM for "BirdWeather PUC" and install.

**Manual:**
1. Hubitat → **Drivers Code → New Driver** → paste `birdweather-puc.groovy` → Save
2. **Devices → Add Device → Virtual** → select **BirdWeather PUC**
3. Open the device, enter your **Station ID** in Preferences → Save
4. Click **Refresh** once — scheduled polling starts automatically

### Finding your Station ID

The Station ID is the number in the URL when viewing your station at [app.birdweather.com](https://app.birdweather.com) — e.g. `app.birdweather.com/stations/25574` → ID is `25574`.

### API Token

Only needed for **private stations**. Leave blank for public stations. The token is found under Advanced Settings in the BirdWeather app.

---

## Preferences

| Setting | Description |
|---------|-------------|
| Station ID | Numeric ID from your station URL |
| API Token | Optional — private stations only |
| Poll Interval | 1, 2, 5, 10, 15, or 30 minutes |
| Recent Detections to Track | Depth of the `recentDetections` JSON history (3–20) |
| Minimum Confidence % | Ignore detections below this threshold |
| Fire events only for certainty level ≥ | Filter events by certainty: all / very_likely / almost_certain |
| Pause polling at night | Skip polls between sunset and sunrise |
| Enable Debug Logging | Verbose logging in the hub log viewer |

---

## Links

- **GitHub:** https://github.com/brossow/hubitat-birdweather
- **HPM:** search "BirdWeather PUC"

Feedback and bug reports welcome — either here or as a GitHub issue.
