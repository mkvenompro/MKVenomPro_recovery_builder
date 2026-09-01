<div align="center">

# 🦊🔧 MKVenomPro Recovery Builder

### Automated custom-recovery CI for **OrangeFox · TWRP · PitchBlack · SkyHawk**

<p>
  <img src="https://img.shields.io/badge/Recovery-OrangeFox%20%7C%20TWRP%20%7C%20PBRP%20%7C%20SHRP-FF8C00?style=for-the-badge&logo=android&logoColor=white" />
  <img src="https://img.shields.io/badge/GitHub%20Actions-Automated-2088FF?style=for-the-badge&logo=githubactions&logoColor=white" />
  <img src="https://img.shields.io/badge/ccache-HIT-brightgreen?style=for-the-badge" />
</p>

**🌲 Sync → 🧠 Detect → 🏗️ Build → 🔍 Validate → 📦 Release**

</div>

---

## 📖 About

**MKVenomPro Recovery Builder** is a set of self-contained GitHub Actions workflows that compile **custom recoveries** (OrangeFox, TWRP, PitchBlack, SHRP) from any Android device tree — fully automated from sync to release.

No device-specific workflow is required. Pick a recovery, supply the device-tree URL + branch, and the pipeline takes care of the environment, source sync, device detection, build, validation, caching, and publishing.

The pipeline is production-tested on both **GitHub-hosted** and **self-hosted** runners, with ccache restore to slash rebuild times.

---

## ⚡ Features

| | Benefit |
|---|---|
| 🦊 / 🧡 / ⬛ / 🔷 | **4 independent builders** for OrangeFox, TWRP, PitchBlack (PBRP) and SkyHawk (SHRP) |
| 🌲 | **Dynamic device trees** — public or private, any repo, any branch |
| 🧠 | **Auto-detection** of device identity from the tree (`PRODUCT_*` macros) |
| ⚡ | **ccache** with disk-capped sizing, permissions repair and safe cache restore |
| 🧙 | **Latest Magisk** APK bundling (optional) |
| 🧬 | **LDCheck** — missing shared-library scans (optional) |
| 🔍 | **Built-image validation** (presence, size, MD5) |
| 📦 | **GitHub Releases + oshi.at + Gofile mirrors** |
| 📲 | **Live Telegram notifications** (start / progress / completion) |
| 🖥️ | **GitHub-hosted or self-hosted** runners, configurable swap up to 200 GB |

---

## 🧱 The Builders

| Workflow | Recovery | Manifest source | Manifest branches | Lunch target | Build |
|---|---|---|---|---|---|
| [`OrangeFox-OFRP.yml`](.github/workflows/OrangeFox-OFRP.yml) | 🦊 OrangeFox | `gitlab.com/OrangeFox/sync.git` | `16.0 · 14.1 · 12.1 · 11.0 · 10.0 · 9.0 · 8.1 · 7.1 · 6.0` | `twrp_<device>-eng` | `mka adbd <target>` |
| [`TeamWin-TWRP.yml`](.github/workflows/TeamWin-TWRP.yml) | 🧡 TWRP | `minimal-manifest-twrp` | `twrp-12.1 · twrp-11 · twrp-10 · omni-9.0 · 8.1 · 7.1 · 6.0` | `twrp_<device>-eng` | `mka <target>` |
| [`PitchBlack-PBRP.yml`](.github/workflows/PitchBlack-PBRP.yml) | ⬛ PitchBlack | `PitchBlackRecoveryProject/manifest_pb` | `android-12.1 · 11.0 · 10.0 · 9.0` | `pb_<device>-eng` | `mka pbrp` |
| [`SkyHawk-SHRP.yml`](.github/workflows/SkyHawk-SHRP.yml) | 🔷 SkyHawk | `SHRP-Reborn/manifest` (modern) · `SHRP/manifest` (legacy) | `shrp-12.1 · v3_11.0 · android-9.0` | `twrp_<device>-eng` | `mka <target>` |

> Each workflow shares the same robust skeleton (ccache, swap, LDCheck, mirrors, Telegram) and only differs in the recovery source, manifest branch and output naming.

---

## 🎛️ Build Flow

```text
   ┌─────────────────┐
   │ Workflow Inputs │  → recovery, manifest branch, device tree, target…
   └────────┬────────┘
            ↓
   ┌─────────────────┐
   │ Runner Prep     │  → deps, swap, ccache restore
   └────────┬────────┘
            ↓
   ┌─────────────────┐
   │ Source Sync     │  → repo init + repo sync (correct manifest per recovery)
   └────────┬────────┘
            ↓
   ┌─────────────────┐
   │ Clone Device T  │  → validated clone (public/private), auto-detect device
   └────────┬────────┘
            ↓
   ┌─────────────────┐
   │ BUILD           │  → envsetup → lunch → make clean → mka <target>
   └────────┬────────┘
            ↓
   ┌─────────────────┐
   │ Validate + MD5  │  → image presence, size, checksum
   └────────┬────────┘
            ↓
   ┌─────────────────┐
   │ Release/Mirror  │  → GitHub Release, oshi.at, Gofile
   └─────────────────┘
```

