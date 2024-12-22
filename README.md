# Zotify

## A highly customizable music and podcast downloader

<p align="center">
  <img src="https://i.imgur.com/hGXQWSl.png" width="50%" alt="Zotify logo">
</p>

## Features
- Downloads at up to 320kbps \*
- Downloads directly from the source \*\*
- Downloads podcasts, playlists, liked songs, albums, artists, singles.
- Downloads synced lyrics from the source
- Option to download in real time to reduce suspicious API request behavior \*\*\*
- Supports multiple audio formats
- Download directly from URL or use built-in in search
- Bulk downloads from a list of URLs in a text file or parsed directly as arguments

\* Free accounts are limited to 160kbps \*\
\*\* Audio files are NOT substituted with ones from other sources (such as YouTube or Deezer) \*\*\
\*\*\* 'Real time' downloading limits at the speed of data transfer to typical streaming rates (download time ≈  duration of the track) \*\*\*

## Installation

#### Dependencies:
  - Python 3.9 or greater
  - FFmpeg

#### Command:

`python -m pip install git+https://github.com/Googolplexed0/zotify.git`

See [INSTALLATION](INSTALLATION.md) for a more detailed and opinionated installation walkthrough.

## Command Line Usage

#### Basic Usage:

`zotify <track/album/playlist/episode/artist url>`   

Download track(s), album(s), playlist(s), podcast episode(s), or artist(s) specified by the URL(s) passed as a command line argument(s). 
If an artist's URL is given, all albums by the specified artist will be downloaded. Can take multiple URLs as multiple arguments, which must be passed within quotes.

| Command Line Flag             | Function                                                                               |
|-------------------------------|----------------------------------------------------------------------------------------|
| `-h`, `--help`                | See this message                                                                       |
| `-c`, `--config-location`     | Specify a directory containing a Zotify `config.json` file to load settings            |
| `-u`, `--username`            | Account username                                                                       |
| `--password`                  | Account password                                                                       |

| Command Line Flag (exclusive) | Function                                                                               |
|-------------------------------|----------------------------------------------------------------------------------------|
| `-d`, `--download`            | Download all tracks/albums/episodes/playlists URLs within the file passed as argument  |
| `-p`, `--playlist`            | Download playlist(s) saved by your account (interactive)                               |
| `-l`, `--liked`               | Download all Liked Songs on your account                                               |
| `-f`, `--followed`            | Download all songs by all followed artists                                             |
| `-s`, `--search`              | Search tracks/albums/artists/playlists based on argument (interactive)                 |

## Advanced Options

All these options can either be configured in the config or via the commandline, in case of both the commandline-option has higher priority.  
Set arguments in the commandline like this: `-sc False` or `--codec mp3`. Wrap arguments containing spaces or non-alphanumeric characters (weird symbols) with quotes like this: `--output-liked-songs "Liked Songs/{song_name}"`

