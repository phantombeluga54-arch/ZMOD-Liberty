<div align="center">

# 🌐 ZMOD-Liberty — Android Mali Multiplayer Edition

![Platform](https://shields.io)
![GPU](https://shields.io)
![Multiplayer](https://shields.io)
![Episodes](https://shields.io)

**The first Mali-compatible Android port of Liberty Recompiled with restored 64-player LAN & Community online multiplayer.**

</div>

---

## ✨ Features & What This Fork Adds

* **🔌 Restored Multiplayer Engine:** Compiles `libcurl`, `OpenSSL`, and `nlohmann_json` directly into the Android `arm64-v8a` native library instead of stubbing out Xbox Live as offline-only.
* **📡 Fixed LAN Lobby Hosting (`hosted_sessions_` fix):** Patches an upstream bug in `LanSessionDirectory::Join` where hosting a LAN session failed with `result=1627` when the host joined their own lobby.
* **👥 Expanded 64-Player Lobbies:** Supports up to **64 players** per session across **Party Mode, Free Mode, Deathmatch, Team Deathmatch, GTA Race, Race, Cops 'n' Crooks, Hangman's NOOSE, Bomb Da Base II, and Deal Breaker**.
* **🎨 Full Mali / Dimensity Vulkan Support:** Built on `ZMOD-Liberty` with custom Vulkan driver proxy and shader fixes so Dimensity & Mali GPUs run smoothly.
* **🎮 All Episodes Supported:** Works across **Grand Theft Auto IV**, **The Lost and Damned (TLAD)**, and **The Ballad of Gay Tony (TBOGT)**.
* **🏷️ Custom Player Names & Session Settings:** Set your own display name, host public or private lobbies, and customize weather, traffic density, police wanted levels, and weapons.

---

## 🛠️ Step-by-Step Multiplayer Setup Tutorial

### Step 1: Install the APK & Boot Once
1. Download **`ZMOD-Liberty-0.5.9.3-mali.apk`** and the pre-made **`args.txt`** template from the **[Releases](../../releases)** tab, then install the APK.
2. Open the app, select your game folder, and boot into single-player once so Android creates the `Android/data/com.libertyrecomp/files/` folders.
3. Force-close the game completely.

### Step 2: Configure `args.txt` (Choose LAN or Community Server)
Drop the **`args.txt`** file (or create a new text file named `args.txt`) inside **`Android/data/com.libertyrecomp/files/`** with one of the two setups below:

#### Option A: LAN & Online Virtual LAN (ZeroTier / Tailscale) — *Recommended*
Use this to play over local Wi-Fi **or** over the internet with friends using free **ZeroTier One** or **Tailscale** virtual LAN apps:
```text
--gta4_multiplayer_backend=lan
--gta4_player_name=YourNameHere
```

#### Option B: Custom Community Server (`libserver`)
If you are hosting your own `libserver` instance or connecting to a community-hosted server URL:
```text
--gta4_multiplayer_backend=community
--gta4_community_url=https://your-server-url-here
--gta4_player_name=YourNameHere
```

### Step 3: Host or Join a Lobby In-Game
1. Launch **Liberty Recompiled** and load into single-player.
2. Pull up your **in-game phone** (D-Pad Up) and select **Multiplayer**.
3. **Important — Choosing the Right Mode:**
   * **Playing Solo / Drop-In Free Roam:** Select **Party Mode**! Party Mode lets you load straight into Liberty City by yourself right away while friends join anytime.
   * **Playing with 2+ Players:** Select **Player Match -> Create Match** (Free Mode, Deathmatch, GTA Race, Cops 'n' Crooks, etc.). Note that Player Match lobbies stay on the *"Kick back"* screen until at least **2 players** are in the lobby and ready.
   * **Joining a Friend:** Select **Custom Match** or **Quick Match** to join an active LAN / ZeroTier / Tailscale session.

---

## 📂 How to Access `Android/data` on Android 13 / 14 / 15+

Modern Android versions restrict `Android/data` access in some file managers. If **ZArchiver** gives an *"Access Denied"* error when opening `Android/data/com.libertyrecomp/`, use one of these two methods:

### Method 1: Built-in Android Files App (`data1` Trick)
1. Open the **Files** app (the blue stock AOSP system file manager — *not* Google Files; you can launch it via **FV File Explorer** or **Activity Launcher** -> `com.android.documentsui`).
2. Navigate to **`Android/`** and **rename `data` to `data1`**.
3. Open **`data1/com.libertyrecomp/files/`**, paste or edit **`args.txt`**, and save it.
4. Go back to **`Android/`**. If Android automatically recreated an empty **`data`** folder while you were editing, **rename that new empty `data` folder to another number (like `data2`) and delete it**, then **rename your original `data1` folder back to `data`** before launching the game!

### Method 2: Shizuku + ZArchiver (No Rename Needed)
1. Install **Shizuku** and enable Wireless Debugging in Android Developer Options to start the Shizuku service.
2. Open **ZArchiver** -> **Settings (`⋮`)** -> **Root** -> set **Type of root access** to **Shizuku**.
3. You can now edit `Android/data/com.libertyrecomp/files/args.txt` directly inside ZArchiver.

---

## 🌍 How to Play Online with Friends via ZeroTier / Tailscale

Because `--gta4_multiplayer_backend=lan` uses local network broadcasts (`UDP port 36002`), you can play over the internet with friends anywhere in the world—including **Android-to-Android** and **Android-to-PC**—without router port forwarding:

1. **Both players install ZeroTier One** (or **Tailscale**) from the Google Play Store (or PC client) and sign in.
2. **Create a network:** One person creates a free network at `://zerotier.com` and shares the **16-character Network ID** with everyone playing.
3. **Connect & enable:** Every player pastes that Network ID into ZeroTier One, toggles it **ON**, and the host approves each device in the ZeroTier dashboard.
4. **Launch & join:** Keep ZeroTier running in the background, open **Liberty Recompiled**, pull up your phone -> **Multiplayer**, and join via **Custom Match** or **Quick Match**.

---

## ⚠️ Important Multiplayer Tips

> [!IMPORTANT]
> * **Always restart after editing `args.txt`:** `gta4_multiplayer_backend` and `gta4_player_name` are read at startup—force-close and reopen the app after saving changes.
> * **Keep the app in the foreground:** Android suspends background network sockets—minimizing the game while in a lobby will disconnect you.
> * **Keep mods for private lobbies:** Public/LAN lobbies require matching `script.img` and `common.rpf` files. Only use script mods or trainers in private lobbies with friends running the exact same files.
> * **Back up your saves:** Copy `Android/data/com.libertyrecomp/files/LibertyRecomp/saves/` to `Downloads` before updating APKs.

---

## 🛠️ Troubleshooting

| Symptom | Cause | Fix |
| :--- | :--- | :--- |
| **"Cannot connect to Xbox Live"** | `args.txt` wasn't read or `gta4_community_url` is unreachable | Make sure `args.txt` is inside `Android/data/com.libertyrecomp/files/` with `--gta4_multiplayer_backend=lan`, then force-close and restart. |
| **Stuck on *"Kick back"* in Free Mode** | Player Match lobbies require 2+ players to start | Use **Multiplayer -> Party Mode** to load straight into Liberty City solo while friends join. |
| **Still shows `"Player"` in lobby** | Edited `args.txt` while the game was open | Force-close the app completely and relaunch so `gta4_player_name` reloads. |
| **Drops out of lobby when switching apps** | Android suspends background network sockets | Keep Liberty Recompiled in the foreground while inside a multiplayer session. |
| **Instant desync or kick in lobby** | Mismatched `script.img` / `common.rpf` mod files | Use vanilla game files in public lobbies, or ensure all friends in a private lobby have identical modded files. |

> [!TIP]
> **Need to check why something failed?** Open `Android/data/com.libertyrecomp/files/LibertyRecomp/logs/` and open the newest `.log` file to inspect multiplayer connection messages.

---

## 🐛 Bug Reports & Help

Running into an issue, crash, or multiplayer connection error on your device? **Open an issue in the [Issues tab](../../issues)** and I will help you troubleshoot it!

To help me fix your problem faster, please include:
1. **Your device model & Android version** (e.g., Dimensity 8400, Android 15).
2. **Whether you are using `lan` or `community` mode** (and your `args.txt` contents).
3. **Your latest `.log` file** from `Android/data/com.libertyrecomp/files/LibertyRecomp/logs/`.

---

## 🤝 Maintenance & Contributing

> [!NOTE]
> **Maintenance Status:** Getting this working from scratch on mobile took weeks of trial and error, so **I probably won't update or maintain this fork regularly** (I might try occasionally, or I might not). If you want future upstream updates, GPU optimizations, or new ports, feel free to **fork this repository** and build your own updated APK—or ask **[prollynotzjay](https://github.com)** about merging multiplayer support upstream!

Want to improve Android compatibility, GPU performance, or multiplayer features? You are welcome to **fork this repo** or open a **Pull Request** anytime.

---

## 🙏 Credits & Upstream Projects

* **[ZMOD-Liberty](https://github.com/ZMOD-Liberty):** Base Mali / Dimensity Android fork and Vulkan shader/performance fixes.
* **[Liberty Recompiled](https://github.com):** Core static recompilation engine, `ReXGlue`, and community/LAN multiplayer architecture.
* 
