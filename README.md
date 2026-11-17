# mcbetoolbox_opencv

A trimmed, submodule-friendly vendor copy of [OpenCV](https://github.com/opencv/opencv) 4.8.0, used by [MCBEToolbox](https://github.com/JaderoChan/MCBEToolbox).

## Why this exists

MCBEToolbox only needs a small slice of OpenCV's functionality (image/video decoding, encoding and basic processing). Depending on an officially distributed OpenCV build would drag in dozens of modules and third-party libraries the project never uses, bloating the final statically-linked executable. This repository keeps only what MCBEToolbox actually needs, built entirely as static libraries, to **minimize the final executable's size**.

## What's included

- `modules/core`, `modules/imgproc`, `modules/imgcodecs`, `modules/videoio`
- Only the codec libraries `imgcodecs` needs: `libjpeg-turbo`, `libpng`, `libtiff`, `libwebp`
- OpenCV's own `cmake/` build scripts, with two small patches so it can be consumed via `add_subdirectory()` instead of an installed `find_package(OpenCV)`:
  - reuse the consuming project's own zlib target instead of vendoring/downloading a second zlib
  - skip generating the build-tree `OpenCVModules.cmake` export, which isn't needed by an `add_subdirectory()` consumer and otherwise fails because the shared zlib target isn't part of OpenCV's own export set

## What's NOT included

Everything else from upstream OpenCV: other modules (`dnn`, `features2d`, `highgui`, `calib3d`, `gapi`, ...), documentation, sample code, test data, and third-party codecs not needed here (OpenEXR, OpenJPEG/Jasper, FFmpeg's Windows plugin downloader, IPP, TBB, protobuf, quirc, etc.).

## Upstream

Based on [opencv/opencv](https://github.com/opencv/opencv) `4.8.0`, licensed under the Apache License 2.0 (see `LICENSE` / `COPYRIGHT`).

## Usage

This repository is consumed as a git submodule by [MCBEToolbox](https://github.com/JaderoChan/MCBEToolbox) at `3rdparty/opencv`, configured and built as static libraries via `add_subdirectory()`. It is not intended to be used standalone.
