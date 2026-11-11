# @tsclang/desktop — official toolchain bundles

This repository hosts the downloadable toolchain bundles consumed by the
[`@tsclang/desktop`](https://github.com/tsclang/tsclang/tree/develop/packages/platforms/desktop)
platform package of the [TSClang](https://github.com/tsclang/tsclang) compiler.

The packages' `platform.toolchains.<name>.downloads["<os>-arch>"]` entries point
at [Releases](../../releases) of this repository — never at raw upstream
downloads. This is the **sha256 contract**: a pinned hash ships with the
package manifest, so upstream re-uploads cannot silently change what users
execute, and availability is under our control.

## Bundles

| Tag | Toolchain | OS-arch | Origin | License |
|-----|-----------|---------|--------|---------|
| `toolchains-v1` | gcc 16.2.0 | win32-x64 | [WinLibs GCC](https://winlibs.com/) (self-contained MinGW-w64 build) | GPL-3.0 + GCC Runtime Library Exception |
| `toolchains-v1` | gcc 16.2.0 | linux-x64 | [LLVM/Clang 23.1.5](https://github.com/llvm/llvm-project/releases/tag/llvmorg-23.1.5) official portable build | Apache-2.0 WITH LLVM-exception |
| `toolchains-v1` | gcc 16.2.0 | darwin-arm64 | LLVM/Clang 23.1.5 official portable build | Apache-2.0 WITH LLVM-exception |

Posix bundles ship **LLVM/Clang**: upstream GCC publishes no portable
linux/macOS binaries, while LLVM ships statically linked ones that run on any
distribution. The `gcc` toolchain name is preserved — `bin/gcc`, `bin/g++` and
`bin/cc` are symlinks to `clang`/`clang++` (GNU-compatible driver), stated in
each archive's `TSC_BUNDLE_NOTICE.txt`.

**darwin-x64** (Intel Macs): LLVM publishes no x64 macOS binaries, so no
bundle is released for it. The documented fallback is the system compiler:
```sh
xcode-select --install   # installs clang from Xcode Command Line Tools
```

Bundles are assembled by this repository's CI
([`.github/workflows/bundles.yml`](.github/workflows/bundles.yml)).

Every archive carries a `TSC_BUNDLE_NOTICE.txt` stating the upstream source
and license. Layout contract: a `bin/` directory on top (the TSClang
toolchain resolver prepends it to `PATH` and spawns absolute paths from it).

Release assets are immutable. A new toolchain version ships as a **new tag**
(`toolchains-v2`, …) plus a package manifest bump — existing installs keep
verifying against their pinned hash.
