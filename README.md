🦊 MKVenomPro OrangeFox Builder

<p align="center">
  <img src="https://img.shields.io/badge/🦊_OrangeFox-AUTOMATED_RECOVERY_BUILDER-FF8C00?style=for-the-badge" alt="OrangeFox Builder" />
  <img src="https://img.shields.io/badge/⚙️_GitHub_Actions-CI/CD-2088FF?style=for-the-badge&logo=githubactions&logoColor=white" alt="GitHub Actions" />
  <img src="https://img.shields.io/badge/🤖_Android-Recovery_BUILD-3DDC84?style=for-the-badge&logo=android&logoColor=white" alt="Android Recovery" />
</p><p align="center">
  <strong>Build OrangeFox Recovery automatically from any compatible device tree.</strong>
</p><p align="center">
  <sub>
    Dynamic Device Tree • Multiple OrangeFox Branches • Magisk Integration • ccache • LDCheck • Auto Patching • Releases • Mirrors • Telegram
  </sub>
</p>---

🚀 What Is This?

MKVenomPro OrangeFox Builder is an automated GitHub Actions build system designed to turn a compatible Android recovery/device tree into a ready-to-use OrangeFox Recovery image with minimal manual configuration.

Instead of maintaining a separate workflow for every device, this builder accepts the important parameters at build time:

OrangeFox Branch
      ↓
Device Tree Repository
      ↓
Device Tree Branch
      ↓
Build Target
      ↓
Optional Magisk
      ↓
Optional LDCheck
      ↓
OrangeFox Build
      ↓
Validation / Patching
      ↓
Recovery Image
      ↓
GitHub Release
      ↓
Optional Mirrors

The goal is simple:

«One builder. Multiple devices. Multiple branches. Automated recovery builds.»

---

🧠 Builder Architecture

<p align="center">┌───────────────────────────────────────────────────────────────┐
│                    🦊 MKVenomPro BUILDER                      │
├───────────────────────────────────────────────────────────────┤
│                                                               │
│  🎛️ INPUTS                                                    │
│  ├── OrangeFox Branch                                         │
│  ├── Device Tree Repository                                   │
│  ├── Device Tree Branch                                       │
│  ├── Build Target                                             │
│  ├── Latest Magisk                                            │
│  ├── LDCheck                                                  │
│  └── Upload / Mirror                                          │
│                                                               │
│                         ↓                                     │
│                                                               │
│  📥 SOURCE INTAKE                                              │
│  ├── OrangeFox Sync                                           │
│  └── Dynamic Device Tree Clone                                │
│                                                               │
│                         ↓                                     │
│                                                               │
│  🔍 DEVICE DETECTION                                           │
│  ├── PRODUCT_NAME                                             │
│  ├── PRODUCT_BRAND                                            │
│  ├── PRODUCT_DEVICE                                           │
│  └── URL-based fallback detection                             │
│                                                               │
│                         ↓                                     │
│                                                               │
│  🏗️ BUILD ENGINE                                               │
│  ├── Android Build Environment                                │
│  ├── ccache                                                   │
│  ├── Swap                                                     │
│  ├── Magisk                                                   │
│  └── OrangeFox                                                │
│                                                               │
│                         ↓                                     │
│                                                               │
│  🧬 POST-BUILD ENGINE                                         │
│  ├── Optional library patching                                │
│  ├── DT_NEEDED modification                                   │
│  ├── Recovery ramdisk verification                            │
│  └── Target image rebuild                                     │
│                                                               │
│                         ↓                                     │
│                                                               │
│  📦 DELIVERY                                                   │
│  ├── GitHub Release                                            │
│  ├── oshi.at                                                   │
│  ├── Gofile                                                    │
│  └── Telegram notifications                                    │
│                                                               │
└───────────────────────────────────────────────────────────────┘

</p>---

⚡ Features

Feature| Status
🦊 OrangeFox automated builds| ✅
🌿 Multiple OrangeFox branches| ✅
📱 Dynamic device tree support| ✅
🔐 Public device trees| ✅
🔒 Private GitHub device trees| ✅
🎯 Custom build targets| ✅
🧙 Automatic device detection| ✅
🔄 Device-tree branch selection| ✅
⚡ ccache acceleration| ✅
💾 Automatic swap allocation| ✅
🧹 Runner disk cleanup| ✅
🧩 Latest Magisk integration| ✅
🧬 Optional library patching| ✅
🔍 LDCheck support| ✅
📦 Automatic artifact validation| ✅
🏷️ Automatic GitHub Releases| ✅
🔢 MD5 generation| ✅
☁️ oshi.at mirror| ✅
☁️ Gofile upload| ✅
📲 Telegram notifications| ✅
📝 Automatic build metadata| ✅

