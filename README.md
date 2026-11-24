# Prebuilt DuckDB build tree: upstream commit 87d96b77, linux/aarch64

**This is NOT an official DuckDB release and not a general-purpose installation.** It is a build cache: the unmodified output directory of a CMake/Ninja build of upstream DuckDB, kept so a container image can restore it instead of compiling from scratch. It only works in one container layout (see "Usage constraints").

## What it is
- Source: https://github.com/duckdb/duckdb at commit `87d96b7773a10356b11cc15c352125acdc58a9ce` (the build records `DUCKDB_SOURCE_ID=87d96b7773` and `DUCKDB_VERSION=v2.1.0-dev1`). Built from that commit with no source changes.
- Platform: Linux, aarch64 (arm64) only (`CMAKE_SYSTEM_PROCESSOR=aarch64`; the compiled code is `-march=armv8-a -mtune=generic`).
- Built in a container based on Ubuntu 24.04 with GCC/G++ 13.3.0, CMake 3.28.3, Ninja 1.11.1.
- Configuration recorded in `CMakeCache.txt`: generator Ninja, `CMAKE_BUILD_TYPE=Release` (`-O3 -DNDEBUG`, C++17), `BUILD_UNITTESTS=1`, `BUILD_BENCHMARKS=0`, `BUILD_SHELL=ON`, `EXTENSION_STATIC_BUILD=ON`, `ENABLE_JEMALLOC=ON`. The cache also holds `ENABLE_SANITIZER=ON` and `ENABLE_UBSAN=ON`, but no `-fsanitize` flag appears in any build rule, so the binaries are not sanitizer-instrumented. The build rules reference `/usr/bin/ccache` as compiler launcher (it must exist in the target image).
- Targets present: `duckdb` (shell), `src/libduckdb.so`, `test/unittest`, and the object files and static libraries they were linked from.

## Usage constraints
- The tree embeds absolute paths: build directory `/opt/job2-release`, source directory `/root/repos/duckdb`. It is only usable when extracted at `/opt` (top-level directory `job2-release/`) with the same upstream commit checked out at `/root/repos/duckdb`.
- Ninja decides what to rebuild from file times: the source files must be older than the oldest file in the archive (2026-11-23 13:21 UTC).
- It is not an installer, package or supported distribution. No warranty.

## Archive
- File: `job2-release-unpatched-tree.tar.gz` (gzip-compressed pax tar, `tar --sort=name`, owner and group 0, `gzip -n -6`).
- Size 153,049,079 bytes; 1,963 entries (1,158 regular files, 805 directories, no links, devices or setuid files); 633,732,540 bytes of regular-file content when unpacked.
- SHA-256: `3f6ec2d458fc3cc9cdf64624e73213c6601285a641e042286f76d30710d19268`.

## Licence
DuckDB is released under the MIT licence, Copyright 2018-2026 Stichting DuckDB Foundation (https://github.com/duckdb/duckdb/blob/87d96b7773a10356b11cc15c352125acdc58a9ce/LICENSE). The tree includes compiled third-party components (for example brotli, fmt, fsst, jemalloc, lz4, mbedtls, miniz, re2, snappy, thrift, utf8proc, yyjson, zstd, parquet and the ICU extension), each under its own licence found in the upstream source tree under `third_party/` and `extension/`. The archive contains build outputs only and carries no licence files itself; the MIT licence text above applies to the DuckDB portions and the third-party licences apply to theirs.
