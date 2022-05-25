---
title: "Fastboot Update Packages"
date: 2022-05-24T20:02:17+05:30
draft: false
---

An _updatepackage_, or a fastboot update package, is a file that contains raw partition images that is used to flash a ROM build onto the corresponding device the build is made for.

For AOSP developers, this _updatepackage_ is created using the command `m updatepackage` in the AOSP source tree.

This update package consists of the following contents:
- `android-info.txt`: A file containing information about the build, but usually the bootloader board name.
- `boot.img`: The boot image
- `boot_other.img`: An extended boot image
- `dtbo.img`: The device tree blob overlay image
- `vbmeta.img`: The vbmeta image
- `vbmeta_system.img`: The vbmeta image for system partition
- `vbmeta_vendor.img`: The vbmeta image for vendor partition
- `system.img`: The system image
- `system_ext.img`: The system_ext image, if it exists (only on devices launched with Android 11, usually missing on MediaTek devices on 11)
- `system_other.img`: An optional system image to extend the stock system partition
- `vendor.img`: The vendor image
- `vendor_boot.img`: The vendor boot image, containing vendor-specific bits of the boot image, in order to maintain GKI compatibility
- `vendor_dlkm.img`: An extended vendor image that contains only the DLKM modules
- `vendor_other.img`: An optional vendor image to extend the stock vendor partition
- `odm.img`: The ODM image
- `odm_dlkm.img`: An extended ODM image that contains only the DLKM modules
- `cache.img`: The cache image
- `userdata.img`: The userdata image
- `recovery.img`: The recovery image

Some of the above partitions may or may not be present in the update package depending on the build, and device, on the basis of different factors, i.e. depending on whether if the device supports Generic Kernel Images, or not, different partitioning methods (A, A/B, virtual A/B, dynamic partitioning, and such.)

Dynamic partitioning requires booting into a _fastbootd_ mode, which allows writing to the logical partitions available on the device, inside the super partition.

The resulting build after running `m updatepackage` in the AOSP source tree is of the following format usually: `<device>-img-<eng.user>.zip`, for example: `phoenix-img-eng.gagan.zip`.

These builds can be flashed using `fastboot update <filename>`.
