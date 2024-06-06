# FRIScreen

FRIScreen is a simple, minimal digital signage slideshow application.
It consists of a single .html file with some JavaScript and no external dependencies.


## Installation

### Install website/application

  1. Download index.html from this repo to your web server. 
  2. Download config.example.json and rename it to config.json. 
  3. Create a directory called "default". Ensure that autoindex is enabled for this directory.
  4. Upload some images and/or movies to default.
  5. ???
  6. Enjoy your slideshow.

## Configuration file

### Example configuration file

```json
{
  "urls": ["./screen1", "./default"],
  "refresh": 30,
  "present": 10,
  "resize_to_fullscreen": true,
  "fade_ms": 500,
  "config_refresh": 20,
  "reload_page": 600
}
```

### Configuration variables

| Key                      | Type           | Description                                                                                                         |
|--------------------------|----------------|---------------------------------------------------------------------------------------------------------------------|
| **urls**                 | *list[string]* | a list of url addresses to fetch the media files from                                                    |
| **refresh**                   | *integer*      | time *(in seconds)* after which the shlideshow from `urls` will be refreshed                                            |
| **present**                   | *integer*      | default time *(in seconds)* for each image to be displayed (can be overwritten by renaming the file, see *Changing the display duration* below ) |
| **resize_to_fullscreen** | *boolean*      | resizes the media *(images/videos)* to fit the screen ***(WARNING: This WILL distort the image/aspect ratio)***     |
| **fade_ms**              | *integer*      | duration of the cross fade *(in milliseconds)*                                                                      |
| **config_refresh**       | *integer*      | time *(in seconds)* after which the configuration file will be refreshed                                            |
| **reload_page**          | *integer*      | time *(in seconds)* after which the page will be reloaded/refreshed                                                 |

### Changing the config file / per-screen configuration

The `urls` parameter in the configuration file may contain multiple sources. You can use this to create per-screen slideshows.

Usually, must screens in a venue will display the same slideshow. Occasionally, one might want to change this slideshow to something specific (for example, display "LUNCH" on the screen in the main lobby, but not in front of each lecture room). A user can do this by setting `urls` to something like:

```
[ "./lobby", "./default" ]
```

Then, place an image of "LUNCH" (e.g. lunch.png) in *lobby/lunch.png*. Once lunch is over, simply rename, remove or empty the *lobby/* directory. The slideshow will revert to whatever is in *default/*.

FRIScreen will try to load each URL from urls in turn. As soon as one returns a non-empty list of links, it will be used for the slideshow.

With multiple screens, each screen will require a separate `urls`. This is done by specifying the config file.

To change the name/path of the configuration file to use, modify the query parameter when opening the
website. The query parameter used for this is named `config`.

**Examples *(the domain below is just an example and is not valid)*:**
- [https://fri-screen.com?config=lobby_config.json]() *(will use the `lobby_config.json` file)*
- [https://fri-screen.com]() *(will use the default `config.json` file)*


## Media configuration

### Changing the display duration

By default, every file is displayed for `present` seconds *(retrieved from configuration file)*. If we want certain files
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

## Supported file formats

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

## Supported browsers

This application is regularly tested in Chromium and Firefox. Your mileage may vary on Safari. Patches are welcome.
