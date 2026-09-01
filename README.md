<div align="center">🦊 MKVenomPro Recovery Builder

Automated OrangeFox Recovery CI

<p>
  <img src="https://img.shields.io/badge/🦊_OrangeFox-RECOVERY_BUILDER-FF8C00?style=for-the-badge" />
  <img src="https://img.shields.io/badge/⚙️_GitHub_Actions-AUTOMATED-2088FF?style=for-the-badge&logo=githubactions&logoColor=white" />
  <img src="https://img.shields.io/badge/🤖_Android-RECOVERY-3DDC84?style=for-the-badge&logo=android&logoColor=white" />
</p><p>
  <strong>🌲 SOURCE → 🧠 DETECT → 🏗️ BUILD → 🧬 PATCH → 📦 RELEASE</strong>
</p></div>---

🚀 About

MKVenomPro Recovery Builder is an automated GitHub Actions workflow for building OrangeFox Recovery from external Android device trees.

No device-specific build workflow is required — simply provide the tree and build configuration.

---

⚡ Features

<table>
<tr>
<td align="center">🦊<br/><b>OrangeFox</b><br/><sub>Multiple branches</sub></td>
<td align="center">🌲<br/><b>Device Trees</b><br/><sub>Dynamic sources</sub></td>
<td align="center">🧠<br/><b>Auto Detect</b><br/><sub>Device identity</sub></td>
<td align="center">⚡<br/><b>ccache</b><br/><sub>Faster builds</sub></td>
</tr>
<tr>
<td align="center">🧙<br/><b>Magisk</b><br/><sub>Latest release</sub></td>
<td align="center">🧬<br/><b>LDCheck</b><br/><sub>Optional patching</sub></td>
<td align="center">🔍<br/><b>Validation</b><br/><sub>Image & MD5</sub></td>
<td align="center">📦<br/><b>Release</b><br/><sub>Automatic upload</sub></td>
</tr>
</table>---

🎛️ Build Flow

<p align="center">┌────────────┐
│  Workflow  │
│   Inputs   │
└─────┬──────┘
      ↓
┌────────────┐
│  OrangeFox │
│   + Tree   │
└─────┬──────┘
      ↓
┌────────────┐
│    Auto    │
│   Detect   │
└─────┬──────┘
      ↓
┌────────────┐
│   BUILD    │
└─────┬──────┘
      ↓
┌────────────┐
│ 🧬 PATCH   │
│  Optional  │
└─────┬──────┘
      ↓
┌────────────┐
│  VERIFY    │
│  + MD5     │
└─────┬──────┘
      ↓
┌────────────┐
│   RELEASE  │
│ + MIRRORS  │
└────────────┘

</p>---

🎯 Workflow Inputs

🌿 OrangeFox Branch
🌲 Device Tree URL
🌿 Device Tree Branch
🎯 Build Target
🧙 Latest Magisk
🧬 LDCheck
☁️ Upload Results

Supported targets:

vendorbootimage
recoveryimage
bootimage

---

🧠 Smart Device Detection

The builder automatically attempts to detect:

PRODUCT_NAME
PRODUCT_BRAND
PRODUCT_DEVICE

It can also use optional device-tree configuration such as:

lunch_target.txt
lib_fix.conf

This keeps the main workflow device-independent.

---

🔐 Optional Integrations

🔒 Private GitHub Trees → GT_TOKEN
📲 Telegram Notifications → TG_TOKEN / TG_CHAT_ID
☁️ oshi.at
☁️ Gofile
🧙 Latest Magisk

---

📦 Output

Successful builds can automatically produce and publish:

🦊 OrangeFox Recovery Image
📦 OrangeFox ZIP
🔢 MD5
📋 Build Metadata

Releases can include:

Device
OrangeFox Branch
Device Tree
Commit
Build Target
MD5

---

🛠️ Requirements

Only a compatible Android recovery/device tree is required.

The GitHub Actions runner automatically prepares the build environment, dependencies, swap, and ccache.

---

⚠️ Disclaimer

Always verify the device codename, partition layout, Android base, and image type before flashing.

A successful build does not guarantee compatibility with every device configuration.

---

<div align="center">🦊 BUILD SMART • PATCH PRECISELY • SHIP CLEAN

<sub>MKVenomPro Recovery Builder — Automated OrangeFox CI</sub>

</div>
