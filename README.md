# Benchmarking Video conversion in the browser

This is a non-scientific, but good-faith benchmark showing that `@remotion/webcodecs` is faster than FFmpeg.wasm to back up claims that we may make in order to promote `@remotion/webcodecs`.

## Test conditions

- Each conversion is run 3 times, average is taken.
- We alternated between running a test using `@remotion/webcodecs` and alternatives to minimize system load differences
- Tests were run on Google Chrome 131.0.6778.205 on a M2 MacBook Air running macOS 15.0.1. No other apps and websites were running.
- Rounding: For `@remotion/webcodecs` the time is **rounded up** to the next second, for FFmpeg.wasm the time is **rounded down** to seconds.

**Settings and versions**:
`@remotion/webcodecs`: 4.0.244 through https://remotion.dev/convert.
`FFmpeg.wasm`: Through https://ffmpeg.wide.video, tests executed on December 29, 2024. We selected this interface because it includes the SIMD version of FFmpeg.wasm, which should benefit its speed.

## Converting a MP4 to WebM

Asset used: https://remotion-assets.s3.eu-central-1.amazonaws.com/example-videos/bigbuckbunny.mp4
Asset was first downloaded to the machine, download time is not included.

Settings for remotion.dev/convert: Container WebM, Video convert to VP8, Audio convert to Opus
Command for FFmpeg.wasm: `ffmpeg -i bigbuckbunny.mp4 -c:v libvpx -c:a libopus bigbuckbunny.webm`

|                     | Run 1  | Run 2  | Run 3  | Average  |
|---------------------|--------|--------|--------|----------|
| @remotion/webcodecs | 8sec   | 7sec   | 7sec   | 7.4sec   |
| FFmpeg.wasm         | 112sec | 116sec | 115sec | 113.3sec |

## Converting an AV1 WebM to MP4
Asset used: https://remotion-assets.s3.eu-central-1.amazonaws.com/example-videos/av1-bbb.webm
Asset was first downloaded to the machine, download time is not included.

Settings for remotion.dev/convert: Container MP4, Video convert to H.264
Command for FFmpeg.wasm: `ffmpeg -i av1-bbb.webm out.mp4`

|                     | Run 1 | Run 2 | Run 3 | Average |
|---------------------|-------|-------|-------|---------|
| @remotion/webcodecs | 4sec  | 4sec  | 4sec  | 4sec    |
| FFmpeg.wasm         | 22sec | 20sec | 19sec | 20.3sec |

## Summary

In our tests, we observed every time that `@remotion/webcodecs` is at least 5 times faster. 
To make a claim in good faith and give the benefit of the doubt in case of any deviations, we make the conservative claim that `@remotion/webcodecs` is 3 times faster on a M2 MacBook.