---

🎛️ Build Control Panel

The workflow is controlled entirely through GitHub Actions → Run workflow.

🌿 OrangeFox Branch

Supported branches currently include:

16.0
14.1
12.1
11.0
10.0
9.0
8.1
7.1
6.0

The workflow automatically converts the selected version into the appropriate OrangeFox synchronization branch.

Example:

16.0
 ↓
fox_16.0

---

📱 Device Tree Repository

Provide the URL of the recovery/device tree repository.

Example:

https://github.com/username/device_xiaomi_example

The builder supports both:

🌐 Public repositories
🔐 Private repositories

For private GitHub repositories, the workflow can automatically authenticate through:

GT_TOKEN

---

🌿 Device Tree Branch

Specify the branch containing the device tree.

Example:

android-12.1

This makes the builder independent from a single hardcoded device tree.

---

🎯 Build Target

Available targets:

vendorbootimage
recoveryimage
bootimage

This allows the same workflow to support different recovery deployment layouts.

---

🧠 Automatic Device Detection

One of the core features of the builder is that the workflow does not require the device codename to be manually entered.

After cloning the tree, the builder attempts to detect:

PRODUCT_NAME
PRODUCT_BRAND
PRODUCT_DEVICE

from the device makefiles.

For example:

PRODUCT_NAME := twrp_spinel
PRODUCT_BRAND := xiaomi
PRODUCT_DEVICE := spinel

The workflow can derive:

device/xiaomi/spinel

automatically.

If the makefile information cannot be detected, the builder also contains a fallback mechanism based on the repository name.

Example:

device_xiaomi_spinel

can be interpreted as:

Brand  → xiaomi
Device → spinel

This makes the builder considerably more portable between projects.

---

🔐 Private Device Trees

Private device trees are supported without requiring separate cloning logic.

The workflow configures Git authentication globally when:

GT_TOKEN

is available.

GitHub URLs are transparently redirected through the authenticated token, allowing the same build logic to work with:

Public Tree
       │
       ├──────────────┐
       │              │
Private Tree      Private Dependencies
       │              │
       └───────→ Git Authentication

No separate workflow is required.

---

🧙 Automatic Magisk Integration

The builder can optionally retrieve the latest Magisk release and provide it to the OrangeFox build system.

Toggle:

USE_LATEST_MAGISK=true

When enabled:

GitHub API
    ↓
Latest Magisk Release
    ↓
Magisk APK
    ↓
APK → ZIP
    ↓
OrangeFox Build

This avoids manually updating a Magisk ZIP every time a new release is published.

---

🧬 Smart Post-Build Patching

This builder contains an optional post-build patching system for device trees that require a specific shared-library dependency inside the recovery environment.

A device tree can provide:

lib_fix.conf

with parameters such as:

LIB_NAME=libpixelflinger.so
SYMBOL=res_get_pixel_formatv

The workflow can then:

Locate library
      ↓
Verify required symbol
      ↓
Use built library
      ↓
OR fallback to device-tree prebuilt
      ↓
Place library inside recovery
      ↓
Patch recovery DT_NEEDED
      ↓
Rebuild target image
      ↓
Verify library inside ramdisk

This is especially useful for recovery builds where a binary expects a library that is not automatically included by the standard build process.

---

🔍 LDCheck

The workflow includes optional LDCheck support.

Inputs:

LDCHECK
LDCHECKPATH

Example:

LDCHECK=true

and:

system/bin/qseecomd

This allows specific blobs/binaries to be inspected as part of the recovery build workflow.

---

⚙️ Build Environment

The workflow prepares the GitHub Actions runner automatically.

The build environment includes:

Ubuntu 22.04
Android build dependencies
OrangeFox build environment
aria2
ccache
12 GB swap
Git
curl
jq
patchelf
Android build tools

The runner also performs disk-space cleanup before the build.

This is important because Android recovery builds can consume a significant amount of temporary storage.

---

⚡ ccache Acceleration

The builder uses GitHub Actions cache support for:

/tmp/ccache

with a cache size of:

50 GB

The cache key is based on the device tree and OrangeFox branch, allowing builds from the same configuration to reuse compiled objects.

Conceptually:

First Build
───────────
Source → Compile → ccache populated

