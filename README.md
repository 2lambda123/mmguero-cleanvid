# cleanvid

[![Latest Version](https://img.shields.io/pypi/v/cleanvid)](https://pypi.python.org/pypi/cleanvid/) [![Docker Image](https://github.com/mmguero/cleanvid/workflows/cleanvid-build-push-ghcr/badge.svg)](https://github.com/mmguero/cleanvid/pkgs/container/cleanvid)

cleanvid is a little script to mute profanity in video files in a few simple steps:

1. The user provides as input a video file and matching .srt subtitle file. If subtitles are not provided, [`subliminal`](https://github.com/Diaoul/subliminal) is used to attempt to download the best matching .srt file.
2. [`pysrt`](https://github.com/byroot/pysrt) is used to parse the .srt file, and each entry is checked against a [list](swears.txt) of profanity or other words or phrases you'd like muted. Mappings can be provided (eg., map "sh*t" to "poop"), otherwise the word will be replaced with *****.
3. A new "clean" .srt file is created. with *only* those phrases containing the censored/replaced objectional language.
4. [`ffmpeg`](https://www.ffmpeg.org/) is used to create a cleaned video file. This file contains the original video stream, but the audio stream is muted during the segments containing objectional language. The audio stream is re-encoded as AAC and remultiplexed back together with the video.

You can then use your favorite media player to play the cleaned video file together with the cleaned srt file.

cleanvid is part of a family of projects with similar goals:

* 📼 [cleanvid](https://github.com/mmguero/cleanvid) for video files
* 🎤 [monkeyplug](https://github.com/mmguero/monkeyplug) for audio files
* 📕 [montag](https://github.com/mmguero/montag) for ebooks

## Prerequisites

[cleanvid](cleanvid.py) requires:

* Python 3
* [FFmpeg](https://www.ffmpeg.org)
* [babelfish](https://github.com/Diaoul/babelfish)
* [delegator.py](https://github.com/kennethreitz/delegator.py)
* [pysrt](https://github.com/byroot/pysrt)
* [subliminal](https://github.com/Diaoul/subliminal)

## usage

```
usage: cleanvid [-h] [-s <srt>] -i <input video> [-o <output video>] [--subs-output <output srt>]
                [-w <profanity file>] [-l <language>] [-p <int>] [-e] [-f] [--subs-only]
                [-r] [-b] [-v VPARAMS] [-a APARAMS]

optional arguments:
  -h, --help            show this help message and exit
  -s <srt>, --subs <srt>
                        .srt subtitle file (will attempt auto-download if unspecified)
  -i <input video>, --input <input video>
                        input video file
  -o <output video>, --output <output video>
                        output video file
  --subs-output <output srt>
                        output subtitle file
  -w <profanity file>, --swears <profanity file>
                        text file containing profanity (with optional mapping)
  -l <language>, --lang <language>
                        language for srt download (default is "eng")
  -p <int>, --pad <int>
                        pad (seconds) around profanity
  -e, --embed-subs      embed subtitles in resulting video file
  -f, --full-subs       include all subtitles in output subtitle file (not just scrubbed)
  --subs-only           only operate on subtitles (do not alter audio)
  -r, --re-encode       Re-encode video
  -b, --burn            Hard-coded subtitles (implies re-encode)
  -v VPARAMS, --video-params VPARAMS
                        Video parameters for ffmpeg (only if re-encoding)
  -a APARAMS, --audio-params APARAMS
                        Audio parameters for ffmpeg
```

### Docker

Alternately, a [Dockerfile](Dockerfile) is provided to allow you to run cleanvid in Docker. You can build the `ghcr.io/mmguero/cleanvid:latest` Docker image with [`build_docker.sh`](./docker/build_docker.sh), then run [`cleanvid-docker.sh`](./docker/cleanvid-docker.sh) inside the directory where your video/subtitle files are located.

## Contributing

If you'd like to help improve cleanvid, pull requests will be welcomed!

## Authors

* **Seth Grover** - *Initial work* - [mmguero](https://github.com/mmguero)

## License

This project is licensed under the BSD 3-Clause License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

Thanks to:

* the developers of [FFmpeg](https://www.ffmpeg.org/about.html)
* [Mattias Wadman](https://github.com/wader) for his [ffmpeg](https://github.com/wader/static-ffmpeg) image
* [delegator.py](https://github.com/kennethreitz/delegator.py) developer Kenneth Reitz and contributors
* [pysrt](https://github.com/byroot/pysrt) developer Jean Boussier and contributors
* [subliminal](https://github.com/Diaoul/subliminal) developer Antoine Bertin and contributors