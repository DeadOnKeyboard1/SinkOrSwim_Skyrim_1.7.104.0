# Sink Or Swim - Skyrim 1.7.104.0 (SKSE 2.3.1) Port

Native SKSE plugin port of **Sink Or Swim** for **The Elder Scrolls V: Skyrim Special Edition / Anniversary Edition (Runtime 1.7.104.0)** and **SKSE 2.3.1**.

Original mod by **Loki** on [Nexus Mods (Mod #42962)](https://www.nexusmods.com/skyrimspecialedition/mods/42962).

---

## Features

- **Walk Underwater:** Wearing a full set of heavy armor (head, chest, hands, feet) causes you to sink to the waterbed and walk/fight on the ocean or river floor instead of swimming.
- **Surface by Removing Armor:** Remove at least one piece of heavy armor to float back up to the surface and swim.
- **Wading Slowdown:** Characters and NPCs slow down dynamically when wading through shallow water.
- **Native Engine Hooking:** Implemented cleanly via SKSE trampoline hooks in `Actor::Update`. No Papyrus script lag.

---

## 1.7.104.0 Port Highlights & Fixes

1. **AE 1.7.104.0 Register Convention Adaptation:**
   - In Skyrim SE (1.5.97), the swimming state was held in `r13b`.
   - In Skyrim AE (1.7.104.0), the game compiler assigned the swimming flag to the **`sil`** register at offset `+0x69D` and stored it in the stack frame at `[rbp + 0x1D0]`, before testing `sil` at `+0x6E1` to toggle the `0x800000` swimming bit.
   - The hook at `ActorUpdate + 0x69D` now clears `sil` and writes `0` to `[rbp + 0x1D0]` when heavy armor is worn, cleanly disabling swimming and enabling walking on the riverbed.
2. **Robust Equipment Detection (`ActorHasPowerArmor`):**
   - Directly inspects the actor's equipped items across head, body, hands, and feet for heavy armor type (`kHeavyArmor`).
   - Works consistently with both vanilla and mod-added heavy armors, with graceful fallback to `WaterSlowdownSink` active effect checks.
3. **SKSE 2.3.1 & Address Library Compatibility:**
   - Fully declared for Address Library and SKSE 2.3.1 runtime validation without SE/AE ODR layout mismatches.
4. **Standalone Replacer Packaging:**
   - Bundles `SinkOrSwim.esp` alongside `SinkOrSwim.dll` for seamless installation in Vortex and Mod Organizer 2.

---

## Requirements

- **The Elder Scrolls V: Skyrim Special Edition / Anniversary Edition** (Runtime `1.7.104.0`)
- **SKSE64** (Runtime `2.3.1`)
- **Address Library for SKSE Plugins** (All in one / AE version)
- **`SinkOrSwim.esp`** (Included in the release)

> **Note:** Do not use standalone *Wade In Water* or *Wade In Water Redone* simultaneously with *Sink Or Swim*, as Sink Or Swim already incorporates wading slowdown mechanics internally.

---

## Building from Source

### Prerequisites
- Visual Studio 2022 (MSVC toolset v143+)
- Windows SDK (10.0 or 11.0)
- CMake 3.23 or newer
- [vcpkg](https://github.com/microsoft/vcpkg)
- [CommonLibSSE-NG](https://github.com/alandtse/CommonLibVR) (branch `ng`, commit `1504349d` / v8.0.0)

### Build Commands

```powershell
# Configure CMake
cmake -B build -S . `
  -DCMAKE_TOOLCHAIN_FILE="$env:VCPKG_ROOT/scripts/buildsystems/vcpkg.cmake" `
  -DVCPKG_TARGET_TRIPLET="x64-windows-static-md" `
  -DCMAKE_BUILD_TYPE=Release

# Compile DLL and PDB
cmake --build build --config Release
```

The compiled plugin and debug symbols will be output to:
- `build/Release/SinkOrSwim.dll`
- `build/Release/SinkOrSwim.pdb`

---

## Credits & License

- Original mod concept and source code by **Loki** ([LokiWasHere](https://www.nexusmods.com/skyrimspecialedition/users/60161421)).
- Skyrim 1.7.104.0 compatibility update by **DeadOnKeyboard1**.

### License

This Skyrim 1.7.104.0 compatibility build is distributed under the
**GNU General Public License v3.0 or later (GPL-3.0-or-later)**.

This build uses and statically links against **CommonLibSSE-NG 8.0.0**,
revision:

`1504349dddfc622d4d25704bba19e2ade669dc5a`

CommonLibSSE-NG 8.0.0 is distributed under **GPL-3.0-or-later**
with its applicable Modding Exception and GPL-3.0 Linking Exception.

The original **Sink Or Swim** source code by Loki was released under
the **MIT License**. The original copyright and MIT license notice are
preserved in:

`licenses/SinkOrSwim-MIT.txt`

CommonLibSSE-NG licensing files:

- `licenses/CommonLibSSE-NG/COPYING.txt`
- `licenses/CommonLibSSE-NG/EXCEPTIONS.md`
- `licenses/CommonLibSSE-NG/LICENSE-MIT.txt`

Additional third-party license notices are available under:

`licenses/third-party/`
