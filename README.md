# Aircraft Lookup

A small Windows desktop app that uses the [FlightAware AeroAPI](https://www.flightaware.com/aeroapi/portal/) to look up an aircraft by its registration (tail number) and show:

- the **current flight**: flight ID, route, aircraft type and registration, and position
- the **previous three flights**: flight ID, route, aircraft type and registration
- a **world map** with the aircraft's current position marked on it

Example output:

```
Current flight
  Flight ID:  QFA1
  Route:      SYD - SIN
  Aircraft:   A388  VH-OQA
  Position:   25 00N 125 30E

Previous flights
  1. QFA2      SIN - SYD   A388  VH-OQA
  2. QFA1      SYD - SIN   A388  VH-OQA
  3. QFA2      SIN - SYD   A388  VH-OQA
```

This is an independent hobby project. It is not affiliated with or endorsed by FlightAware.

---

## What you need

1. **A FlightAware AeroAPI key.** Sign up at the [AeroAPI portal](https://www.flightaware.com/aeroapi/portal/) and create an API key. Check which plan you are on, as it affects how much history is available and how requests are billed.
2. **Windows 10 or 11.**
3. **An internet connection.** It is needed for AeroAPI, and once, on first run, to download the map data.

---

## Quick start

### 1. Prepare the app

Extract all files from the zip.

Copy the `.exe` anywhere you like.

### 2. Add your API key

The app does not ask for your key in the window. Instead, copy `api_key.txt` containing only your key on one line, with no quotes or extra text. Put it in **either** of these places:

| Next to the `.exe` | Easiest. Keeps the key with the program. |
| `%APPDATA%\AircraftLookup\api_key.txt` | Per-user location. You may need to create the `AircraftLookup` folder. |

The app checks those locations in that order. As a fallback it also reads the `AEROAPI_KEY` environment variable.

If no key is found, the window tells you exactly which locations it checked, and the **Look up** button stays disabled. Add the key and restart the app.

### 3. Run it

Double-click the `.exe` .

---

## Using the app

1. Type an aircraft registration into the **Registration** box, e.g. `VH-OQA`. Case and hyphens don't matter, so `vhoqa` works too.
2. Press **Enter** or click **Look up**.
3. Read the results, and find the aircraft on the map at the bottom of the window. The red marker is labelled with the flight ID.

**What the results mean**

- **Current flight** is a flight that has departed but not yet landed. If the aircraft isn't airborne, the app shows "Not currently airborne" and no marker appears on the map.
- **Previous flights** are the three most recently landed flights, newest first. Cancelled flights are skipped.
- **Position** is shown as degrees and minutes, e.g. `25 00N 125 30E`.
- Flight IDs are shown in ICAO form (`QFA1`, not `QF1`).

---

## The map

On first run the app downloads a world coastline file (Natural Earth 1:110m land, public domain) and saves it to:

```
%APPDATA%\AircraftLookup\land.geojson
```

After that the map loads from disk and works offline. While it loads, a "Loading map..." note appears under the map.

If the download fails (for example behind a firewall), the note under the map says so. To fix it manually, download `ne_110m_land.geojson` from the [Natural Earth vector data on GitHub](https://github.com/nvkelso/natural-earth-vector) (in the `geojson` folder) and save it as `land.geojson` at the path above.

The map covers 80°N to 60°S, so Antarctica is cropped off.

---

## Troubleshooting

| Problem | What to try |
|---|---|
| "No API key found" in red | Create `api_key.txt` as described above and restart the app. |
| Error mentioning `401` or `403` | The key is wrong, expired, or your AeroAPI plan doesn't include the endpoint being called. Check the key and your plan in the AeroAPI portal. |
| Error mentioning `429` | You have hit your request rate limit. Wait a minute and try again. |
| "Not currently airborne" | The aircraft has no flight in progress. The previous flights are still shown. |
| Position says "unavailable" | The flight is active but no position reports have come back yet. This is common just after departure or in areas with poor coverage. Try again shortly. |
| Fewer than three previous flights | AeroAPI only returns a limited recent window (roughly the last 10 days on many plans). A rarely used aircraft may not have three flights in that time. |
| Map is blank with a message underneath | See [The map](#the-map) above. |
| Registration not found | Check the spelling. Some registrations have unusual formats, so try without any hyphen. |

---

## Costs

AeroAPI bills per request. A typical lookup uses one request for the flight list, plus one more when the current position isn't included in that response. Be aware of this if you look up many aircraft.

---

## Credits

- Flight data: [FlightAware AeroAPI](https://www.flightaware.com/aeroapi/)
- Coastline data: [Natural Earth](https://www.naturalearthdata.com/) (public domain)
- The aircraft icon is an original design and is not FlightAware's logo.
