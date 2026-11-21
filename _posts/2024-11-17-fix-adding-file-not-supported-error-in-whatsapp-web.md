---
title: 'Fix "Adding File Not Supported" Error in WhatsApp Web'
layout: post
tags:
  - ffmpeg
description: Fix unsupported video uploads in WhatsApp Web by re-encoding the file with supported codecs.
image:
  path: /assets/img/posts/fix-adding-file-not-supported-error-in-whatsapp-web/cover.png
  alt: "Fix \"Adding File Not Supported\" Error in WhatsApp Web"
---


In what's app web when you upload some video, it shows that **"1 file you tried adding is not supported"**. This is because the video file you are trying to upload does not have correct audio or video codecs.

What's app support, 
- `AAC` or `AC3` audio stream codec and 
- `H264` or `MPEG4` video codec 
- with either `MP4` container or `AVI/MKV` container. 

To fix this you can use some video converter to change codecs to supported codecs. I use `ffmpeg` CLI for this purpose.

```bash
ffmpeg -i input_file -c:v libx264 -c:a aac -strict experimental output_file.mp4

# in case only audio codec need to be changed
ffmpeg -i input_file.mp4 -codec:a aac output_file.mp4
```

## References 

- <https://superuser.com/questions/1216363/whatsapp-doesnt-support-video-format>
