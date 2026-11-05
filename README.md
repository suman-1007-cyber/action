# FinELikEWinE-MT6768-ActioN_69

Build your own flashable kernel zip on GitHub, no computer needed.
This is a GitHub Actions builder for the **FinELikEWinE_69** kernel (Linux 4.19, MediaTek MT6768,
Xiaomi Redmi 9 / Redmi Note 9) with KernelSU, SUSFS and NoMount.

> Tested on **lancelot** (Redmi 9) with an Android 17 custom ROM. **merlin** builds from the same
> config but is untested. Use a recovery (OrangeFox / TWRP) to flash.

---

## Quick start

1. **Get your own copy.** Click **Use this template -> Create a new repository** (best), or **Fork**.
   - If you used **Fork**: open the **Actions** tab and click **"I understand my workflows, enable them"**.
2. **Edit `ksun.env`** (the only file you normally touch). See the table below.
3. **Run the build:** **Actions -> Build Kernel (mt6768) -> Run workflow**.
   Leave the three override boxes **empty** unless you know why you need them. Leave
   "Publish a GitHub release" unchecked for a first test.
4. **Download the zip:** open the finished run, scroll to **Artifacts**, and download the zip named
   `FinELikEWinE_69-<device>-<date>.zip`. If your browser gives you a zip that contains another zip,
   extract it and flash the inner one.
5. **Flash it in recovery.** Make a **boot backup first** (OrangeFox: Backup -> Boot).

---

## What to edit in `ksun.env`

| Key | What it is |
|---|---|
| `KERNEL_SOURCE`, `KERNEL_SOURCE_BRANCH` | Git URL and branch of the kernel source to build |
| `DEVICES` | `lancelot`, `merlin`, or `lancelot,merlin` (each builds `vendor/<device>.config`) |
| `LOCALVERSION` | The name added to the kernel version (shown in Settings -> About phone) |
| `KERNELSU_SETUP_SOURCE`, `KERNELSU_TAG` | The KernelSU setup script URL and the branch/tag it checks out |
| `ANYKERNEL3_SOURCE`, `ANYKERNEL3_BRANCH` | The installer template that becomes your flashable zip |
| `CLANG_SOURCE` | Download link of the Clang toolchain archive |
| `ENABLE_BBR_FQ` | `true` = TCP BBR as default congestion control and `fq` as default qdisc |
| `ENABLE_BFQ` | `true` = adds BFQ as a selectable I/O scheduler (default scheduler unchanged) |
| `ENSURE_EXTRA_FEATURES` | `true` = pins WireGuard, PSI and exFAT on |
| `DISABLE_DEBUG_OPTIONS` | `true` = turns off `CFG80211_DEVELOPER_WARNINGS` and `SCHEDSTATS` |

Rules: every key must appear **once** (if a key is repeated, the last value wins), and don't put
spaces around `=`.

### Optional run settings (Run workflow dialog)
- **Override devices**: build only some devices this time.
- **Override KERNELSU_TAG / SETUP_SOURCE**: try another KernelSU branch without editing files.
- **Publish a GitHub release**: attach the zip to a release (direct download, no artifact wrapper).

---

## Build your own branded version

1. Put your kernel source, KernelSU copy and AnyKernel3 copy in your own GitHub repos.
2. Point `KERNEL_SOURCE`, `KERNELSU_SETUP_SOURCE` and `ANYKERNEL3_SOURCE` in `ksun.env` at them.
3. In your KernelSU copy, `kernel/setup.sh` must clone **your** repo into a folder named `KernelSU`:
   `git clone https://github.com/<you>/<your-kernelsu-repo> KernelSU`
4. Change `LOCALVERSION` to your name. Edit `anykernel.sh` and `banner` in your AnyKernel3 copy.

---

## Using it after flashing
- Use a KernelSU-Next / xxKSU compatible manager (the manager must match the driver version).
- SUSFS features need a SUSFS module such as BRENE. Install a metamodule (e.g. NoMount) for modules.

## Common problems

| Problem | Likely cause |
|---|---|
| `cd: .../KernelSU: No such file or directory` | `setup.sh` clone line is missing the `KernelSU` folder name at the end |
| "Kernel update required" in the manager | Manager and kernel driver versions don't match |
| Kernel name shows twice in Settings | `LOCALVERSION` is applied twice (config and environment) |
| BRENE says SUSFS is not supported | The KernelSU driver you built has no SUSFS support |
| Build fails | Open the failed step, copy the first line containing `error:` |

---

## Security: no tokens in this repo

- Never commit a password, personal access token, or API key. Not in `ksun.env`, not in the workflow.
- The workflow only needs GitHub's built-in token, which GitHub provides automatically. You don't create one.
- If you ever add a secret (for example a Telegram bot token), store it under
  **Settings -> Secrets and variables -> Actions** and read it as `${{ secrets.NAME }}`.
- If you used a personal access token to push code, delete it afterwards under
  **GitHub Settings -> Developer settings -> Personal access tokens**.

---

## Credits

This project stands on other people's work. Thank you to:

- **ShockWAVE Kernel**, [@MrShockWAVEog](https://github.com/MrShockWAVEog): kernel base, installer template,
  KernelSU fork and toolchain mirror
- **@sshprb** and **@emred_n**: kernel tree
- **@Jbub5**: the original GitHub Action
- **KernelSU**: tiann and contributors
- **KernelSU (non-GKI driver)**: [backslashxx](https://github.com/backslashxx/KernelSU)
- **KernelSU-Next**: the KernelSU-Next team
- **SUSFS**: simonpunk (susfs4ksu)
- **AnyKernel3**: osm0sis and contributors

## License

The Linux kernel and KernelSU are **GPL-2.0**: keep their license files and keep your source public.
Check each project's own license before redistributing its code.
