## High-Resolution Map Generator

This is a web-based tool for generating and exporting 100x100 map tiles from high-resolution map providers, designed for RC pilots and hobbyists using **b14ckyy ETHOS Mapping Widget** or **Yaapu mapping widgets** on their radios. It also supports generating **INAV Terrain (SRTM)** elevation data files (.DAT) for INAV flight controllers. It enables quick, straightforward operation by selection of a flying area and direct download of compatible map tiles or terrain files to your SD card or as a ZIP file for later transfer.

⚠️ **You can use the tool online without downloading** by visiting: [https://martinovem.github.io/High-Resolution-Map-Generator/](https://martinovem.github.io/High-Resolution-Map-Generator/)

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/fe00dd94-3e69-469e-9afa-52c2fe8a25a2" />


---

### Supported Map Providers and Types

- **Providers:**
  - OSM (OpenStreetMap, standard map, non high-res)
  - ESRI (High-Res Satellite and Street)
  - Google (High-Res Satellite, Street, Hybrid)

- **Map Types:**
  - Street
  - Satellite
  - Hybrid (Satellite + Labels, where available)

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
	 - Output path: `/bitmaps/yaapu/maps/{GoogleMapType}/{Zoom}/{Y}/s_{X}.png`
	- **[Yaapu - EdgeTX](https://github.com/yaapu/HorusMappingWidget):**
	 - Output path: `/IMAGES/yaapu/maps/{GoogleMapType}/{Zoom}/{Y}/s_{X}.png`
	 - **Note:** For both Yaapu sub-targets, regardless of the selected provider (including ESRI or OSM), the output folder will always use the "Google" naming convention (`GoogleMap`, `GoogleSatelliteMap`, or `GoogleHybridMap`). This is required for compatibility with the Yaapu widget. For example, if you select ESRI as the provider and Satellite as the map type, the output will still be placed in `.../GoogleSatelliteMap/...` even though the tiles are from ESRI. This applies equally to Yaapu - ETHOS (`/bitmaps/yaapu/maps/...`) and Yaapu - EdgeTX (`/IMAGES/yaapu/maps/...`).
	- **[INAV Terrain (SRTM)](https://github.com/iNavFlight/inav/wiki/Navigation-Terrain-Following):**
	 - Generates `.DAT` terrain elevation files for INAV flight controllers
	 - Data source: [NASA SRTM1](https://www.usgs.gov/centers/eros/science/usgs-eros-archive-digital-elevation-shuttle-radar-topography-mission-srtm-1) — 1 arc-second (~30 m) resolution (public domain)
	 - Coverage: 60°N to 56°S latitude
	 - Output: One `.DAT` file per 1°×1° grid square (~111 km), written to FC SD card root
	 - Format: 2048-byte blocks, 32×28 int16 height grids, CRC-16/XMODEM checksums. The generated files are also compatible with ArduPilot
	 - Ocean depths are clamped to 0m
	 - **HGT Cache:** Downloaded SRTM tiles are cached in the browser (IndexedDB) so subsequent generations for nearby areas don't need to re-download
	 - **Live Altitude:** Toggle to show real-time ground elevation under cursor using cached SRTM data
- **Direct SD Card Sync:** If your browser supports the File System Access API (Chrome; Edge), write tiles directly to your SD card.
- **ZIP Download Fallback:** If SD card access is not available, the tool will offer a ZIP download for manual transfer.

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
	- Choose the widget type based on the Lua script installed on your radio (**b14ckyy** or **Yaapu**), or select **INAV Terrain (SRTM)** to generate elevation data for your flight controller.

4. **Set Project Name**

5. **Set Provider and Map Type**
	- For b14ckyy and Yaapu targets, select the map provider and type.
	- For INAV Terrain, the map is automatically set to OSM (map tiles are not exported — only elevation data).

6. **Set Zoom Levels**
	- Adjust the minimum and maximum zoom levels for the tiles exported. Range 1-20.
	- Not applicable for INAV Terrain — elevation resolution is fixed at SRTM1 30m.

7. **Find Your Flying Field**
	- Use the search box to locate your field, or pan/zoom manually. Optional zoom-lock feature.

8. **Select Area of Interest**
	- Draw a rectangle on the map to define the area for tile download.
	- Right-mouse click deletes the rectangle drawn. Second right-mouse click disengages the drawing tool.

	<img width="1364" height="768" alt="image" src="https://github.com/user-attachments/assets/7e9fd1f6-2f49-4fc1-85fc-4c82db1c14c4" />

	<img width="2560" height="1600" alt="select_area" src="https://github.com/user-attachments/assets/a71b231f-47b7-4775-8517-d51b91eb7c84" />




9. **Edit Area (Optional)**

10. **Sync or Download**
	 - **For b14ckyy / Yaapu:** Click "Sync to SD Card" to export tiles. If SD card is not connected, a ZIP download will be offered.

	 <img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/4f859e99-b11d-4cb8-afb0-a8634ccf848f" />


	 <img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/0ffde6b7-4b6e-43f0-9975-ed1528dccd2a" />

	 - **For INAV Terrain:** Click "Sync to SD Card" to generate `.DAT` files. The tool downloads SRTM elevation data from NASA servers, converts it to `.DAT` format, and writes the files directly to the flight controller SD card root. If no SD card is linked, a ZIP file (`INAV_Terrain_SRTM.zip`) will be offered for download. A 1°×1° grid overlay on the map shows which tiles will be generated. Each tile is approximately 30 MB.

	<img width="1366" height="768" alt="generating" src="https://github.com/user-attachments/assets/e71bb8a1-405b-4cfe-b820-d18ec07c480b" />


11. **Transfer to Device**
	 - **b14ckyy / Yaapu:** If you downloaded a ZIP, extract it and copy the extracted folder `ethosmaps` or `yaapu` to your radio's SD card → `bitmaps/`
	 - **INAV Terrain:** If you downloaded a ZIP, extract the `.DAT` files and copy them to the root of the flight controller's SD card (e.g. `N42E023.DAT`).

	 **SD Card Preparation (INAV Terrain only):**
	 - Format the SD card using the official [SD Memory Card Formatter](https://www.sdcard.org/downloads/formatter/sd-memory-card-formatter-for-windows-download/) (max 4 GB partition recommended). This ensures correct alignment and a compatible file system.
	 - Before copying `.DAT` files, delete the `FREESPAC.E` file from the SD card root. INAV uses this file to track free space — without deleting it, the card may appear full. INAV will automatically recreate it on the next startup.
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
- **INAV Terrain Coverage:** SRTM data covers latitudes 60°N to 56°S. Selections extending beyond this range will generate tiles with no elevation data for the out-of-range portions. A warning is shown when this occurs.
- **INAV Terrain HGT Cache:** SRTM tiles are cached locally in the browser using IndexedDB (~25 MB per tile). This means repeat generations for nearby or overlapping areas are fast — the data is reused without re-downloading. You can clear the cache from the sidebar.
- **INAV Terrain Data:** Only SRTM1 (1 arc-second, ~30 m) resolution is supported by INAV. This is the resolution used by the tool — no configuration needed.
- **INAV Terrain Live Altitude:** After generating `.DAT` files, you can enable the "Live Altitude" toggle to see real-time ground elevation under your cursor. This uses the locally cached SRTM data — no additional server requests are made.

---

For questions or issues, please open an issue on the [GitHub repository](https://github.com/MartinovEm/High-Resolution-Map-Generator/issues).
