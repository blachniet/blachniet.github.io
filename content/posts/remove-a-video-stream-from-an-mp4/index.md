---
title: "Remove a video stream from an MP4"
date: 2023-01-16T14:22:49-05:00
tags:
- ffmpeg
---

My kids figured out pretty early how to capture videos and pictures on our tablets. I ran across a video recently that was taking up almost 2 GB of space in my library. I played it back to find that my toddler (at the time) had captured an hour-long video, but had left the tablet on the ground the entire time, camera facing down.

The entire video was black, but the audio was pretty adorable. I wanted to keep the audio, but hated that it was taking up so much space. Fortunately, I've used [`ffmpeg`](https://ffmpeg.org/) enough to know that I can strip streams out of a media file like this.

In this post, I'll walk you through how I accomplished this. I'll show you how I was able to:

1. Remove the video, since I know it was all black.
1. Keep the audio.
1. Keep all the metadata from the original file (e.g. `creation_time`).

Let's start by figuring out what streams are in the file. We'll use [`ffprobe`][ffprobe], which *gathers information from multimedia streams and prints it in human- and machine-readable fashion[^1].*

```sh
ffprobe 20140716_071935.mp4
```

I won't include the entire output, because it's fairly verbose. Instead, I'll skip down to the section that describes the input.

```plain{hl_lines=[8,15]}
Input #0, mov,mp4,m4a,3gp,3g2,mj2, from '20140716_071935.mp4':
  Metadata:
    major_brand     : isom
    minor_version   : 0
    compatible_brands: isom3gp4
    creation_time   : 2014-07-16T12:17:10.000000Z
  Duration: 00:57:32.52, start: 0.000000, bitrate: 4743 kb/s
  Stream #0:0[0x1](eng): Video: h264 (High) (avc1 / 0x31637661), yuv420p(progressive), 1280x720,
 4615 kb/s, 29.91 fps, 29.92 tbr, 90k tbn (default)
    Metadata:
      creation_time   : 2014-07-16T12:17:10.000000Z
      handler_name    : VideoHandle
      vendor_id       : [0][0][0][0]
      encoder         :                                
  Stream #0:1[0x2](eng): Audio: aac (LC) (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 123 kb/s (
default)
    Metadata:
      creation_time   : 2014-07-16T12:17:10.000000Z
      handler_name    : SoundHandle
      vendor_id       : [0][0][0][0]
```

We can see that the file has one audio stream (`Stream #0:1[0x2](eng): Audio`) and one video stream (`Stream #0:0[0x1](eng): Video`).

I want to remove the video stream since I know it's all black. This will result in a much smaller file. I want to keep the audio stream as well as all the metadata (e.g. `creation_time`). The command below will do exactly that.

```sh
ffmpeg \
    # Path to the input file.
    -i 20140716_071935.mp4 \
    # Copy all streams without re-encoding.
    -codec copy \
    # Copy all the streams.
    -map 0 \
    # Omit the video stream.
    -map -0:v:0 \
    # Copy the global metadata.
    -map_metadata 0 \
    # Copy the audio stream's metadata.
    -map_metadata:s:a 0:s:a \
    # Add some custom metadata.
    -metadata comment="Removed video stream because it was all black." \
    # Path to the output file.
    20140716_071935-audio-only.mp4
```

Let's use `ffprobe` to confirm the metadata and audio stream were copied to the output file.

```sh
ffprobe 20140716_071935-audio-only.mp4
```

Again, I'm omitting some of the verbose output at the beginning.

```plain {hl_lines=[6,8,10,13]}
Input #0, mov,mp4,m4a,3gp,3g2,mj2, from '20140716_071935-audio-only.mp4':
  Metadata:
    major_brand     : isom
    minor_version   : 512
    compatible_brands: isomiso2mp41
    creation_time   : 2014-07-16T12:17:10.000000Z
    encoder         : Lavf59.27.100
    comment         : Removed video stream because it was all black.
  Duration: 00:57:32.52, start: 0.000000, bitrate: 125 kb/s
  Stream #0:0[0x1](eng): Audio: aac (LC) (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 123 kb/s (
default)
    Metadata:
      creation_time   : 2014-07-16T12:17:10.000000Z
      handler_name    : SoundHandle
      vendor_id       : [0][0][0][0]
```

We can see that we retained the original `creation_time` metadata, both globally and in the stream. There's only one stream, an audio stream, and we added our `comment` metadata.

Now, let's check the file size.

```plain
-rw-r--r--  1 blachniet  staff    51M Jan 16 15:03 20140716_071935-audio-only.mp4
-rw-r--r--@ 1 blachniet  staff   1.9G Dec 18 07:30 20140716_071935.mp4
```

Wow! Our new file is around 97% smaller! 🎉

All that's left to do is play the output file in an audio player to confirm everything sounds good! If you want to dig into the `ffmpeg` options further, check out the [documentation](https://ffmpeg.org/ffmpeg.html).

[ffprobe]: https://ffmpeg.org/ffprobe.html
[^1]: https://ffmpeg.org/ffprobe.html#toc-Description
