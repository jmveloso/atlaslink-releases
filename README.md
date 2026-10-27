# AtlasLink

**Self-hosted remote desktop for IT support.** A desktop client paired with a control server that your organization runs itself.

## 1. Overview

AtlasLink connects IT staff to managed computers through a self-hosted control server (device approval, session brokering, policy, audit). Sessions run peer-to-peer over WebRTC. It ships in two editions:

- **Full Client (IT edition):** for IT support staff. It can start and receive authorized remote sessions and has the full remote-support interface.
- **Agent edition:** for supported endpoints. It receives authorized remote-support sessions under server-enforced restrictions.

## 2. Latest release

**[AtlasLink 0.3.38](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.38)** — Windows, macOS (unsigned zip) and Linux (experimental).

## 3. Downloads by platform and edition

| Platform | Full Client (IT) | Agent |
|---|---|---|
| Windows 10/11 x64 | [AtlasLink-0.3.38-setup.exe](https://github.com/jmveloso/atlaslink-releases/releases/download/v0.3.38/AtlasLink-0.3.38-setup.exe) (77.7 MB) | [AtlasLink-Agent-0.3.38-setup.exe](https://github.com/jmveloso/atlaslink-releases/releases/download/v0.3.38/AtlasLink-Agent-0.3.38-setup.exe) (77.7 MB) |
| Windows x86 (32-bit) | — | [AtlasLink-Agent-0.3.38-setup-ia32.exe](https://github.com/jmveloso/atlaslink-releases/releases/download/v0.3.38/AtlasLink-Agent-0.3.38-setup-ia32.exe) (72.4 MB) |
| Linux x86_64 (experimental) | [AtlasLink-0.3.38-linux-x86_64.AppImage](https://github.com/jmveloso/atlaslink-releases/releases/download/v0.3.38/AtlasLink-0.3.38-linux-x86_64.AppImage) (80.0 MB) | [AtlasLink-Agent-0.3.38-linux-x86_64.AppImage](https://github.com/jmveloso/atlaslink-releases/releases/download/v0.3.38/AtlasLink-Agent-0.3.38-linux-x86_64.AppImage) (80.0 MB) |
| macOS x64 / arm64 | [AtlasLink-0.3.38-mac-x64.zip](https://github.com/jmveloso/atlaslink-releases/releases/download/v0.3.38/AtlasLink-0.3.38-mac-x64.zip) / [AtlasLink-0.3.38-mac-arm64.zip](https://github.com/jmveloso/atlaslink-releases/releases/download/v0.3.38/AtlasLink-0.3.38-mac-arm64.zip) | [AtlasLink-Agent-0.3.38-mac-x64.zip](https://github.com/jmveloso/atlaslink-releases/releases/download/v0.3.38/AtlasLink-Agent-0.3.38-mac-x64.zip) / [AtlasLink-Agent-0.3.38-mac-arm64.zip](https://github.com/jmveloso/atlaslink-releases/releases/download/v0.3.38/AtlasLink-Agent-0.3.38-mac-arm64.zip) |

**Full Client vs Agent**

| | Full Client (IT) | Agent |
|---|---|---|
| Start outgoing sessions | Yes | No (blocked in the app and by the server) |
| Receive authorized sessions | Yes | Yes |
| Admin remote commands | Allowed when the server policy enables them | Always refused |
| Admin quick actions (reboot, restart app) | Yes | Yes |
| Recording, printer and Wake-on-LAN settings | Shown | Hidden |
| Windows installer | `AtlasLink-X.Y.Z-setup.exe` | `AtlasLink-Agent-X.Y.Z-setup.exe` |

## 4. Features

- Remote screen, keyboard and mouse control over WebRTC, with multi-monitor switching and view modes
- File transfer, text chat, clipboard sync and one-way system audio (permission-controlled)
- Session passwords, approval prompts, view-only mode and timeouts
- Recent connections with favorites and aliases
- Admin console: device approval, live sessions, security policy, audit log, remote commands and quick actions
- VNC bridge (0.3.32): reach legacy VNC hosts (private networks by default) from the admin console
- Optional TURN relay for restrictive networks

## 5. System requirements

| Platform | Requirement |
|---|---|
| Windows | Windows 10 or 11, x64 |
| Linux (experimental) | x86_64 with an X11 session for remote control (XTest). Wayland sessions without X11 are view-only. Not yet validated on real hosts |
| macOS | Unsigned zip (x64 and arm64). First launch via right-click, then Open. Screen Recording and Accessibility permissions required. |
| Network | Outbound access to the control server's WebSocket endpoint (`ws://` or `wss://`) |

## 6. Installation

- **Windows:** run the installer for your edition. It installs per user, and you can choose the folder. Open Settings and enter your organization's control server URL. New devices stay *Pending* until an administrator approves them.
- **Linux:** `chmod +x AtlasLink-*.AppImage`, then run it. Use an X11 session if this machine will be controlled remotely.
- **macOS:** unzip, then right-click the app and choose Open. Grant Screen Recording and Accessibility when prompted.

## 7. Control server requirements

| Requirement | Details |
|---|---|
| Runtime | Node.js 18 or later |
| Database | SQLite 3 (embedded) |
| Ports | One TCP port for HTTP(S) and WebSocket (default 7080) |
| Optional | TURN relay; VNC bridge enabled under Security, VNC bridge |

The control server is deployed and managed by your organization's IT administrator. Ask them for the server URL.

## 8. Security

- New installs require device approval and session passwords by default. File transfer and remote audio are off by default.
- Session passwords use challenge-response and are never sent over the network.
- WebRTC media is encrypted with DTLS-SRTP, and data channels are encrypted.
- Server passwords are hashed with PBKDF2-SHA256 (210,000 iterations). Admin login and device registration are rate-limited.
- Administrative actions, remote commands, quick actions and VNC sessions are audited.
- Use TLS (`wss://`/`https://`) in production and restrict server access to your network or VPN.

## 9. Update / upgrade

- **Windows:** the app checks for updates about a minute after it starts and every 4 hours after that. It downloads only its own edition's installer and verifies the Ed25519 signature (`.sig`), version, size and SHA-256. It then installs silently when no session is active. It never downgrades, and it installs nothing if verification fails. Manual upgrade: run the newer installer over the existing one. Settings are kept.
- **Linux:** the app shows that an update is available with a link to this page. Replace the AppImage by hand.
- **macOS:** the app shows a download link (no silent install). Replace the app from the zip by hand.

## 10. Download verification

```powershell
Get-FileHash -Algorithm SHA256 .\AtlasLink-0.3.38-setup.exe
```
Expected output: `453AF80FDFFAB543D2D45C2E3541A8E2A10A4B85F57035CEA5E73D045D206C50`

```cmd
certutil -hashfile AtlasLink-0.3.38-setup.exe SHA256
```
Expected output: `453af80fdffab543d2d45c2e3541a8e2a10a4b85f57035cea5e73d045d206c50`

PowerShell prints uppercase and `certutil` prints lowercase. The values are the same.

| File | SHA-256 |
|---|---|
| AtlasLink-0.3.38-setup.exe | `453AF80FDFFAB543D2D45C2E3541A8E2A10A4B85F57035CEA5E73D045D206C50` |
| AtlasLink-Agent-0.3.38-setup.exe | `FD66CC6A1D0DF572FBC589DCFF87226AF2F8B74DA85D5D9727A0448C02FE3557` |
| AtlasLink-0.3.38-linux-x86_64.AppImage | `C1E5B18024439FA7EF84A05E309BDDC645C30FDD36244C3042F8395010BB253A` |
| AtlasLink-Agent-0.3.38-linux-x86_64.AppImage | `A70B74EEE0CB42679D22308EEE1A1DD35DC0782AB17FDD4EA2D91D4146434FAE` |
| AtlasLink-0.3.38-mac-x64.zip | `B4F652CCECD04B1F4B9A3F037AB281F88651F7320254D6BC66431D7BDE5A4BC1` |
| AtlasLink-0.3.38-mac-arm64.zip | `E3117A88B26B5FBD662933A233BDC2E48A038591E9E99DE3908E18419B21E3AF` |
| AtlasLink-Agent-0.3.38-mac-x64.zip | `D9FC023025E33C7FC8E4EBC72452BDFD5BAD314A6AA3407E77DE6457881088CB` |
| AtlasLink-Agent-0.3.38-mac-arm64.zip | `2D3CC305026BBC646B543BED84F81D8E5550D2B0041355B2D14C4517A8370FEF` |

## 11. Known limitations

- Linux is experimental: built in CI but not yet validated on real hosts. Remote control needs X11 (Wayland without X11 is view-only). Updates are manual.
- macOS builds are unsigned (first launch via right-click, then Open) and need Screen Recording and Accessibility permissions.
- Both editions share the same settings folder if installed for the same Windows user.

## 📜 Version History

### 0.3.39 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.39)**

**Installer:** `AtlasLink-0.3.39-setup.exe` (not published)

**SHA-256:** *(see release page)*

---

### 0.3.38 — 2026-10-27

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.38)**

**Installer:** `AtlasLink-0.3.38-setup.exe` (77.7 MB)

**Agent:** `AtlasLink-Agent-0.3.38-setup.exe` (77.7 MB)

**SHA-256:** `453AF80FDFFAB543D2D45C2E3541A8E2A10A4B85F57035CEA5E73D045D206C50`

**Agent SHA-256:** `FD66CC6A1D0DF572FBC589DCFF87226AF2F8B74DA85D5D9727A0448C02FE3557`

---

### 0.3.37 — 2026-10-27

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.37)**

**Installer:** `AtlasLink-0.3.37-setup.exe` (77.7 MB)

**Agent:** `AtlasLink-Agent-0.3.37-setup.exe` (77.7 MB)

**SHA-256:** `84E09AEB2DF628FFF1F63059B8817EDC6D9FA13E2CFA278ED353CA0D40825D9A`

**Agent SHA-256:** `FC1B578614BCE6296268375F7226AE10EA3B868B1A1619856C88A70679C24FA2`

---

### 0.3.36 — 2026-10-27

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.36)**

**Installer:** `AtlasLink-0.3.36-setup.exe` (77.7 MB)

**Agent:** `AtlasLink-Agent-0.3.36-setup.exe` (77.7 MB)

**SHA-256:** `106AA4B3B5B174A21B8C2FA0D7AAB6F05AB330DDE68B047D646CC69998C296FA`

**Agent SHA-256:** `B2C0633629A2FBBA7315793B3FCC4F485B61BD4D4738D90004777BA26CC43F59`

---

### 0.3.35 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.35)**

**Installer:** `AtlasLink-0.3.35-setup.exe` (77.7 MB)

**Agent:** `AtlasLink-Agent-0.3.35-setup.exe` (77.7 MB)

**SHA-256:** `FC1B7276D26CA2BD8981D180C4A565CFCD8B62E9886DEC24E4256F0A62DD88FF`

**Agent SHA-256:** `425DD25EEE6849296D29EF76DA07F448286ECB2B9DB88143682DFDB6D0F0998E`

---

### 0.3.34 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.34)**

**Installer:** `AtlasLink-0.3.34-setup.exe` (77.7 MB)

**Agent:** `AtlasLink-Agent-0.3.34-setup.exe` (77.7 MB)

**SHA-256:** `7302C833376CF710F50B166988BACA921D7B406DA2C534D406B820FC4CB8ADA3`

**Agent SHA-256:** `2A58D9F81DBE871EDB7CF6225225FEF5B6D81F49AFF9F252E6A993FD22DB1B1A`

---

### 0.3.33 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.33)**

**Installer:** `AtlasLink-0.3.33-setup.exe` (77.7 MB)

**Agent:** `AtlasLink-Agent-0.3.33-setup.exe` (77.7 MB)

**SHA-256:** `9A3F152D961DDB89D8E1306C3F70434908838E607BB078C476C0DEFB64AE057A`

**Agent SHA-256:** `0FC8F4CD1B824741DD58A607B181BE9D00E8CF456BF2C7375D9EC998E6AA70AC`

---

### 0.3.32 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.32)**

**Installer:** `AtlasLink-0.3.32-setup.exe` (77.7 MB)

**Agent:** `AtlasLink-Agent-0.3.32-setup.exe` (77.7 MB)

- **Remote quick actions** — reboot a PC or restart the AtlasLink app straight from the Devices tab. Confirmed, rate-limited and audited.
- **macOS + Linux builds** — the full client and Agent now ship for all three desktop platforms, with native input injection (CGEvent / XTest) and documented permission boundaries.
- **VNC bridge** — reach legacy machines (XP/7-era boxes running TightVNC/UltraVNC) from the admin console's new **legacy vnc** page. Enable it under Security → VNC bridge; targets are restricted to private networks and every session is audited.
- **Update status on macOS/Linux** — shows a Download link instead of attempting a silent install.

**SHA-256:** `A89F3B619F8987FD7D2BFF2E69AC1A72E3D045285E67BBC1421FDFB47324618F`

**Agent SHA-256:** `56517ECC163998C647185CD4A6E403AB367D80230D052C2D64E91005F4FF9C4D`

---

### 0.3.31 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.31)**

**Installer:** `AtlasLink-0.3.31-setup.exe` (77.7 MB)

**Agent:** `AtlasLink-Agent-0.3.31-setup.exe` (77.7 MB)

A sharper picture on any link — plus real fleet tooling.

- New **"Clarity (slow link)"** mode in the session toolbar: keeps the remote display at native resolution and sheds only bitrate and framerate — text stays legible where other modes go blurry
- Display capture no longer caps at 1440p — 4K and ultrawide hosts now arrive at native resolution, and Chromium's capture path stops trading fidelity for latency
- Video gets priority over file transfers and audio on the same connection, and severe congestion (≥10% loss or ≥500ms RTT) now steps quality down instantly instead of after ~4 seconds of mush
- Admins can run a shell command on any online full-edition device straight from the Devices tab (**Cmd** button) — output returns in a modal with exit code, copy, and truncation notice
- Guarded at every layer: admin auth, the new `remoteCommands` security policy toggle, and Agent-class devices refused both server-side and inside the Agent build itself
- Every run is audited with the command text and exit code

**SHA-256:** `7CAAA7DB9E086E937D5F5637DA35BA9C536B5DB66700FD2C9BE36DB1A411FF48`

**Agent SHA-256:** `4B23D518B5E8EFE74C1AC00AF687BF83DA6FE9BF89C5A54B3EC869FFDE478D48`

---

### 0.3.30 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.30)**

**Installer:** `AtlasLink-0.3.30-setup.exe` (77.7 MB)

**Agent:** `AtlasLink-Agent-0.3.30-setup.exe` (77.7 MB)

A picture-quality pass: fewer pixelated moments on constrained links, and better visibility into *why* a session looks the way it does.

- `saver`, `low` and `balanced` sessions now negotiate **VP9** — its screen-content coding keeps text and UI edges legible at bitrates where H.264 turned to blocks
- `sharp` stays H.264-first: at 9 Mbps/60 fps there's headroom and hardware encode keeps the host's CPU free
- **Est. capacity** — the link's stable bandwidth estimate (`~3.4 Mbps`), WebRTC's own converged estimator rather than instantaneous throughput; the host relays its uplink estimate so the tech sees the remote end's headroom
- **Adaptive** — the viewer now sees the host's live adaptive rung (`max`, `-2/4`…) instead of a permanent dash, updated the moment the ladder moves

**SHA-256:** `F063022127B56240150385A6EC169DC523C5E1792044721AA3E010D457E45941`

**Agent SHA-256:** `669120AD4C54CDF9B6BC20D0FAA6CC9EAF5BCAB7C2C39CB143DA03668FC03899`

---

### 0.3.29 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.29)**

**Installer:** `AtlasLink-0.3.29-setup.exe` (77.7 MB)

**Agent:** `AtlasLink-Agent-0.3.29-setup.exe` (77.7 MB)

Fleet management at scale and a proper support surface for the Agent edition.

- Assign every device to a **group** (site, department, customer) from the Devices tab — a new `Set group…` control, per-device group tags, and `Group:` entries in the toolbar filter
- Row checkboxes with a select-all header bring a **bulk action bar**: Approve, Unblock, Block, Kick, Set group, and Remove apply to every selected device in one call — including bulk group assignment or clearing
- Group names join the device search; selections survive the live refresh; bulk operations are audited (`device-bulk` / `device-group`)
- The Agent home now leads with a hero **Your support ID** card — the 9-digit device ID grouped `123 456 789`-style at headline size, with "Read this code to your technician" and a Copy button
- The permanent device ID is the support credential — no ephemeral codes, nothing new for the end user to understand

**SHA-256:** `9BBF1953F161058853B6D94CED5B2FF5E8498558DED52D0C8ACA527F88BDE083`

**Agent SHA-256:** `5C598E5D39BEC108C7EE7656CC597AD276E6BA37EDCE0942847B43B3359A4AA2`

---

### 0.3.28 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.28)**

**Installer:** `AtlasLink-0.3.28-setup.exe` (77.7 MB)

**Agent:** `AtlasLink-Agent-0.3.28-setup.exe` (77.7 MB)

Three hardening improvements that make the two-edition split genuinely enterprise-ready.

- Each device can now be **pinned** to `agent` or `full` from the admin console (Devices tab → `class:` picker). A pin is admin-controlled state the client cannot change by re-registering: a tampered Agent build claiming `full` still gets refused on `connect` and wake requests
- The device row shows a `pinned` badge, and a pinned device reporting a different edition writes an `edition-mismatch` event to the audit log
- Until you pin, the class the client reports at registration is used (`class: auto`)
- When your server policy has **Require user sign-in** on, the full client now opens to a *"Sign in required"* screen instead of the workspace — device ID, connect box, and recents stay hidden until a viewer account signs in
- Signing out returns to the gate; an in-progress session is never masked (you can always disconnect). The Agent edition is unaffected
- Drop a code-signing certificate into repo secrets (`CSC_LINK` + `CSC_KEY_PASSWORD`) and release builds are Authenticode-signed automatically — SmartScreen warnings gone for both editions

**SHA-256:** `C941B1F3F559D5E910B2A7C890EF70CC8DF3365C6F1640002441714E37912849`

**Agent SHA-256:** `FD175508782BDD34A5FBB3E2B71DBA6D65838D6D8692BB987222B2F6B1AA8A49`

---

### 0.3.27 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.27)**

**Installer:** `AtlasLink-0.3.27-setup.exe` (77.7 MB)

**Agent:** `AtlasLink-Agent-0.3.27-setup.exe` (77.7 MB)

A correctness release for the new Agent edition — the "cannot connect out" promise is now **enforced by the server**, not just hidden in the UI.

- The control server now refuses `connect` and wake requests from devices that registered as the Agent edition — even under open-access policies or an explicit wildcard permission, and even if the device holds a valid user sign-in
- Denied attempts are audited (`connect-denied` / `wol-denied` with `reason: 'edition'`) so tampering attempts show up in the audit log
- Agents can still *receive* sessions and still relay Wake-on-LAN packets for other devices on their LAN — only initiating is blocked
- The Agent build's outbound-session path is additionally gated by a compile-time constant — it cannot be re-enabled by changing app state
- Agent window and tray now read "AtlasLink Agent"; Settings hides the user sign-in block it doesn't need
- `npm run dev:agent` runs the Agent edition in dev mode

**SHA-256:** `0094412DAB9E139F3E98CFD4E2DD4C16C9AFAE2A656108CE97149A83909D1DCC`

**Agent SHA-256:** `CDF1C85A119EEB07CAA5FDBDD88403C1CF094C8C77A42CE197CA75841D7AE152`

---

### 0.3.26 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.26)**

**Installer:** `AtlasLink-0.3.26-setup.exe` (77.7 MB)

**Agent:** `AtlasLink-Agent-0.3.26-setup.exe` (77.7 MB)

AtlasLink now ships **two editions**: the full client for IT, and **AtlasLink Agent** — a deliberately limited build for the people being helped.

- Clean "Ready for support" screen: device ID, copy button, unattended-access and sharing toggles — nothing else
- **Cannot connect to other devices**: no connect box, no history, no sign-in — the server also grants agents no connect permissions, so it's enforced, not just hidden
- During a support session it still shows who's connected, plus chat and file panes
- Installs side-by-side with the full client (own identity, own update channel — it only ever receives Agent updates)
- Admins see a green **Agent** badge on these devices in the console

**SHA-256:** `7A7A67D7D39455DC0341E7F54079F3A661936993F35F1325391A1A34C287EF6B`

**Agent SHA-256:** `85CB7DD6AB5D8559AA1945834BBDF945B353F634046F2A8B89C60DF03FDB319E`

---

### 0.3.25 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.25)**

**Installer:** `AtlasLink-0.3.25-setup.exe` (77.7 MB)

A small admin-console update: **device MAC addresses are now visible** in the Devices list.

- Each device row shows its registered MAC (e.g. `mac 7c:8a:e1:2f:9b:04`) under the subnet line in the location cell
- The WoL tag's tooltip now shows the MAC it would target
- No client changes needed — the MAC was already collected at registration since 0.3.21; this release only surfaces it. Older devices without a NIC report simply show no line

**SHA-256:** `911B97FC125595B458E817FC6216361B5F8C4BFA5781DCF7159E5F2B3C3763FC`

---

### 0.3.24 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.24)**

**Installer:** `AtlasLink-0.3.24-setup.exe` (77.7 MB)

AtlasLink 0.3.24 keeps session chat reachable when the app isn't: minimize AtlasLink mid-session and an incoming chat message now pops up a compact, standalone chat window.

- Incoming messages open a small chat window at the bottom-right of your screen and flash the taskbar button — while the main window is minimized or in the tray
- Appears **without stealing focus**, so it never interrupts what you're typing elsewhere
- Shows the peer's name and ID, the full session history so far, and an **Open app** button that restores the main window
- Reply right from the popup — messages go through the same session channel as normal chat
- Close the popup once and it stays closed for the rest of that session (messages still flash the taskbar)
- Restore or focus the main window and it re-arms for next time

**SHA-256:** `5494324DE86E0C8247A39E6983BC594C135EB5236E6EC0669E195264F915E8CB`

---

### 0.3.23 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.23)**

**Installer:** `AtlasLink-0.3.23-setup.exe` (77.7 MB)

AtlasLink 0.3.23 makes the admin console a real IT control surface: check a remote PC's health without starting a session, and see Wake-on-LAN readiness at a glance.

- Devices → **Health** on any online machine shows live **CPU, memory and disk** meters plus uptime, hostname, OS, and app version
- Works without a remote session — the agent answers over its secure control channel; every lookup is audited
- Device rows now show a **WoL tag** and the LAN subnet, so you can tell which PCs are wakeable and which can relay for each other
- New policy switch to turn Wake-on-LAN off fleet-wide (Security policy → Access)
- Fixed a security gap: "Require user sign-in" now also applies to wake requests
- **Audit log paginates** — 50 events per page, with a "Load older events" button for deep history

**SHA-256:** `26828C55BC04AC420A3427841EFE18D2427CA14E937B0FEA5227EB7A8C5FEB01`

---

### 0.3.22 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.22 adds session recording and remote printing — the last two planned features are now real.

- A **record button** in the session toolbar saves what you see (and hear, when remote audio is on) as a video file
- Files land in `Downloads\AtlasLink\recordings` and a red timer in the toolbar shows the recording is live
- Recording stops and saves automatically if the session ends or drops — a crash loses at most a second
- After you accept a file, printable formats — **PDF, images, plain text** — get a **print button**
- Choose any printer installed on your PC, or pick through the Windows print dialog
- Office documents (.docx, .xlsx) can't be rendered for printing — open them and print normally instead

**SHA-256:** `A8FD90C3ECD7A7107CB323E0EA48E4F20F7FC384EDFCFAF36D01F158FA8B6E89`

---

### 0.3.21 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.21)**

**Installer:** `AtlasLink-0.3.21-setup.exe` (77.7 MB)

AtlasLink 0.3.21 adds Wake-on-LAN — a powered-off or sleeping PC can be woken remotely and connected to, straight from the app.

- **Wake button on offline devices** — connecting to a PC that is offline now offers to wake it. One click sends the wake packet; AtlasLink retries the connection automatically while it boots.
- **Smart delivery** — the wake packet is broadcast by the server when it is on the device's network, or relayed securely through another online AtlasLink device on that network. You never have to think about which path it took.
- **Remembers the PC's network adapter** — each device reports its wired adapter address when it registers, so wake still works even if it last connected over Wi-Fi.
- **Same permissions as connecting** — you can only wake devices you are allowed to connect to, and wake requests are rate-limited and logged on the server.

**SHA-256:** `2B4FDE1D32B83EB17EFA5C07C50C677D07103F6BD83CAD27F80EF4ABD7F61759`

---

### 0.3.20 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.20)**

**Installer:** `AtlasLink-0.3.20-setup.exe` (77.7 MB)

AtlasLink 0.3.20 makes file transfer faster and easier, and turns clipboard sharing into real automatic sync.

- **Drag & drop** — drop files anywhere on the session window to send them; a drop-zone overlay confirms it
- **Faster transfers** — bigger read batches and two parallel streams per direction; large files move noticeably quicker
- **Bigger files** — per-file limit raised from 100 MB to 1 GB
- **Accept all** — one click approves every pending incoming file when several arrive at once
- **ETA** — transfer cards now show estimated time remaining next to speed and percent
- **Automatic sync** — copy on one side and it is simply there on the other; no more push/pull buttons. Honors the per-session clipboard permission — turning it off stops syncing instantly

**SHA-256:** `8610D53F062006AC49AC9771E59F63C1FB27D6DF61ACB1B3DC449A3EF0748596`

---

### 0.3.19 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.19)**

**Installer:** `AtlasLink-0.3.19-setup.exe` (77.7 MB)

AtlasLink 0.3.19 fixes the picture flicker seen when viewing a remote PC whose screen is mostly still.

- **No more black flashes on idle screens** — when the remote screen barely changes, the video could blink black every few seconds. The stream now holds the last frame smoothly instead. Most noticeable on quiet desktops like a home PC sitting at the desktop

**SHA-256:** `FF11D802EA587428D4029C91852D581BFCF03A925C69E517C0E59D4418621231`

---

### 0.3.18 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.18)**

**Installer:** `AtlasLink-0.3.18-setup.exe` (77.7 MB)

AtlasLink 0.3.18 makes streaming smoother on fast and slow connections alike, and tidies up the main window.

- **Higher frame rates when you want them** — the Sharp quality setting now actually captures up to 60 fps (it was silently capped at 30). Raising the quality mid-session updates the capture right away
- **New "Slow link" quality preset** — tuned for connections under ~2 Mbit/s: steady 12 fps video that stays smooth instead of stuttering, and it automatically degrades further if the link gets worse
- **Performance stats in the toolbar** — a small live readout next to the quality selector shows fps, resolution, codec, bitrate and ping — so the old floating box no longer covers part of the remote screen. Click it for the full diagnostics panel. On smaller windows it shrinks to just fps and ping
- **Main window fix** — on some displays the workspace sat off-centre and the "Connect to a device" card ran past the right edge. The two cards now stay centred and fully inside the window, and they stack neatly at minimum window size or high display scaling (125–150%)

**SHA-256:** `EC36ACAFAAE5B6F43602E7BB9CDA2DB6FFD94544172C0148AA058348AB2FCF18`

---

### 0.3.17 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.17)**

**Installer:** `AtlasLink-0.3.17-setup.exe` (77.7 MB)

AtlasLink 0.3.17 moves the session toolbar out of the way of the remote desktop and makes relayed sessions steadier.

- **Toolbar no longer covers the remote screen** — in a window, the session controls sit in their own bar above the picture, so the remote browser's tabs and address bar stay reachable. Device name on the left, controls in the middle, Disconnect on the right
- **Fullscreen toolbar** — hidden until you point at the small handle at the top of the screen; it stays open while you use it and hides again afterwards
- **Screen buttons** — pick the remote monitor with "Screen 1", "Screen 2", … (hover for the monitor's name and resolution)
- **Small windows** — less-used controls move into a "More" menu
- **Steadier relayed sessions** — the default bitrate limit for sessions going through the relay is now 1.5 Mbit/s (fewer picture stalls on Wi-Fi), and changing the limit in the admin console now applies to sessions that are already running

**SHA-256:** `40584F01D4589908C7B807013EC581046D6AD9D029086C971BFDF4B74572B0DE`

---

### 0.3.16 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.16)**

**Installer:** `AtlasLink-0.3.16-setup.exe` (77.7 MB)

AtlasLink 0.3.16 fixes a flickering picture when the remote screen is idle, and makes reconnecting after a remote restart faster.

- **Fixed: picture flickering on an idle remote screen** — when nothing moved on the remote PC, the app briefly treated the screen as "off or locked" every few seconds and flashed a notice over the picture (and showed a toast on the remote PC). Only a screen that stays blank now counts
- **Faster reconnect after a remote restart** — a reconnect attempt that stalls while the PC is still starting is retried after 15 seconds instead of waiting about a minute
- **Clearer restart notice** — no more "someone may need to sign in" warning for PCs that do sign in by themselves; you are warned only when AtlasLink does not start with Windows on that PC
- **Steadier video on relayed sessions** — the relay bitrate limit no longer lifts for a moment when a connection statistic is briefly missing

**SHA-256:** `7A771B9928DC937AE06EE286B36CA3E448B2B567B1D9D95D262CD1CF3F68FEFC`

---

### 0.3.15 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.15)**

**Installer:** `AtlasLink-0.3.15-setup.exe` (77.7 MB)

AtlasLink 0.3.15 adds remote restart: restart the PC you are connected to, and AtlasLink reconnects when it is back.

- **Restart remote PC** — a new button in the session toolbar. Windows on the other PC restarts after a 15-second warning
- **Automatic reconnect** — the viewer waits for the PC to come back and reconnects by itself (up to 6 minutes), using the remembered session password
- **Off by default** — a PC can only be restarted if *Allow remote restart* is turned on in its own Settings → Security
- **Audited** — every remote restart is recorded in the admin console's audit log

**SHA-256:** `26F575CCB35C53C973004B1B2BE912C1788ACE15BA60BDD6F18DB639DE326DC9`

---

### 0.3.14 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.14)**

**Installer:** `AtlasLink-0.3.14-setup.exe` (77.7 MB)

AtlasLink 0.3.14 adds an enrolment token, so only PCs you set up can join your server. PCs that are already registered keep working without any change.

- **Enrolment token** — generate it in the admin console (Server → Enrolment token). New PCs need it once to register; existing PCs are never asked for it
- **Easy setup on new PCs** — enter the token in Settings → Connection, or start the app with `AtlasLink.exe --enroll-token=<token>` (useful for deploying to many PCs)
- **Clear message** — a new PC without the token now says what is needed instead of silently failing to connect

**SHA-256:** `4E52300C0D325993D76A110E583FCE66AF7D76AF196672D2C42DCE02FD5874E6`

---

### 0.3.13 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.13)**

**Installer:** `AtlasLink-0.3.13-setup.exe` (77.7 MB)

AtlasLink 0.3.13 makes the TURN relay more dependable and keeps relayed sessions from overloading the relay server.

- **Relayed sessions use less server bandwidth** — while a session goes through the TURN relay, video is capped (2.5 Mbit/s by default, adjustable in the admin console). Direct sessions keep full quality
- **Fixed: sessions that could not use the relay** — a session started right after the app opened, or on a PC that accepts connections by password, could begin without relay credentials and then fail on networks that need the relay
- **Relay reachable from strict networks** — the server can now also offer the relay on TCP port 443 (see deploy/TURN.md); the admin console's Test relay checks TCP addresses too

**SHA-256:** `E21DFDF7672F7609724F087D35139A225909A6EC4B58C3522E6BCCD775F001E3`

---

### 0.3.12 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.12)**

**Installer:** `AtlasLink-0.3.12-setup.exe` (77.7 MB)

AtlasLink 0.3.12 updates itself. Install it once; from then on new releases download and install automatically.

- **Automatic updates** — the app checks for a new release shortly after start-up and every few hours, downloads it in the background, and installs it only when no session is active. It then restarts itself on the new version. Settings, device ID and remembered passwords are kept
- **Signed updates only** — every release is signed. The app installs an update only if its signature, size and checksum all match, and never installs an older version
- **Pause updates from the admin console** — Policy → *Pause automatic updates* holds all PCs on their current version; connected PCs pick up the change immediately
- **Instant policy changes** — policy edits in the admin console now reach connected PCs straight away instead of at their next reconnect

**SHA-256:** `EF8E7043B353A1D6EEE24B50CB17803FC5C5E2F89AD62EA9655ECAA12BF4E4DE`

---

### 0.3.11 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.11)**

**Installer:** `AtlasLink-0.3.11-setup.exe` (77.7 MB)

AtlasLink 0.3.11 keeps admin console sessions across server restarts. Redeploying or restarting the control server no longer signs administrators out.

- **Admin sessions persist across restarts** — Bearer tokens are stored in the database instead of process memory
- **Revocation still works** — Logout, password changes, and admin account removal immediately invalidate tokens
- **No behavior change** — Sessions still expire and refresh the same way; they just survive server restarts now

**SHA-256:** `BA3DA5751967DF2E8B6C86419749F5D8C1BA694048BF924A93D1EC0CED5FACD9`

---

### 0.3.10 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.10)**

**Installer:** `AtlasLink-0.3.10-setup.exe` (77.7 MB)

AtlasLink 0.3.10 fixes the most common cause of sessions being interrupted. Short drops of the connection to the control server — even a few seconds — ended the session about 30 seconds later, even though everything had already reconnected.

- **Fixed: sessions ending 30 seconds after any brief connection drop.** The session now continues once the connection is back
- **Fixed: sessions ending even though the outage was shorter than 30 seconds.** While a session is reconnecting, the app now retries the server every 1–2 seconds instead of backing off for up to 30 seconds
- **Fixed: PCs stuck as "busy" after a longer outage.** A session that ended while the server was unreachable is now closed on the server once the PC reconnects, so the PC accepts new connections straight away

**SHA-256:** `5ABE07964BFBFF2EEE6B2CEB30C2A1F9160F6D030572B8E29EC4B4E4CFB5D48A`

---

### 0.3.9 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.9)**

**Installer:** `AtlasLink-0.3.9-setup.exe` (77.7 MB)

- **Server tab** — View server version, uptime, database size, configuration summary, and security warnings with actionable fixes
- **TURN diagnostics** — Test relay tool performs a real allocation attempt to distinguish secret mismatches from unreachable servers
- **Connection health insights** — See which PCs drop connections most often or need the relay, ranked over the last 7 days
- **Outdated app detection** — Devices below the current server version are tagged and filterable
- **Direct/Relay session badges** — Sessions now show whether they used a direct peer connection or relayed through TURN
- **Better error responses** — Wrong-password replies are 403 instead of 401, preventing unintended sign-outs from the admin console

**SHA-256:** `B46FF9677A42F0903486BC95BBB462AE6F1DE6A50EBDC20B55DC5ACFF1641FF1`

---

### 0.3.8 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.8)**

**Installer:** `AtlasLink-0.3.8-setup.exe` (77.7 MB)

- **Fixed: TURN relay never used after app starts** — Relay credentials sent at sign-in were lost before the app was ready, so relays only worked after the first reconnect
- **Fixed: TURN relay stops working after an hour** — Credentials expired but were never renewed, breaking relayed sessions silently

**SHA-256:** `45479D677D6C8E9B4CA85B4059454826561E95E5B2F6187A4C7F87F9695551A0`

---

### 0.3.7 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.7)**

**Installer:** `AtlasLink-0.3.7-setup.exe` (77.7 MB)

- **Interruption reports in audit log** — New **Session interrupted** event type records what dropped (signaling / peer / ICE), duration, and outcome
- **24-hour interruption metrics** — Admin overview shows interruption counts split by outcome (recovered / ended)
- **Automatic reporting** — Desktop app sends reports when reconnections finish, queued if the control server itself was down

**SHA-256:** `88E71AEA4E6FB6F314F88E1F52A45078B0019D3E8F0449D6176019CEA3D3CBB2`

---

### 0.3.6 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.6)**

**Installer:** `AtlasLink-0.3.6-setup.exe` (77.7 MB)

- **Fixed: Session ending with "Could not start screen capture" error** — Duplicate password proofs no longer crash the session
- **Fixed: Remote PC stuck "busy" after failed connection** — The host PC now recovers immediately instead of being unavailable for ~60 seconds

**SHA-256:** `69C3EB71E83486B075FB4A12119FAC1BAFED1A413FCEAE9CD82BDF1A4C1CFCD6`

---

### 0.3.5 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.5)**

**Installer:** `AtlasLink-0.3.5-setup.exe` (77.7 MB)

- **Smoother video on lossy networks** — Lost packets are retransmitted instead of forcing keyframes that freeze the picture
- **Faster quality ramp-up** — Sessions start at 2 Mbps (up from ~0.3 Mbps) so the picture is sharp within seconds
- **Sharper text under congestion** — Screen content maintains resolution when bandwidth drops instead of becoming blurry
- **Reliable session cancel** — Outgoing connections can be cancelled immediately, and stale incoming prompts are cleaned up properly
- **Redesigned audit log** — Summary cards with 24-hour counts act as filters, and events expand to show full details

**SHA-256:** `42216393F06E0886A26080B310844E881FA9838981061A84FB5D5B173E281035`

---

### 0.3.4 — 2026-10-24

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.4)**

**Installer:** `AtlasLink-0.3.4-setup.exe` (77.7 MB)

- **Hardened admin console sign-in** — Optional TOTP two-factor authentication, cookie-based sessions with token revocation, account-level login throttling
- **Safer defaults for new servers** — Open access between devices is now off by default
- **TURN relay configuration** — Manual TURN/STUN servers in Settings now take effect
- **New default control-server port** — 7080 (previously 8080)

**SHA-256:** `8F9951109783B55F7B2CFDD8433CE15EF8C2E3A63E085F003A9524310F985D7D`

---

### 0.3.3 — 2026-10-23

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.3)**

