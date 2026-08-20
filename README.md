## High-Resolution Map Generator

This is a web-based tool for generating and exporting 100x100 map tiles from high-resolution map providers, designed for RC pilots and hobbyists using **b14ckyy ETHOS Mapping Widget** or **Yaapu mapping widgets** on their radios. It also supports generating **Terrain** elevation data files for INAV flight controllers (**.TER**, the default) or ArduPilot (**.DAT**), from either **Re:Earth** (default) or **NASA SRTM** elevation data. It enables quick, straightforward operation by selection of a flying area and direct download of compatible map tiles or terrain files to your SD card or as a ZIP file for later transfer.

⚠️ **You can use the tool online without downloading** by visiting: [https://martinovem.github.io/High-Resolution-Map-Generator/](https://martinovem.github.io/High-Resolution-Map-Generator/)

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/fe00dd94-3e69-469e-9afa-52c2fe8a25a2" />


---

### Supported Map Providers and Types

- **Providers:**
  - OSM (OpenStreetMap, standard map, non high-res)
  - ESRI (High-Res Satellite and Street)
  - Google (High-Res Satellite, Street, Hybrid)
  - MapTiler (High-Res Satellite, Street, Hybrid, Outdoor, Topo, Winter — needs a free API key, entered once)

- **Map Types:**
  - Street
  - Satellite
  - Hybrid (Satellite + Labels, where available)
  - Outdoor, Topo, Winter (MapTiler only)

---

### Key Features

- **Web-based, No Installation:** Download and open the HTML file in your browser.
- **Area Selection:** Draw a rectangle on the map to select your area of interest.
- **Editing Tools:** Edit or clear the selected area easily.
- **Measurement Tool:** Measure distances directly on the map.
- **Full Screen Mode:** Expand the map for easier area selection.
- **Mobile Device Compatibility:** Responsive design for phones and tablets. Desktop version auto-hides left-sided panel in monitors smaller than 13".
- **Settings Memory:** Option to remember your settings for future sessions.
- **Restore Defaults:** Quickly reset all settings to defaults.
- **Zoom Adjustment:** Set minimum and maximum zoom levels for tile export. Range 1-20.
- **Search Box:** Find your flying field or any location by name.
- **Geolocation:** Center the map on your current location (browser permission required, session limited to 10 seconds for security).
- **Output Targets:**
	- **[b14ckyy ETHOS Mapping Widget](https://github.com/b14ckyy/ETHOSMappingWidget-Revisited):**
	 - Output format: JPEG 70% quality
	 - Output path: `/bitmaps/ethosmaps/maps/{Provider}/{MapType}/{Zoom}/{X or Y}/{Y or X}.jpg`
	- **[Yaapu - ETHOS](https://github.com/yaapu/EthosMappingWidget/tree/main):**
	 - Output format: JPEG
	 - Output path: `/bitmaps/yaapu/maps/{GoogleMapType}/{Zoom}/{Y}/s_{X}.jpg`
	- **[Yaapu - EdgeTX](https://github.com/yaapu/HorusMappingWidget):**
	 - Output format: JPEG
	 - Output path: `/IMAGES/yaapu/maps/{GoogleMapType}/{Zoom}/{Y}/s_{X}.jpg`
	 - **Note:** For both Yaapu sub-targets, regardless of the selected provider (including ESRI or OSM), the output folder will always use the "Google" naming convention (`GoogleMap`, `GoogleSatelliteMap`, or `GoogleHybridMap`). This is required for compatibility with the Yaapu widget. For example, if you select ESRI as the provider and Satellite as the map type, the output will still be placed in `.../GoogleSatelliteMap/...` even though the tiles are from ESRI. This applies equally to Yaapu - ETHOS (`/bitmaps/yaapu/maps/...`) and Yaapu - EdgeTX (`/IMAGES/yaapu/maps/...`).
	- **[Terrain](https://github.com/iNavFlight/inav/wiki/Navigation-Terrain-Following):** `.TER` (INAV Terrain, default) or `.DAT` (ArduPilot) elevation files for the flight controller SD card — see the dedicated **Terrain** section below.
- **Direct SD Card Sync:** If your browser supports the File System Access API (Chrome; Edge), write tiles directly to your SD card.
- **ZIP Download Fallback:** If SD card access is not available, the tool will offer a ZIP download for manual transfer.

---

## Terrain

Generate terrain elevation files at 1 arc-second (~30 m) resolution. The files go on the flight controller's SD card and enable terrain altitude reference in the OSD and for terrain-following. Two output formats and two elevation sources are selectable.

**Output format**

| Format | For | Notes |
|---|---|---|
| **`.TER` (INAV Terrain)** — default | INAV (10+) | 28×32 heights packed as 10-bit offsets in 2 m steps above a per-block base |
| `.DAT` (ArduPilot) | ArduPilot | 28×32 int16 heights; the ArduPilot-compatible format |

**Elevation source**

| Source | Coverage | Notes |
|---|---|---|
| **Re:Earth Terrain** (default) | global, incl. >60°N | [Re:Earth](https://terrain.reearth.land/) · Mapterhorn (CC BY 4.0); Copernicus-based, more accurate on bare earth |
| NASA / Mapzen SRTM1 | 60°N – 56°S | [NASA SRTM1](https://www.usgs.gov/centers/eros/science/usgs-eros-archive-digital-elevation-shuttle-radar-topography-mission-srtm-1), public domain |

**Details**

| Detail | Value |
|---|---|
| **Output format** | `.TER` (INAV Terrain, default) or `.DAT` (ArduPilot) — selectable in the tool |
| **Data source** | Re:Earth (default) or NASA / Mapzen SRTM1 — selectable in the tool |
| **Resolution** | 1 arc-second (~30 m) |
| **Output** | One file per 1°×1° grid square (~111 km), e.g. `N42E023.TER` |
| **Output location** | FC SD card root (e.g. `G:\N42E023.TER`) |
| **File size** | ~30 MB per tile |
| **Format** | 2048-byte blocks with CRC-16/XMODEM checksums; `.TER` packs 28×32 heights as 10-bit offsets in 2 m steps above a per-block base, `.DAT` stores 28×32 int16 heights |
| **Ocean depths** | clamped to 0 m |

### Terrain elevation accuracy

The elevation sources were validated against **ICESat-2** — NASA's satellite laser altimeter (~10–15 cm accurate), the reference the scientific literature uses. Sampled at the ICESat-2 bare-earth laser points over test regions in Bulgaria, the sources rank by mean absolute error:

- **Re:Earth** (default) — **~1.1 m**
- **Copernicus GLO-30** — **~1.1 m** (on par with Re:Earth; used by native tools such as the INAV Configurator, not available to this browser tool)
- **SRTM / Mapzen** — **~2.0 m**
- **JAXA / ALOS** (the ArduPilot terrain server's source) — **~3.0 m**

So **Re:Earth is roughly twice as accurate as SRTM** on bare ground and is the most accurate source this tool can use. Differences are small in absolute terms (1–3 m); all sources are usable for terrain-following clearance, but Re:Earth (default) is the best available. Accuracy varies with terrain, vegetation and snow — these figures are for the tested mid-latitude regions, not a global guarantee.

<!-- Forest plot: DEM source accuracy vs ICESat-2 — add the chart image here -->

---

## How to Use

⚠️ **You can use the tool online without downloading** by visiting: [https://martinovem.github.io/High-Resolution-Map-Generator/](https://martinovem.github.io/High-Resolution-Map-Generator/)

1. **Download the Tool**
	- Go to the [GitHub repository](https://github.com/MartinovEm/High-Resolution-Map-Generator).
	- Click the green “Code” button and select “Download ZIP”.
	- Extract the ZIP and open `HighResMapDownloader.html` in your browser, or simple double-click.

2. **Link Your SD Card**
	- (Optional) Click "Link SD Card" (or "Link FC SD Card" for INAV Terrain) to connect your SD card for direct export.

3. **Select Output Target**
	- Choose the widget type based on the Lua script installed on your radio (**b14ckyy** or **Yaapu**), or select **Terrain** to generate elevation data for your flight controller.

4. **Set Project Name**

5. **Set Provider and Map Type**
	- For b14ckyy and Yaapu targets, select the map provider and type.
	- If you choose **MapTiler**, paste your free MapTiler API key into the box that appears — you only need to enter it once. Get a free key at [maptiler.com](https://www.maptiler.com/).
	- For **Terrain**, use the **Provider** picker to choose the elevation source — **Re:Earth** (default) or **Mapzen (NASA) SRTM** — and the **Output Format** picker to choose **.TER (INAV Terrain)** (default) or **.DAT (ArduPilot)**. The map is automatically set to OSM (map tiles are not exported — only elevation data).

6. **Set Zoom Levels**
	- Adjust the minimum and maximum zoom levels for the tiles exported. Range 1-20.
	- Not applicable for Terrain — elevation resolution is ~30 m regardless of the selected source.

7. **Find Your Flying Field**
	- Use the search box to locate your field, or pan/zoom manually. Optional zoom-lock feature.

8. **Select Area of Interest**
	- Draw a rectangle on the map to define the area for tile download.
	- Right-mouse click deletes the rectangle drawn. Second right-mouse click disengages the drawing tool.

	<img width="1364" height="768" alt="image" src="https://github.com/user-attachments/assets/7e9fd1f6-2f49-4fc1-85fc-4c82db1c14c4" />

	<img width="2535" height="1394" alt="terrain_overview" src="https://github.com/user-attachments/assets/83fb24c0-e9b5-4e15-a4b1-1e9093865913" />





9. **Edit Area (Optional)**

10. **Sync or Download**
	 - **For b14ckyy / Yaapu:** Click "Sync to SD Card" to export tiles. If SD card is not connected, a ZIP download will be offered.

	 <img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/4f859e99-b11d-4cb8-afb0-a8634ccf848f" />


	 <img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/0ffde6b7-4b6e-43f0-9975-ed1528dccd2a" />

	 - **For Terrain:** Click "Sync to SD Card" to generate the terrain files in the selected **Output Format** (`.TER` or `.DAT`). The tool downloads elevation data from the selected source (Re:Earth or SRTM), converts it, and writes the files directly to the flight controller SD card root. If no SD card is linked, a ZIP file (`INAV_Terrain_ReEarth.zip` or `INAV_Terrain_SRTM.zip`) will be offered for download. A 1°×1° grid overlay on the map shows which tiles will be generated. Each tile is approximately 30 MB.

	<img width="2535" height="1394" alt="generating_terrain_files" src="https://github.com/user-attachments/assets/e60f5da3-365c-4ba6-a70f-2c98d55d6cb2" />



11. **Transfer to Device**
	 - **b14ckyy / Yaapu:** If you downloaded a ZIP, extract it and copy the extracted folder `ethosmaps` or `yaapu` to your radio's SD card → `bitmaps/`
	 - **Terrain:** If you downloaded a ZIP, extract the terrain files and copy them to the root of the flight controller's SD card (e.g. `N42E023.TER` or `N42E023.DAT`). The Re:Earth ZIP also includes a `NOTICE.txt` with the required data attribution — keep it with the files if you redistribute them.

	 **SD Card Preparation (Terrain only):**
	 - Format the SD card using the official [SD Memory Card Formatter](https://www.sdcard.org/downloads/formatter/sd-memory-card-formatter-for-windows-download/) (max 4 GB partition recommended). This ensures correct alignment and a compatible file system.
	 - Before copying the terrain files, delete the `FREESPAC.E` file from the SD card root. INAV uses this file to track free space — without deleting it, the card may appear full. INAV will automatically recreate it on the next startup.
	 - Use an external SD card reader for copying — INAV MSC (Mass Storage Class) mode is very slow.
	 - Use only high-quality, branded SD cards. The terrain system is sensitive to read speed and reliability.

12. **Other Features**
	 - Measurement tool, full screen mode, remember settings, restore settings, lock zoom, Android mobile devices compatibility.

---

**Example of High-Res Map Generator loaded on mobile phone (screen 6.5")**

![Screenshot_2026-03-21-02-50-35-21_40deb401b9ffe8e1df2f1cc5ba480b12](https://github.com/user-attachments/assets/e57b34e9-6c41-43f3-bbb1-75b6f81024d0)

![Screenshot_2026-03-21-02-52-05-46_40deb401b9ffe8e1df2f1cc5ba480b12](https://github.com/user-attachments/assets/6d21b6a1-a9d8-47f3-b90b-34760d20a759)


---

## Notes

- **Geolocation:** The app can center the map on your current location, but for security reasons, browser geolocation permission is only requested for 10 seconds per session.
- **Yaapu Output Folders:** Even when using ESRI or OSM as the provider, the Yaapu output folders will always be named as if they are Google map types. This is required for Yaapu compatibility.
- **Terrain Source Fallback:** When **SRTM** is selected, a warning is shown if the area extends beyond its 60°N–56°S band (Re:Earth is global). If **Re:Earth** is unreachable during a generation, the tool asks before switching to SRTM — it never changes the source silently.
- **Terrain Elevation Cache:** Downloaded elevation grids are cached locally in the browser using IndexedDB (~25 MB per grid), kept separate per source. Repeat generations for nearby or overlapping areas are fast — the data is reused without re-downloading. You can clear the cache from the sidebar.
- **Terrain Live Altitude:** After generating terrain files, you can enable the "Live Altitude" toggle to see real-time ground elevation under your cursor. This uses the locally cached elevation data — no additional server requests are made.

---

## Data sources & attribution

Map tiles are © their respective providers (OpenStreetMap, Esri, Google, MapTiler). Terrain elevation
data is credited as follows:

- **Re:Earth Terrain · Mapterhorn (CC BY 4.0)** — see the full source list at [mapterhorn.com/attribution](https://mapterhorn.com/attribution/). Re:Earth Terrain is a best-effort community service, provided without warranty; always verify terrain clearance before flight.
- The Re:Earth terrain output is derived largely from Copernicus data:
  > produced using Copernicus WorldDEM-30 © DLR e.V. 2010-2014 and © Airbus Defence and Space GmbH 2014-2018 provided under COPERNICUS by the European Union and ESA; all rights reserved
  >
  > The organisations in charge of the Copernicus programme by law or by delegation do not incur any liability for any use of the Copernicus WorldDEM-30
- **NASA SRTM** elevation data is in the public domain, fetched via the AWS "Terrain Tiles" (Mapzen) mirror.

This project is not endorsed by or affiliated with Re:Earth, Mapterhorn, Copernicus, ESA, or NASA.

---

For questions or issues, please open an issue on the [GitHub repository](https://github.com/MartinovEm/High-Resolution-Map-Generator/issues).