| Config Key                   | Command Line Flag                        | Default Value           | Description
|------------------------------|------------------------------------------|-------------------------|------------------------------------------------------------------|
| `ROOT_PATH`                  | `-rp`, `--root-path`                     | `~/Music/Zotify Music`                           | Directory where Zotify saves music
| `SAVE_CREDENTIALS`           | `--save-credentials`                     | True                                             | Whether login credentials should be saved
| `CREDENTIALS_LOCATION`       | `--creds`, `--credentials-location`      |                                                  | Directory containing credentials.json
| `OUTPUT`                     | `--output`                               |                                                  | Master output file pattern (see below)
| `OUTPUT_PLAYLIST`            | `-op`, `--output-playlist`               | `{playlist}/{artist}_{song_name}`                | Output file pattern for playlists
| `OUTPUT_PLAYLIST_EXT`        | `-oe`, `--output-ext-playlist`           | `{playlist}/{playlist_num}_{artist}_{song_name}` | Output file pattern for extended playlists
| `OUTPUT_LIKED_SONGS`         | `-ol`, `--output-liked-songs`            | `Liked Songs/{artist}_{song_name}`               | Output file pattern for user's Liked Songs
| `OUTPUT_SINGLE`              | `-os`, `--output-single`                 | `{artist}/{album}/{artist} - {song_name}`        | Output file pattern for single tracks
| `OUTPUT_ALBUM`               | `-oa`, `--output-album`                  | `{album_artist}/{album}/{album_num} - {artist} - {song_name}` | Output file pattern for albums
| `EXPORT_M3U8`                | `-e`, `--export-m3u8`                    | False                     | Export tracks/albums/episodes/playlists with an accompanying .m3u8 file
| `ROOT_PODCAST_PATH`          | `-rpp`, `--root-podcast-path`            | `~/Music/Zotify Podcasts` | Directory where Zotify saves podcasts
| `TEMP_DOWNLOAD_DIR`          | `-td`, `--temp-download-dir`             |           | Download tracks to a temporary directory first
| `DOWNLOAD_FORMAT`            | `--codec`, `--download-format`           | copy      | Audio format/codec of downloads (aac, fdk_aac, m4a, mp3, ogg, opus, vorbis)
| `DOWNLOAD_QUALITY`           | `-q`, `--download-quality`               | auto      | Audio quality of downloads (normal, high, very_high*)
| `TRANSCODE_BITRATE`          | `-b`, `--bitrate`, `--transcode-bitrate` |           | Overwrite the bitrate for FFMPEG encoding
| `SONG_ARCHIVE_LOCATION`      | `--song-archive-location`                |           | Directory where Zotify saves the global song_archive file
| `DISABLE_DIRECTORY_ARCHIVES` | `--disable-directory-archives`           | False     | Disable local song_archive in download directories
| `SPLIT_ALBUM_DISCS`          | `--split-album-discs`                    | False     | Saves each disk in its own folder
| `DOWNLOAD_LYRICS`            | `--download-lyrics`                      | True      | Downloads synced lyrics in .lrc format, uses unsynced as fallback
| `LYRICS_LOCATION`            | `--lyrics-location`                      |           | Directory where Zotify saves lyrics files (default is output directory)
| `MD_DISC_TRACK_TOTALS`       | `--md-disc-track-totals`                 | True      | Whether track totals and disc totals should be saved in metadata
| `MD_SAVE_GENRES`             | `--md-save-genres`                       | False     | Whether genres should be saved in metadata
| `MD_ALLGENRES`               | `--md-allgenres`                         | False     | Save all relevant genres in metadata
| `MD_GENREDELIMITER`          | `--md-genredelimiter`                    | ,         | Delimiter character used to split genres in metadata
| `SKIP_EXISTING_FILES`        | `-ie`, `--skip-existing`                 | True      | Skip songs with the same name
| `SKIP_PREVIOUSLY_DOWNLOADED` | `-ip`, `--skip-previously-downloaded`    | False     | Use the global song_archive file to skip previously downloaded songs
| `RETRY_ATTEMPTS`             | `--retry-attempts`                       | 1         | Number of times Zotify will retry a failed request
| `BULK_WAIT_TIME`             | `--bulk-wait-time`                       | 1         | The wait time between bulk downloads
| `OVERRIDE_AUTO_WAIT`         | `--override-auto-wait`                   | False     | Totally disable wait time between songs with the risk of instability
| `CHUNK_SIZE`                 | `--chunk-size`                           | 20000     | Chunk size for downloading
| `DOWNLOAD_REAL_TIME`         | `-rt`, `--download-real-time`            | 0         | Specifies the download speed, where 1 means real-time speed and 2 means twice the real-time speed, should prevent account bans
| `LANGUAGE`                   | `--language`                             | en        | Language of metadata
| `PRINT_SPLASH`               | `--print-splash`                         | False     | Show the Zotify logo at startup
| `PRINT_SKIPS`                | `--print-skips`                          | True      | Show messages if a song is being skipped
| `PRINT_DOWNLOAD_PROGRESS`    | `--print-download-progress`              | True      | Show song download progress bar
| `PRINT_URL_PROGRESS`         | `--print-url-progress`                   | True      | Show url progress bar
| `PRINT_ALBUM_PROGRESS`       | `--print-album-progress`                 | True      | Show album progress bar
| `PRINT_ARTIST_PROGRESS`      | `--print-artist-progress`                | True      | Show artist progress bar
| `PRINT_PLAYLIST_PROGRESS`    | `--print-playlist-progress`              | True      | Show playlist progress bar
| `PRINT_PROGRESS_INFO`        | `--print-progress-info`                  | True      | Show download progress info
| `PRINT_DOWNLOADS`            | `--print-downloads`                      | True      | Print messages when a song is finished downloading
| `PRINT_WARNINGS`             | `--print-warnings`                       | True      | Show warnings
| `PRINT_ERRORS`               | `--print-errors`                         | True      | Show errors
| `PRINT_API_ERRORS`           | `--print-api-errors`                     | True      | Show API errors
| `FFMPEG_LOG_LEVEL`           | `--ffmpeg-log-level`                     | error     | FFMPEG's logged level of detail when completing a transcoded download

