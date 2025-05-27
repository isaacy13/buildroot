[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/flutter/buildroot/badge)](https://api.securityscorecards.dev/projects/github.com/flutter/buildroot)

# buildroot

Build environment for the Flutter engine

This repository is used by the [flutter/engine](https://github.com/flutter/engine) repository.
For instructions on how to use it, see that repository's [CONTRIBUTING.md](https://github.com/flutter/engine/blob/main/CONTRIBUTING.md) file.

To update your checkout to use the latest buildroot, run `gclient sync`.

To submit patches to this buildroot repository, create a branch, push to that branch, then submit a PR on GitHub for that branch.

To point the engine to a new version of buildroot after your patch is merged, update the buildroot hash in the engine's [DEPS file](https://github.com/flutter/engine/blob/main/DEPS).


### Isaac notes

The purpose of this was to cherry-pick Flutter code while retaining current flutter/engine/buildroot versions.
- Add xcprivacy files to various frameworks, including Flutter's macOS SDK
- Also added some hotfixes (new macOS 14 broke compiling on M-series chips, so added fixes)

```
isaacyeang@MacBookPro src % flutter --version
Flutter 3.16.8 • channel master • git@github.com:isaacy13/flutter.git
Framework • revision 67457e669f (1 year, 4 months ago) • 2024-01-16 16:22:29
-0800
Engine • revision 6e2ea58a5c
Tools • Dart 3.2.5 • DevTools 2.28.5
```

You can also dig thru old documentation in Flutter's flutter/engine/buildroot repos, but copying these down before I forget
- mac-cpu and no-goma flags are macOS arm64 specific
- disabled unit tests b/c I sat there for 2 hours and even put my M1 pro in the freezer, still didn't go thru -- takes up disgusting amounts of memory
- can play around w/ runtime-mode flag, the output folder changes accordingly
- unoptimized appears to compile faster, disables cpp compiler optimizations iirc

DEBUG
python3 ./flutter/tools/gn --mac-cpu=arm64 --no-goma --no-enable-unittests --runtime-mode=debug --unoptimized
sudo ninja -C out/host_debug_unopt_arm64

RELEASE
python3 ./flutter/tools/gn --mac-cpu=arm64 --no-goma --no-enable-unittests --runtime-mode=release
sudo ninja -C out/host_release_arm64

When running flutter projects locally, use:
--local-engine-src-path /path/to/engine/src
--local-engine host_debug_unopt_arm64
--local-engine-host host_debug_unopt_arm64

When running flutter projects in a pipeline, use:
--local-engine-src-path /path/to/engine/src
--local-engine host_release_arm64
--local-engine-host host_release_arm64
