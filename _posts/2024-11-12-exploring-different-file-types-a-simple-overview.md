---
title: "Exploring Different File Types: A Simple Overview"
layout: post
tags:
  - images
  - audio
  - videocodecs
  - file-types
description: A simple overview of file types, containers, codecs, and common media formats.
image:
  path: /assets/img/covers/exploring-different-file-types-a-simple-overview-cover.png
  alt: "Exploring Different File Types: A Simple Overview"
---


A computer file is a resource for recording data on a computer storage device, primarily identified by its filename. Just as words can be written on paper, so can data be written to a computer file. Files can be shared with and transferred between computers and mobile devices via removable media, networks, or the Internet. Files provide the abstraction needed to create a better user experience when operating a computing device.

## Types

So how can we categorize files? According to my opinion the best way to categorize files is based on the purpose they serve and the type of content they store. Here is how the categorization looks based on the content:

- Text Files - store only plain text
- Image Files - store images
- Audio Files - store sound digitally
- Video - store sound and images
- Documents - stores text, images, videos
- Application Files - store binary data related to execution of a application
- Archives - store collection of other file types

## Properties of files

Here are some things which normally every file will have:

- A filename
- A extension which is used to identify the file name, but is not necessary to have a extension
- File size: signify how much space the file takes on the storage.
- Compression status: is the file compressed or not. (compression is done to reduce file size).
    - Compression can be lossy and lossless.
    - Loosy compression removes unnecessarily data to reduce size whereas lossless compression maintain all your data.
- Encryption status: is the file encrypted or not

## Audio Files

Audio files store sound in digital format. There are many properties which a audio file will have.

- Type of `audio codecs` used, which affects type of compression, quality of sound.
- Bitrate which is the rate at which the instrument samples the sound, higher bitrate indicate higher data being captured.

Here are some common types of audio file formats and codec:

- `.mp3`
    - <https://en.wikipedia.org/wiki/MP3>
    - Lossy compression
    - Reduced bit rate
    - Store metadata in ID3 tags. <https://en.wikipedia.org/wiki/ID3>
- `aac`
    - <https://en.wikipedia.org/wiki/Advanced_Audio_Coding>
    - lower bitrate
- `ogg` `vorbis`
    - open source
- `flac` `alac`
    - high quality
    - lossless compression
- `waf` `aiff`
    - no compression used
    - contain all information
    - for audio editing
- `abi`
- `flv`
- `m4a`
- `mpv`
- `mpeg1/2/4`
- `wmv`

Most of the audio formats store the metadata associated in [ID3 tags](https://en.wikipedia.org/wiki/ID3).

## Video Files

Video files are more complicated, as they store both audio and images. So they are more like a container for audio and collection of images(video). So usually the extension in video files denotes the type of container. Different container support different type of **audio and video codecs** and other features. These audio and video codecs determines the compression and decompression of the data.

Audio codecs were discussed in audio files section, similarly here are some video codecs:

- `h.264`
    - better compression and quality
    - you can also select amount of compression
- `h.265`
    - more better than `h.264`
- `vp8`, `vp9`
    - codecs pushed by google,
    - open source codecs

Most common containers that you will encounter

- MPEG-4 Part 14 (MP4) `mp4`
    - can hold either mpeg, h264 video codec
    - aac, mp3 audio stream
    - **m4v** - type of mp4 with drm enabled (digital rights management)
- Matroska `mkv`
    - support just about any type of audio and video codecs
-  WebM `webm`
    - made by google
- Audio Video Interleave (AVI)

Also many containers are derived from other containers add features on top of previous one. Here is a hierarchy of how it looks like:

```yaml
- QTFF
    - ISO BMFF
        - MP4
        - 3GPP, 3GPP2
        - F4V
- MCF
    - Matroska
        - WebM
- RM
    - RMVB
- MPEG-PS
    - MPEG-TS
        - M2TS
    - VOB
        - EVOB
- RIFF
    - AVI
        - DMF
```

For comparison of containers visit - <https://en.wikipedia.org/wiki/Comparison_of_video_container_formats>

## Image Files

Before getting in image files we have to discuss how a image can be digitalized. There are basically two ways of doing so, first you create a grid of points and associate a color with that point (called raster images) or you store equations related to the image like a line from point `0,0` to `10, 0` or a circle with radius `5` with center `5,5` and similarly for other shapes(called vector images).
 
Vector Images works on mathematical points system therefore can be scaled indefinitely. This makes them ideal for logos, type work and icons.

Raster Images works on a pixel grid, a fixed size where each pixel is assigned a color. They are ideal for photos, web graphics and illustrations with complex color and shadows.

Here are some common image file types:

- Join photographic experts group `jpeg` 
    - lossy compression and raster format
    - allow to set the compression level
- Graphics interchange format `gif` 
    - lossless, raster
    - limited to 256 colors
- Portable network graphics `png`
    - lossless, raster
    - allow transparency
    - best for large areas of solid, limited colors
- Tagged image format `tiff` 
    - lossless, raster
    - for printing
    - can also do compression on it
- Raw
    - format for raw data captured by a digital camera
    - lossless compression
- Encapsulated postscript `eps` 
    - vector format
- Scalable vector graphics `svg` 
    - vector graphics
    - for icons
- `webp`
    - google replacement for images in web, smaller in sizes

## References

- <https://en.wikipedia.org/wiki/Computer_file>
- <https://en.wikipedia.org/wiki/List_of_open_file_formats>
