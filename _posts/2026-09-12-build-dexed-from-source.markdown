---
layout: post
title:  "Building Dexed plugin from source"
date:   2026-09-12 17:00:00 +0200
categories: ["programming", "music"]
---

I wanted to use the [Dexed][Dexed] plugin (v1.0.1) with [Ardour][Ardour].
So I downloaded the plugin (VST3 format) from the [Github release (v1.0.1)](https://github.com/asb2m10/dexed/releases/tag/v1.0.1),
extracted it and put it at `~/.vst3` (I'm using Debian).

However, Ardour couldn't load the plugin (VST3 format) because it required a more recent version of GLIBC which I didn't have (I had GLIBC 2.36).

I was not sure if updating my GLIBC globally was a good idea. 
I thought that it may breaks other programs that depends on that specific version of the package.

So I decided to build the plugin VST3 from source.

Hopefully, the [build instructions on the project's README](https://github.com/asb2m10/dexed/tree/v1.0.1#how-to-build) were clear and straightforward, so I will not repeat it here.
But I'll talk about some small problem I encountered while following those instructions.

## Problem 1: tools needed for building from source

It needed CMake and pkg-config.

If you do not have the latter, when running the `cmake .. -DJUCE_COPY_PLUGIN_AFTER_BUILD=TRUE` part, you may get the error :

```
Could NOT find PkgConfig (missing: PKG_CONFIG_EXECUTABLE)
```

Just install them.

With my distro (Debian), it was pretty straightforward.
I just had to get the packages provided by my distro (`apt install ...`).

## Problem 2: dependencies

The [dependencies](https://github.com/asb2m10/dexed/wiki/Linux-build-dependencies) were :
```
libx11-dev
libcurl4-gnutls-dev
libfreetype6-dev
libasound2-dev
libxinerama-dev
libjack-jackd2-dev
libxcursor-dev
libxrandr-dev
```

Not having them will cause compilation error when running the `cmake --build .` part.

Again, I just had to get the packages provided by my distro (`apt install ...`).


Personnaly, I could not `apt install` the `libcurl4-gnutls-dev` package.
I had the error:

```
E: Failed to fetch http://deb.debian.org/debian/pool/main/c/curl/libcurl4-gnutls-dev_7.88.1-10%2bdeb12u14_amd64.deb  404  Not Found
```

But for some reason, I could compile the project anyway.

## Conclusion

That's all the small issues I had, but they were not too complicated to fix.

Also, kudos to the Dexed project maintainers and contributors for that great plugin.

[Dexed]: https://asb2m10.github.io/dexed/
[Ardour]: https://ardour.org/

