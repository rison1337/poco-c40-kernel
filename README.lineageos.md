# POCO C40 (frost), kernel1

Complete source snapshot of the tested Linux 5.4.242 kernel.
Upstream: https://github.com/MiCode/Xiaomi_Kernel_OpenSource , frost-r-oss,
commit 47cd8bd80c2c910ed35feadf16e41785fd114603. Upstream COPYING and per-file licenses are retained.

Changes include FM/voice routing, CPU cooling, aligned USB gadget transfers,
and the per-port APR callback lifetime correction. Missing auxiliary RTMM/KTRACE
sources needed by the vendor tree are included.

Use configs/lineage-frost-kernel1.config as the full resolved configuration.
Toolchain: Android clang-r383902, aarch64 GCC4.9 and ARM GCC4.9 from the
pinned LineageOS18.1 manifest in the companion poco-c40-lineageos-18.1 repo.

Example (KERNEL_OUT is an absolute empty output directory):

    cp configs/lineage-frost-kernel1.config "$KERNEL_OUT/.config"
    make O="$KERNEL_OUT" ARCH=arm64 LLVM=1 LLVM_IAS=1 \
      CROSS_COMPILE=aarch64-linux-android- CROSS_COMPILE_COMPAT=arm-linux-androideabi- \
      CLANG_TRIPLE=aarch64-linux-gnu- LD=/absolute/path/to/ld.lld-wrapper -j6 Image.gz

The wrapper used for the tested binary invokes clang-r383902/bin/ld.lld
with --no-threads --thinlto-jobs=1. Include clang and both GCC toolchains in PATH.
This preserves the linked layout and module ABI checked against stock vendor
modules. The kernel binary does not include a boot ramdisk. The primary release boot
image uses the original unpatched stock ramdisk without Magisk. A separate
optional boot image preserves the tested Magisk26.3 ramdisk. Both use the same
verified kernel1 binary; enabling or disabling Magisk does not replace it with
the original stock kernel.

The verified compiled Image SHA256 was
67c1631eee616f697b44f3644ca8a18942d0f8316d00312e69ae5f90b2c9693a.
Compiler/build timestamps and signing choices affect reproducibility; exact
bit-for-bit output from an arbitrary toolchain is not claimed.