**Installer:** `AtlasLink-0.3.3-setup.exe` (77.7 MB)

- **Redesigned session viewer** — Floating controls with auto-hiding toolbar, side drawers for chat/files/info
- **Bidirectional file transfer** — Transfer files over encrypted WebRTC data channel (permission default-off)
- **Multi-monitor switching** — Switch between displays without reconnecting; supports Fit/Actual/Stretch view modes
- **Remote system audio** — One-way system audio playback from host to viewer (opt-in permission)
- **Server hardening** — Dev/live database isolation, atomic ownership claiming, unapproved devices blocked

**SHA-256:** `0AC480E7ACA0375CD040BBB02DA9F1DD041E6AB9F081EFBB4CCA02039E94DD70`

---

### 0.2.2 — 2026-10-23

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.2.2)**

**Installer:** `AtlasLink-0.2.2-setup.exe` (78.2 MB)

- **Session Manager** — Manage active sessions with permission profiles and live controls
- **In-session text chat** — Real-time messaging over encrypted WebRTC data channel
- **Recent Connections** — Connection history with favorites, custom aliases, and quick reconnect
- **Remembered passwords** — Optional password storage using Windows DPAPI encryption
- **Automatic cleanup** — Orphaned sessions are cleaned up on server restart

**SHA-256:** `F5DE7549F87F263A37ABCED17FA573DECE58034749CB942946C36292D3B5CAAE`