---

## 🚀 Quick Start

1. **Fork or open** this repository on GitHub.
2. Go to the **Actions** tab.
3. Pick the recovery you want: e.g. **🦊 OrangeFox OFRP Builder**, **🧡 TWRP Builder**, **⬛ PitchBlack Builder**, or **🔷 SkyHawk Builder**.
4. Click **Run workflow**, fill in the inputs, and wait for the build.
5. The resulting image (and optional zip) is published to a **GitHub Release** under the run's tag, plus mirrors if enabled.

---

## 🎯 Workflow Inputs

All four builders accept the same set of inputs (the manifest-branch option is named per recovery).

| Input | Description | Example |
|---|---|---|
| **Manifest branch** (`FOX_BRANCH` / `TWRP_MANIFEST_BRANCH` / `PBRP_BRANCH` / `SHRP_BRANCH`) | Source manifest branch for the chosen recovery | `12.1` / `twrp-12.1` / `android-12.1` / `shrp-12.1` |
| **DT_REPO_URL** | Device tree repository (public or private) | `https://github.com/OWNER/android_device_XIAOMI_spinel` |
| **DT_BRANCH** | Device tree branch | `android-12.1` |
| **BUILD_TARGET** | Image to build | `recoveryimage` · `bootimage` · `vendorbootimage` |
| **USE_LATEST_MAGISK** | Bundle the newest Magisk zip into the build | `true` / `false` |
| **UPLOAD_RESULTS** | Publish to GitHub Release + mirrors | `true` / `false` |
| **LDCHECK** | Run missing-libraries scan | `true` / `false` |
| **LDCHECKPATH** | Blob path to scan against | `system/bin/qseecomd` |
| **RUNNER_TYPE** | `github` (hosted) or `self-hosted` | `github` |
| **SELF_HOSTED_LABEL** | Label of your self-hosted runner | `self-hosted` |
| **CCACHE_SIZE** | ccache size budget (`20G…200G`) | `50G` |
| **REPO_SYNC_JOBS** | `repo sync` parallelism (1–4) | `2` |
| **SWAP_SIZE** | Swap for self-hosted (GB, `0` = off). Hosted is fixed at 12 GB | `0` |
| **EXTRA_CMD** | Extra `export`s before `lunch` | `FOX_VARIANT=...` |

### Supported build targets

```
recoveryimage   → out/target/product/<device>/recovery.img
bootimage       → out/target/product/<device>/boot.img
vendorbootimage → out/target/product/<device>/vendor_boot.img
```

---

## 🧠 Smart Device Detection

When a tree is cloned, the builder inspects it and automatically extracts:

- `PRODUCT_NAME`
- `PRODUCT_BRAND`
- `PRODUCT_DEVICE`
- `PRODUCT_MODEL` / `PRODUCT_MANUFACTURER`

Optional per-tree overrides are honoured when present:

- `lunch_target.txt` — forces a custom lunch target
- `BoardConfig.mk` — standard recovery flags (stale `BOARD_RAMDISK_USE_LZ4` is cleared)

The tree is placed at the canonical `device/<brand>/<codename>` path, so no manual configuration is needed.

---

## 🔐 Integrations

| Secret / Config | Purpose |
|---|---|
| **`GT_TOKEN`** | `read-only` GitHub token to clone **private** device trees (auto-detected) |
| **`TG_TOKEN` + `TG_CHAT_ID`** | Live Telegram notifications (start / progress / completion) |
| `oshi.at` / `Gofile` | Automatic file mirrors for every successful build |
| **Magisk** | Latest official Magisk APK pulled at build time |

### Self-hosted runners

- Labels: set `RUNNER_TYPE=self-hosted` and `SELF_HOSTED_LABEL`.
- Use `SWAP_SIZE` to control swap (up to `200` GB).
- ccache keys are stable per recovery-branch and shared across runs for fast rebuilds on both runner types.

---

## 📦 Output & Release

Successful builds verify the image and can publish:

- Recovery image (`<RECOVERY>-<device>-<target>.img`)
- Recovery installer zip (when produced by the source)
- MD5 checksum
- Full build metadata (recovery version, target, tree URL, tree branch, commit, maintainer, build date)

The GitHub Release body is auto-generated per run (`build-<run_id>` tag).

---

## 🛠️ Requirements

- A compatible **Android recovery/device tree** (TWRP-based trees work for TWRP/SHRP; PBRP uses `pb_*.mk`, OrangeFox Standard/OrangeFox trees for OFRP).
- **GitHub repository** — everything else (environment, swap, ccache, `repo`) is prepared automatically on the runner.

---

## ⚠️ Disclaimer

Always verify the **device codename**, **partition layout**, **Android base** and **image type** before flashing.

A successful build does not guarantee compatibility with every device configuration. Use at your own risk.

---

<div align="center">

**BUILD SMART · PATCH PRECISELY · SHIP CLEAN**

<sub>MKVenomPro Recovery Builder — Automated Custom-Recovery CI</sub>

</div>