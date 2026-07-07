
# M3U8 AppScript Relay Downloader

🇺🇸 English



M.A.R.D is an M3U8 video downloader powered by Google Apps Script relays. It allows you to download M3U8 video streams with your maximum available bandwidth while helping bypass certain network restrictions, such as regional blocking, filtering, or access limitations.

Many modern websites and streaming platforms deliver their videos using the HLS (M3U8) format. By simply providing the playlist URL to M.A.R.D, you can download the stream, merge its segments, and save it directly as an MP4 file.


## Features

- 🌍 Bypass network restrictions using Google Apps Script relays.
- 🚀 Download M3U8 (HLS) streams at your maximum available bandwidth.
- 📦 Automatic downloading and merging of TS segments.
- 🎞️ Direct MP4 conversion using FFmpeg.
- ⚡ Batch downloading for improved performance.
- 📂 Customizable download and output directory.
- 🎯 Automatic playlist and stream detection.
- ⚙️ Simple JSON-based configuration.
- 🪶 Lightweight, fast, and written entirely in Go.
- 🖥️ Standalone executable with no installation required.
- 📜 Clean console output with detailed progress and error messages.
## Installation
To use m.a.r.d., download one of the releases.

[📥 Download the latest release](../../releases/latest)
## Configuration
- To use m.a.r.d., you need to perform a few steps in Google Apps Script.
These steps are simple and can be completed in just a few minutes.

1. Open https://script.google.com
2. Create a new Apps Script project.
3. Delete the default code and paste this entire file.
4. Click "Deploy" → "New deployment".
5. Select "Web app".
6. Configure:
   - Execute as: Me
   - Who has access: Anyone
7. Click "Deploy" and authorize the requested permissions.
8. Copy the generated Web App key (deployment ID).
9. Open config.json

- ASA (AppScript api key)
    - paste the URL key (deployment ID) into the "asa" field.
    - Example:
    `"asa": "AKfycbxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"`

- batchSize
    - Number of TS segments processed in each batch.
    - For example, a value of 5 means five TS files will be handled per batch.
    - Larger values may improve speed but increase network load.

    - Example:
`"batchSize": 5`

- Download mode.
    - Available values:
        - "go" - Starts downloading every TS file in the current batch concurrently.
        This is the fastest option and is recommended for stable internet 
        - "nr" - Downloads TS files one by one within each batch.
        Slower, but more reliable on unstable or slow connections.
    - Example:
`"downloadMode": "go"`

- Download path
    - Working directory used during the download process.
    - Temporary files, TS segments, generated playlists, and other intermediate files will be - stored here.
    - If the directory does not exist, it will be created automatically.
    - It is recommended to keep this directory empty before starting a download.
    - This setting is completely independent of the final output file location.
    - You can save the resulting MP4 anywhere you want.
    - Unless you have a specific reason, it is recommended to leave this unchanged.
    - Example:
`"downloadPath": "downloads/"`

- FFmpeg path
    - If you downloaded the full package, ffmpeg.exe is usually included with it. Verify that the file exists before running the program.
    - If FFmpeg is missing, the downloader will still generate the final M3U8 playlist, but it will not be able to automatically merge the TS files into an MP4.
    - In that case, you can convert the generated playlist manually using FFmpeg later.
    - Example:
`"ffmpegPath": "ffmpeg.exe"`


## Usage/Examples

- Extract all the downloaded files and place them in a specific location.
- Open CMD
```console
Mard-1.0.0.exe <M3U8-Url> <Output-File>
```
- Example
```console
Mard-1.0.0.exe https://example.com/example.m3u8 test.mp4
```
## How It Works

First, you provide the URL of an M3U8 file. The application then sends an HTTP GET request to your Google Apps Script endpoint, which fetches the M3U8 file and returns its contents.

The returned content is parsed and analyzed. If the file is a **Master M3U8 playlist**, the application detects all available video qualities and displays them, allowing you to choose your preferred resolution.

Once a quality is selected (or if the provided file is already a media playlist), another GET request is sent to the Google Apps Script to fetch the final M3U8 playlist. The playlist content is then returned to the application.

The application parses this playlist and extracts all video segment URLs (`.ts` files). These segments are grouped into batches based on the `batchSize` value specified in your `config.json`.

The download process depends on the `downloadMode` option:

* **go** – Downloads all segments in each batch concurrently.
* **nr** – Downloads the segments sequentially, one by one, within each batch.

The segment download process is different from a normal direct download. Instead of downloading each segment directly, the application sends the segment URL to the Google Apps Script. The Apps Script downloads the file on Google's servers and returns its contents as encoded text (hexadecimal data). The application then decodes the data back into its original `.ts` binary format and saves it locally.

After all segment batches have been downloaded successfully, FFmpeg uses the generated M3U8 playlist to merge the segments into a single output file, such as **MP4** or any other format you selected.

## About the Bypass Mechanism

The component responsible for bypassing internet restrictions (such as censorship or regional access limitations) is the **Google Apps Script**.

Because the requests are made from Google's servers instead of your local machine, the Apps Script can access resources that may be unavailable or restricted from your network. It also acts as a proxy for downloading the M3U8 playlists and video segments before sending the data back to the application.


## RoadMap

- [x] Windows CLI version
- [ ] Linux CLI version
- [ ] macOS CLI version
- [ ] Graphical User Interface (GUI)
- [ ] Android application
## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