Next Build
──────────
Source → ccache HIT → Faster Build

This can significantly reduce repeated compilation time.

---

🧩 Custom Lunch Targets

Most devices automatically use:

twrp_<device>-eng

For example:

twrp_spinel-eng

However, some device trees require a custom lunch target.

For those devices, simply add:

lunch_target.txt

to the root of the device tree.

Example:

twrp_spinel-ap2a-eng

The builder will automatically detect and use it.

This avoids modifying the main workflow for device-specific lunch requirements.

---

🧹 Build Pipeline

The complete workflow follows this sequence:

🎛️ Workflow Dispatch
        ↓
🧹 Runner Cleanup
        ↓
💾 Swap Setup
        ↓
🛠️ Build Environment
        ↓
⚡ Restore ccache
        ↓
🔐 Configure Git Authentication
        ↓
🦊 Sync OrangeFox
        ↓
📱 Clone Device Tree
        ↓
🧠 Detect Device
        ↓
🧙 Download Magisk
        ↓
🏗️ Build Recovery
        ↓
🧬 Optional Patch / Rebuild
        ↓
🔍 Validate Output
        ↓
🔢 Generate MD5
        ↓
📦 Create GitHub Release
        ↓
☁️ Optional Mirrors
        ↓
📲 Telegram Notification

---

📦 Output

Depending on the selected target and device tree, the build can produce recovery-related images and OrangeFox packages.

The workflow automatically checks for:

vendor_boot.img
OrangeFox-<device>-vendor_boot.img
OrangeFox*.zip
ramdisk-recovery.*

The generated image is also renamed into a device-specific format:

OrangeFox-<device>-vendor_boot.img

Example:

OrangeFox-spinel-vendor_boot.img

---

🔢 Build Verification

Before publishing the result, the workflow performs output checks.

For valid images it generates:

MD5

and stores build information such as:

Device
OrangeFox branch
Build target
Device tree
Device tree branch
Device tree commit
Maintainer
Build date
MD5

This information is also included in the GitHub Release.

---

📦 Automatic GitHub Releases

Successful builds are automatically published as GitHub Releases.

A release contains information similar to:

🦊 OrangeFox Recovery — DEVICE

Branch:
Target:
Device Tree:
Tree Commit:
Maintainer:
Build Date:
MD5:

This makes every successful CI build independently traceable to the exact source tree commit used to produce it.

---

☁️ Mirrors

The builder can optionally upload the generated image to external mirrors.

Supported upload paths include:

oshi.at
Gofile.io

These mirrors are optional and can be controlled from the workflow inputs.

If a mirror fails, the workflow is designed to continue where appropriate instead of unnecessarily invalidating the main GitHub build.

---

📲 Telegram Build Notifications

Telegram notifications can be enabled through repository secrets.

When configured, the workflow can announce:

🦊 OrangeFox CI

✔️ Build triggered!

🌿 Branch:
🎯 Target:
👤 Maintainer:
🌲 Logs:

This gives maintainers a quick way to monitor builds without constantly opening GitHub Actions.

---

🔑 Required Secrets

For the complete feature set, configure the following GitHub repository secrets:

GT_TOKEN
TG_TOKEN
TG_CHAT_ID

GT_TOKEN

Used for authenticated GitHub operations such as private repository access and GitHub API requests.

TG_TOKEN

Telegram bot token used for build notifications.

TG_CHAT_ID

Telegram destination chat/channel ID.

«Public device-tree builds can work without private repository authentication, while some optional features require the corresponding secrets.»

---

🛠️ How To Build

1. Open GitHub Actions

Go to:

Actions

and select:

🦊 OrangeFox Builder

---

2. Run Workflow

Select:

Run workflow

Then provide:

FOX_BRANCH
DT_REPO_URL
DT_BRANCH
BUILD_TARGET
USE_LATEST_MAGISK
UPLOAD_RESULTS
LDCHECK
LDCHECKPATH

---

3. Start The Build

Once started, the workflow handles:

Source synchronization
Device detection
Dependency preparation
Compilation
Patching
Validation
Packaging
Release
Mirrors
Notifications

automatically.

---

🧪 Example Configuration

For a hypothetical Xiaomi device:

FOX_BRANCH:
16.0

DT_REPO_URL:
https://github.com/example/device_xiaomi_device

DT_BRANCH:
android-12.1

BUILD_TARGET:
vendorbootimage

USE_LATEST_MAGISK:
true

UPLOAD_RESULTS:
true

