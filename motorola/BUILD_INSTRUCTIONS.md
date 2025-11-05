# Building TWRP Recovery for Motorola Moto G - 2025 (Kansas)

## Device Information

- **Device**: Motorola Moto G - 2025 (Kansas)
- **Codename**: kansas
- **Platform**: MediaTek MT6835
- **Android Version**: 15 (API Level 35)
- **Architecture**: ARM64 (Cortex-A55)
- **Partitioning**: A/B with Dynamic Partitions
- **Encryption**: File-Based Encryption (FBE) with Metadata Encryption

## Prerequisites

This repository is configured to build TWRP recovery using GitHub Actions. The device tree is located in `motorola/kansas/`.

## Building via GitHub Actions

### Method 1: Using Local Device Tree (Recommended)

1. Go to **Actions** → **Recovery Build**
2. Click **Run workflow**
3. Use the following parameters:

```
MANIFEST_URL: https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp
MANIFEST_BRANCH: twrp-12.1
DEVICE_TREE_URL: (leave blank to use local tree)
DEVICE_TREE_BRANCH: main
DEVICE_PATH: device/motorola/kansas
COMMON_TREE_URL: (leave blank)
COMMON_PATH: (leave blank)
DEVICE_NAME: kansas
MAKEFILE_NAME: twrp_kansas
BUILD_TARGET: recovery
```

4. Click **Run workflow** to start the build

### Method 2: Using Remote Device Tree

If you've pushed the device tree to a separate repository:

```
MANIFEST_URL: https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp
MANIFEST_BRANCH: twrp-12.1
DEVICE_TREE_URL: https://github.com/YOUR_USERNAME/android_device_motorola_kansas
DEVICE_TREE_BRANCH: main
DEVICE_PATH: device/motorola/kansas
COMMON_TREE_URL: (leave blank)
COMMON_PATH: (leave blank)
DEVICE_NAME: kansas
MAKEFILE_NAME: twrp_kansas
BUILD_TARGET: recovery
```

## Device Tree Structure

The device tree includes:

- **BoardConfig.mk**: Board configuration (architecture, kernel, partitions, TWRP settings)
- **device.mk**: Device-specific packages and configurations
- **twrp_kansas.mk**: TWRP product configuration
- **recovery/root/system/etc/**: Recovery fstab and TWRP flags
- **prebuilt/**: Prebuilt kernel, DTB, and DTBO images
- **vendor/motorola/kansas/**: Vendor-specific files
- **sepolicy/**: SELinux policies for recovery

## Key Features

- ✅ Android 15 support (API Level 35)
- ✅ A/B partition support with init_boot
- ✅ Dynamic partition support
- ✅ File-based encryption (FBE) decryption
- ✅ Metadata encryption support
- ✅ Fastbootd support
- ✅ MediaTek MT6835 platform
- ✅ TWRP portrait_hdpi theme
- ✅ ADB support in recovery

## Build Output

After the build completes, the following files will be available in the Release:

- `recovery.img` - TWRP recovery image
- `*.zip` - Installable recovery ZIP (if available)
- `*vendor*.img` - Vendor boot images (if applicable)

## Flashing Instructions

### Flash via Fastboot

```bash
# Boot to bootloader
adb reboot bootloader

# Flash recovery to boot partition (for A/B devices)
fastboot flash boot_a recovery.img
fastboot flash boot_b recovery.img

# Reboot to recovery
fastboot reboot recovery
```

### Flash via Fastbootd

```bash
# Boot to fastbootd
adb reboot fastboot

# Flash recovery
fastboot flash boot recovery.img

# Reboot to recovery
fastboot reboot recovery
```

## TWRP Configuration

The device is configured with the following TWRP settings:

- **Theme**: portrait_hdpi
- **Screen Blank on Boot**: Yes
- **Brightness Control**: Supported (150/255 default)
- **Crypto Support**: Full FBE + Metadata decryption
- **Repack Tools**: Included
- **ResetProp**: Included
- **APEX**: Excluded for compatibility
- **Haptic Feedback**: AIDL support enabled

## Troubleshooting

### Build Fails with Missing Dependencies

The workflow automatically syncs device dependencies. If it fails:
1. Check the `twrp.dependencies` file in the device tree
2. Ensure all dependencies are available in the TWRP manifest

### Decryption Not Working

The device tree is configured for FBE decryption with:
- File-based encryption (FBE)
- Metadata encryption
- Keymaster 4.0 support

If decryption fails, ensure you're using the correct TWRP manifest branch (twrp-12.1).

### Boot Loops

If the device boot loops after flashing:
1. Ensure you flashed to both A and B slots
2. Try wiping cache and dalvik cache
3. Verify the kernel and DTB are compatible

## Contributing

To contribute improvements to the device tree:

1. Make changes in `motorola/kansas/`
2. Test the build via GitHub Actions
3. Submit a pull request

## Support

For issues specific to this device tree, please open an issue in this repository.

For general TWRP questions, visit:
- [TWRP Official Website](https://twrp.me/)
- [XDA Forums](https://forum.xda-developers.com/)

## Credits

- TWRP Team for the recovery project
- MediaTek for platform support
- Motorola for the device

## License

```
Copyright (C) 2025 The Android Open Source Project
Copyright (C) 2025 The TWRP Open Source Project

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
