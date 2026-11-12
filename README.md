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
| `toolchains-v2` | clang 23.1.5 | win32-x64 | [llvm-mingw 20261104](https://github.com/mstorsjo/llvm-mingw/releases/tag/20261104) (LLVM + lld + mingw-w64 UCRT, Martin Storsjö build) | Apache-2.0 WITH LLVM-exception + mingw-w64 |
| `toolchains-v2` | clang 23.1.5 | linux-x64 | [LLVM/Clang 23.1.5](https://github.com/llvm/llvm-project/releases/tag/llvmorg-23.1.5) official portable build | Apache-2.0 WITH LLVM-exception |
| `toolchains-v2` | clang 23.1.5 | darwin-arm64 | LLVM/Clang 23.1.5 official portable build | Apache-2.0 WITH LLVM-exception |

Desktop is **clang/LLVM-only** ([#282](https://github.com/tsclang/tsclang/issues/282)):
every target ships real Clang under its own name, no gcc aliases. Posix
targets use statically linked upstream LLVM builds that run on any
distribution; win32 uses llvm-mingw — the only self-contained portable
Clang + mingw-w64 distribution. The LLVM version matches across targets.

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
(`toolchains-v3`, …) plus a package manifest bump — existing installs keep
verifying against their pinned hash. (`toolchains-v1` held the gcc 16.2.0
bundles of desktop 0.3.0; v2 is the clang transition of #282.)
