# FRIScreen

## Timespan folders

Timespan folders are folders which name contains **two date-times**
*(ex. "2024-06-02T05.30.24 - 2024-06-02T08.30.24")* that represent
a certain timespan. If a timespan folder is detected in the current
directory and the current time is between the provided times, **the media
is fetched from that folder**.

***Currently we DO NOT support nested timespan folders!***

**IMPORTANT:** *Due to operating system restrictions on allowed character for folder names, the `:` from
standard ISO format has been replaced with `.` character*

```text
root/
    2024-06-02T00.00.00 - 2024-06-02T12.00.00/
        1.png
        2.jpg
    2024-06-02T12.00.00 - 2024-06-02T23.59.59/
        3.mp4
    4.png
    5.jpg
```

In the example above, during the entire ***2nd of June***, the first folder in the root directory
will be played. ***First 12 hours*** of the day, `1.png` and `2.png` will be shown and for the
***second 12 hours***, `3.mp4` will be shown. All other days and times, `4.png` and `5.jpg`
will be displayed.

## Media files

### File naming convention

For the script to recognize a file as a media file, all it needs to have is a
[supported file format](#supported-file-formats). If the format is detected, the media will be added into the array.

### Custom durations

By default, every file is displayed for `tP` seconds *(retrieved from configuration file)*. If we want certain files
to be displayed for longer/shorter periods of time, we can append ***_XXXs*** postfix after the file name and before
the file extension.

For example, the following file would be displayed for 15s: *my_avatar_15s.png*

### Video file limitations

Chrome has introduced a new security policy where no audio can be automatically played, unless the user has interacted
with the document at least once. To bypass this, we first try to play the video with audio and if it fails, we mute the
audio and retry. ***This issue is supposedly present only on Chrome browser***

**IMPORTANT:** *If you want the entire video file to play, you need to postfix it with its duration
(ex. my_video_50s.mp4)*. Otherwise, the video will only be played for the duration specified in configuration
and will be changed, even if it hasn't finished yet.

### Supported file formats

Currently, support for most common image and video file formats has been added. To add additional
file  extensions *(first make sure they are compatible with HTML img and video tag)*, just add them
to the following array located near the start of the `index.html` file.

```javascript
const imageFormats = ["png", "jpeg", "jpg", "webp"];
const videoFormats = ["mov", "mp4", "mkv"];
```

## Configuration file

To change the URL from where the configuration is read from, modify the `configUrl` variable in `index.html`. Currently,
the config is assumed to be located in the same folder as `index.html` and is accessible from the internet.

**tO** - time *(in seconds)* after which the content of  `url` from configuration will be refreshed

**tP** - time *(in seconds)* for media to be displayed for *(if it doesn't contain time postfix)*

**url** - url from where the media files will be read from

**resize_to_fullscreen** - resized the media *(image/video)* to fit the screen. ***WARNING: This WILL distort the
image/aspect ratio***

```json
{
  "tO": 10,
  "tP": 5,
  "url": "http://127.0.0.1",
  "resize_to_fullscreen": false
}
```

## Development environment

This project has been tested on Apache web server with directory listing enabled. To turn on the display
of full file names and for other modifications, I used the following `.htaccess` file.

**IMPORTANT:** When setting up the web file server for media content, make sure to set the CORS Allow Origin policy to
appropriate values.

```text
Header set Access-Control-Allow-Origin *

Options +Indexes
<IfModule mod_autoindex.c>
  IndexOptions NameWidth=*
</ifModule>
```