# PoC port of [JUCE](http://www.juce.com/) for the browser via emscripten

This port was originally created [here](https://github.com/beschulz/juce_emscripten) by @beschulz. This fork is an unofficial continuation of the attempt.

## What is working
- messaging
- basic GUI with async repaints
- mouse wheel, left/right/middle buttons, shift/ctrl/alt modifiers
- keyboard events
- clipboard
- font rendering (via freetype)

(note: as of late 2019, this fork only works on Chrome because it needs SharedArrayBuffer support)

## What is not working
- audio (I'd go for an OpenALAudioDevice implementation in C++, because emscripten apparently supports OpenAL (via the AudioAPI) )
- threading (maybe it's working already, at least for the high resolution timer thread, but not thoroughly tested)

## Hacking

- [Download Emscripten](http://kripken.github.io/emscripten-site/docs/getting_started/downloads.html)
- install Emscripten
```shell
# Fetch the latest registry of available tools.
./emsdk update

# Download and install the latest SDK tools.
./emsdk install latest

# Make the "latest" SDK "active"
./emsdk activate latest

# Set the current Emscripten path on Linux/Mac OS X
source ./emsdk_env.sh
```

- compile the sample
```shell
cd examples/juce_emscripten/Builds/Linux/
emmake make
cd build
python -m SimpleHTTPServer
```
- [have a play](http://127.0.0.1:8000)

Note: I had to modify the auto-generated Makefile to get everything to work. So be carefull when you modify and save the jucer project. In the long run, it would be nice to have a emscripten target inside the introjucer.

## Thoughts

I just slapped this together to see, if it's possible. So don't expect your fancy JUCE Applications to Just Work (tm). There's still a lot of work to be done.

One of the hardest problems is threading. There are WebWorkers, but they use an entirely different model (no shared memory with main thread).

Networking is another interesting problem. Maybe one could adapt the Juce networking API to use WebSockets.

The URL-class also needs adoption to work inside the browser.

Audio should be possible via the WebAudioAPI or emscriptens OpenAL wrapper.

OpenGL should be doable.

Currently I am using LowLevelGraphicsSoftwareRenderer to do the rendering. But the API of LowLevelGraphicsContext looks suspiciously like the canvas API. There might be some performance to be gained by implementing LowLevelGraphicsCanvasRenderer.

It would be nice to have support for @font-face fonts.

It would be great to have a working implementation of WebBrowserComponent - just for the Inception-like effect :) 

Overall I was surprised, how nicely everything worked out. I see a lot of practical applications for JUCE supporting emscripten as a platform:
  - rich audio applications in the browser
  - node.js license-key validation (if you're using juce_cryptography)
  - bringing your existing applications to the web


## Licensing

Most of the JUCE-code is [licensed via GPLv2, v3, and AGPLv3](https://github.com/julianstorer/JUCE). A commercial license is available.
[Lato](http://www.latofonts.com/lato-free-fonts/) (The Font I used for the example) is licensed under SIL Open Font License.
My additions and modifications to this repository are licensed under the what-ever-is-compatible-with-the-existing-licensing-but-I-just-hope-it-will-be-usefull license.
![alt text](https://d30pueezughrda.cloudfront.net/juce/JUCE_banner.png "JUCE")

JUCE is an open-source cross-platform C++ application framework used for rapidly
developing high quality desktop and mobile applications, including VST, AU (and AUv3),
RTAS and AAX audio plug-ins. JUCE can be easily integrated with existing projects or can
be used as a project generation tool via the [Projucer](https://juce.com/discover/projucer),
which supports exporting projects for Xcode (macOS and iOS), Visual Studio, Android Studio,
Code::Blocks, CLion and Linux Makefiles as well as containing a source code editor and
live-coding engine which can be used for rapid prototyping.

---

## Getting Started
The JUCE repository contains a [master](https://github.com/weareroli/JUCE/tree/master)
and [develop](https://github.com/weareroli/JUCE/tree/develop) branch. The develop branch
contains the latest bugfixes and features and is periodically merged into the master
branch in stable [tagged releases](https://github.com/WeAreROLI/JUCE/releases)
(the latest release containing pre-built binaries can be also downloaded from the
[JUCE website](https://shop.juce.com/get-juce)).

The repository doesn't contain a pre-built Projucer so you will need to build it
for your platform - Xcode, Visual Studio and Linux Makefile projects are located in
[extras/Projucer/Builds](/extras/Projucer/Builds)
(the minumum system requirements are listed in the __System Requirements__ section below).
The Projucer can then be used to create new JUCE projects, view tutorials and run examples.
It is also possible to include the JUCE modules source code in an existing project directly,
or build them into a static or dynamic library which can be linked into a project.

For further help getting started, please refer to the JUCE
[documentation](https://juce.com/learn/documentation) and
[tutorials](https://juce.com/learn/tutorials).

## Minimum System Requirements
#### Building JUCE Projects
- __macOS__: macOS 10.11 and Xcode 7.3.1
- __Windows__: Windows 8.1 and Visual Studio 2015 64-bit
- __Linux__: GCC 4.8

#### Deployment Targets
- __macOS__: macOS 10.7
- __Windows__: Windows Vista
- __Linux__: Mainstream Linux distributions

## Contributing
For bug reports and features requests, please visit the [JUCE Forum](https://forum.juce.com/) -
the JUCE developers are active there and will read every post and respond accordingly. When
submitting a bug report, please ensure that it follows the
[issue template](/.github/ISSUE_TEMPLATE.txt).
We don't accept third party GitHub pull requests directly due to copyright restrictions
but if you would like to contribute any changes please contact us.

## License
The core JUCE modules (juce_audio_basics, juce_audio_devices, juce_blocks_basics, juce_core
and juce_events) are permissively licensed under the terms of the
[ISC license](http://www.isc.org/downloads/software-support-policy/isc-license/).
Other modules are covered by a
[GPL/Commercial license](https://www.gnu.org/licenses/gpl-3.0.en.html).

There are multiple commercial licensing tiers for JUCE 5, with different terms for each:
- JUCE Personal (developers or startup businesses with revenue under 50K USD) - free
- JUCE Indie (small businesses with revenue under 200K USD) - $35/month
- JUCE Pro (no revenue limit) - $65/month
- JUCE Educational (no revenue limit) - free for bona fide educational institutes

For full terms see [LICENSE.md](LICENSE.md).
