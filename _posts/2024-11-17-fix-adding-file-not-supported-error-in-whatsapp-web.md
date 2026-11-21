---
title: 'Fix "Adding File Not Supported" Error in WhatsApp Web'
layout: post
tags:
  - ffmpeg
description: Fix unsupported video uploads in WhatsApp Web by re-encoding the file with supported codecs.
---

In WhatsApp Web, uploading a video may fail with **"1 file you tried adding is not supported"**.

This usually means the video file does not use a codec and container combination that WhatsApp supports.

WhatsApp generally supports:

- `AAC` or `AC3` audio codec
- `H264` or `MPEG4` video codec
- `MP4` or `AVI/MKV` container

To fix this, re-encode the file with `ffmpeg`:

```bash
ffmpeg -i input_file -c:v libx264 -c:a aac -strict experimental output_file.mp4

# If only the audio codec needs fixing
ffmpeg -i input_file.mp4 -codec:a aac output_file.mp4
```

## References

- Superuser discussion on unsupported WhatsApp video formats