LDCHECK:
false

The builder then handles the rest automatically.

---

🧱 Repository Structure

The project intentionally keeps the main repository lightweight.

MKVenomPro_recovery_builder/
│
├── .github/
│   └── workflows/
│       └── ofox.yml
│
└── README.md

The actual OrangeFox source tree and device tree are pulled dynamically during CI.

This means the builder repository itself does not need to contain:

OrangeFox source
Device trees
Large Android dependencies
Build outputs

The repository acts as the build controller, not the Android source mirror.

---

🎯 Design Philosophy

This project follows a few simple principles:

🧩 Device Independent

The builder should not be locked to one device.

One Workflow
     ↓
Many Device Trees
     ↓
Many Devices

⚙️ Configuration Driven

Device-specific behavior should preferably be controlled through the tree itself rather than by creating another workflow.

Examples:

lunch_target.txt
lib_fix.conf

🤖 Automation First

Anything that can safely be automated should not require manual intervention.

🔍 Verify Before Release

A successful compilation is not enough.

The builder checks:

Output existence
Image integrity
Libraries
Required symbols
Ramdisk contents
MD5

before publishing where applicable.

---

🚧 Compatibility

This builder is intended for OrangeFox-compatible Android recovery/device trees.

Actual compatibility depends on the supplied tree, including:

BoardConfig
AndroidProducts
Recovery configuration
Kernel / boot architecture
Vendor requirements
OrangeFox branch compatibility
Device-specific dependencies

The builder cannot automatically fix every device-tree problem.

A broken or incomplete tree can still fail during:

Sync
Lunch
Compilation
Linking
Image packaging

---

⚠️ Important Notes

🔥 This is a CI Builder, not a recovery source tree

The repository does not contain a complete OrangeFox source checkout.

It dynamically synchronizes the required source during GitHub Actions.

🔐 Private Trees Require Authentication

If the supplied device tree is private, make sure the configured GitHub token has sufficient permissions.

🧬 Device-Specific Patches

"lib_fix.conf" is optional and should only be used when the device actually requires the specified library/dependency fix.

📱 Flash Carefully

A recovery/vendor_boot image is device-specific.

Never flash an image built for another device just because the Android version or OrangeFox branch matches.

Always verify:

Device codename
Partition layout
Boot architecture
Image type
Android base

---

🗺️ Roadmap

Possible future improvements:

[ ] Automatic device-tree compatibility checks
[ ] Automatic manifest detection
[ ] More recovery projects
[ ] Automatic changelog generation
[ ] Build artifact dashboard
[ ] Better failure diagnostics
[ ] Automatic device-tree metadata extraction
[ ] Build badges
[ ] Release statistics
[ ] More mirror providers
[ ] Automatic recovery testing

---

🤝 Contributing

Contributions, fixes, and improvements are welcome.

If you find a problem with the builder:

1. Check the GitHub Actions log.
2. Identify the failing stage.
3. Verify the supplied device tree.
4. Open an issue with the relevant log section.
5. Include the OrangeFox branch and device tree branch.

For device-specific fixes, prefer making the tree self-contained whenever possible rather than adding hardcoded logic to the global builder.

---

🦊 Credits

This project builds upon the OrangeFox recovery ecosystem and Android Open Source tooling.

Special thanks to:

- OrangeFox Recovery developers
- Android Open Source Project contributors
- TWRP ecosystem contributors
- Magisk developers
- GitHub Actions
- Device tree maintainers
- Everyone testing and maintaining Android recoveries

---

📊 Build Stack

<p align="center">
  <img src="https://skillicons.dev/icons?i=linux,androidstudio,bash,python,git,github,githubactions,docker&perline=8" alt="Build Stack" />
</p><p align="center">🐧 Linux
   +
🤖 Android Build System
   +
🦊 OrangeFox
   +
⚡ GitHub Actions
   +
🧠 Dynamic Device Detection
   +
🧬 Optional Binary Patching
   +
📦 Automated Releases

</p>---

🔥 MKVenomPro Recovery Engineering

<p align="center">
  <strong>🦊 BUILD • 🧠 DETECT • 🧬 PATCH • 🔍 VERIFY • 📦 RELEASE</strong>
</p><p align="center">
  <sub>
    One workflow. Multiple devices. Fully automated OrangeFox recovery builds.
  </sub>
</p><p align="center">
  <strong>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</strong>
</p><p align="center">
  <strong>Made for Android Recovery Developers & Device Tree Maintainers</strong>
</p>