\* very_high (320k) is limited to Premium accounts only  

## Configuration Files

Using the `-c` (`--config-location`) flag does not set an alternate config location permanently. Alternate config locations must be specified in the command line each time Zotify is run. When unspecified, the configuration file will be read from and saved to the following default locations based on your operating system:

| OS              | Location          
|-----------------|--------------------------------------------------------------------|
| Windows         | `C:\Users\<USERNAME>\AppData\Roaming\Zotify\config.json`           |
| MacOS           | `/Users/<USERNAME>/Library/Application Support/Zotify/config.json` |
| Linux           | `/home/<USERNAME>/.config/zotify/config.json`                      |

To log out, just remove the configuration file and credentials file. Uninstalling Zotify does ***not*** remove either.

## Path Option Parsing

All pathing-related options (`CREDENTIALS_LOCATION`, `ROOT_PODCAST_PATH`, `TEMP_DOWNLOAD_DIR`, `SONG_ARCHIVE_LOCATION`, `LYRICS_LOCATION`) accept absolute paths.
They will substitute an initial `"."` with `ROOT_PATH` and properly expand both `"~"` & `"~user"` constructs.

The options `CREDENTIALS_LOCATION` and `SONG_ARCHIVE_LOCATION` use the following default locations depending on operating system:

| OS              | Location          
|-----------------|---------------------------------------------------------|
| Windows         | `C:\Users\<USERNAME>\AppData\Roaming\Zotify\`           |
| MacOS           | `/Users/<USERNAME>/Library/Application Support/Zotify/` |
| Linux           | `/home/<USERNAME>/.local/share/zotify/`                 |

## Output Format

With the option `OUTPUT` (or the commandline parameter `--output`) you can specify the pattern for the file structure of downloaded songs (not podcasts).  
The value is relative to the `ROOT_PATH` directory and may contain the following placeholders:

| Placeholder       | Description
|-------------------|--------------------------------
| `{artist}`        | The song artist
| `{album_artist}`  | The album artist
| `{album}`         | The song album
| `{song_name}`     | The song name
| `{release_year}`  | The song release year
| `{disc_number}`   | The disc number
| `{track_number}`  | The track number
| `{id}`            | The song id
| `{track_id}`      | The track id
| `{album_id}`      | (only when downloading albums) ID of the album
| `{album_num}`     | (only when downloading albums) Incrementing track number
| `{playlist}`      | (only when downloading playlists) Name of the playlist 
| `{playlist_num}`  | (only when downloading playlists) Incrementing track number

#### Example Values:

`{playlist}/{artist} - {song_name}`

`{playlist}/{playlist_num}_{artist}_{song_name}`

`{album_artist}/{album}/{album_num} - {song_name}`

`{artist}/{album}/{album_num} - {artist} - {song_name}`

## Docker Usage

#### Build the docker image from the Dockerfile:
`docker build -t zotify .`
#### Create and run a container from the image:
`docker run --rm -v "$PWD/Zotify Music:/root/Music/Zotify Music" -v "$PWD/Zotify Podcasts:/root/Music/Zotify Podcasts" -it zotify`


## What do I do if I see "Your session has been terminated"?

If you see this, don't worry! Just try logging back in. If you see the incorrect username or password error, reset your password and you should be able to log back in.


## Will my account get banned if I use this tool?

Currently no user has reported their account getting banned after using Zotify.

It is recommended you use Zotify with a burner account.
Alternatively, there is a configuration option labeled ```DOWNLOAD_REAL_TIME```, this limits the download speed to the duration of the song being downloaded thus appearing less suspicious.
This option is much slower and is only recommended for premium users who wish to download songs in 320kbps without buying premium on a burner account.

## Disclaimer
Zotify is intended to be used in compliance with DMCA, Section 1201, for educational, private and fair use. \
Zotify contributors are not responsible for any misuse of the program or source code.

## Contributing

Please refer to [CONTRIBUTING](CONTRIBUTING.md)

## Changelog

Please refer to [CHANGELOG](CHANGELOG.md)
