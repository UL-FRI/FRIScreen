# FRIScreen

FRIScreen is an advanced media slideshow application/website. It allows you to define which files should be shown, at
what time, in what order and for how long, just by modifying their names and locations.

## Installation

### Install website/application
To install this application/website, you just have to download the `index.html` and `config.example.json` *(you can
rename it to `config.json` for easier usage)* files and place them both into your web server directory.

After that just access the file/website as you normally would. If you renamed your configuration file to something
different from the default name or if you have multiple configuration file, make sure to specify the correct one in the
[query parameter](#specify-which-configuration-file-to-use) *(see section below for more details)*.

## Configuration file

### Specify which configuration file to use

To change the name/path of the configuration file to use, you just need to modify the query parameter when opening the
website. The query parameter used for this is named `config`.

**Examples *(the domain below is just an example and is not valid)*:**
- [https://fri-screen.com?config=custom_config.json]() *(will use the `custom_config.json` file)*
- [https://fri-screen.com]() *(will use the default `config.json` file)*

### Configuration variables

| Key                      | Type           | Description                                                                                                         |
|--------------------------|----------------|---------------------------------------------------------------------------------------------------------------------|
| **tO**                   | *integer*      | time *(in seconds)* after which the media from `urls` will be refreshed                                             |
| **tP**                   | *integer*      | default time *(in seconds)* for media to be displayed for *(only used if duration is not present in the file name)* |
| **urls**                 | *list[string]* | a list of url addresses from which to fetch the media files from                                                    |
| **resize_to_fullscreen** | *boolean*      | resizes the media *(images/videos)* to fit the screen ***(WARNING: This WILL distort the image/aspect ratio)***     |
| **fade_ms**              | *integer*      | duration of the cross fade *(in milliseconds)*                                                                      |
| **config_refresh**       | *integer*      | time *(in seconds)* after which the configuration file will be refreshed                                            |
| **reload_page**          | *integer*      | time *(in seconds)* after which the page will be reloaded/refreshed                                                 |

### Example configuration file

```json
{
  "tO": 300,
  "tP": 3,
  "url": ["http://127.0.0.1/media/", "http://127.0.0.1/default/"],
  "resize_to_fullscreen": true,
  "fade_ms": 500,

  "config_refresh": 10,
  "reload_page": 600
}
```

## Media configuration

### File-specific display duration

By default, every file is displayed for `tP` seconds *(retrieved from configuration file)*. If we want certain files
to be displayed for shorter/longer periods of time, we can append `_XXXs` postfix after the file name and before
the file extension.

For example, the following file would be displayed for 15s: *my_avatar_15s.png*

**NOTE: *Videos will always play until the end, even if they have time specified in their name***

### Timespan folders

Timespan folders are folders which names contains **two date-times**
*(ex. "2024-06-02T05.30.24 - 2024-06-02T08.30.24")* that represent
a certain timespan. If a timespan folder is detected in the current
directory and the current time is between the provided times, **the media
from that folder will be displayed**.

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

In the example above, on 2nd of June, for the **first 12 hours of the day** the following media
will be displayed: `1.png` and `2.jpg`. For the **second 12 hours of the day** the `3.mp4` file will be displayed. And
for all other days/times, the global files *(outside of directories)* will be displayed: `4.png`, `5.jpg`.

### Supported file formats

Currently, support for most common image and video file formats has been added. We covered ***ALL*** supported video
formats *(mp4, webm, ogg)* and a lot of different image formats.

**Supported image formats:**

- apng
- avif
- bmp
- cur
- gif
- ico
- jpeg
- jfif
- jpg
- pjp
- pjpeg
- png
- svg
- tiff
- webp

**Supported video formats:**
- mp4
- webm
- ogg
