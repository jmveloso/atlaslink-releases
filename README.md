# AtlasLink

**Self-hosted remote desktop for Windows** — complete control, zero cloud dependencies.

AtlasLink is a self-hosted remote desktop solution offering full remote control capabilities through a Windows desktop application paired with a self-hosted control server, giving your organization complete ownership and control of your remote access infrastructure.

---

## 🆕 What's New in 0.3.22

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.21 adds Wake-on-LAN — a powered-off or sleeping PC can be woken remotely and connected to, straight from the app.

---

## 📥 Download

**Latest Release: [AtlasLink 0.3.22](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)** (77.7 MB)

🔗 **[Download AtlasLink-0.3.22-setup.exe](https://github.com/jmveloso/atlaslink-releases/releases/download/v0.3.22/AtlasLink-0.3.22-setup.exe)**

**SHA-256:** `A8FD90C3ECD7A7107CB323E0EA48E4F20F7FC384EDFCFAF36D01F158FA8B6E89`

---

View all releases on the [**Releases page**](https://github.com/jmveloso/atlaslink-releases/releases).

---

## ✨ Key Features

### Desktop Application

- **Full remote control** — Complete screen, keyboard, and mouse control over WebRTC
- **Redesigned session viewer** — Floating controls with auto-hiding toolbar, side drawers (chat/files/info), and real-time connection diagnostics
- **Bidirectional file transfer** — Transfer files between host and viewer over encrypted WebRTC data channel (permission-controlled)
- **In-session text chat** — Real-time messaging over the encrypted WebRTC data channel
- **Remote system audio** — One-way system audio playback from host to viewer (opt-in permission)
- **Clipboard integration** — Push/pull clipboard text between host and viewer
- **Multi-monitor support** — Switch between displays during sessions without reconnecting; select Fit/Actual/Stretch view modes
- **Session Manager** — Manage active sessions with permission profiles and live controls
- **Recent Connections** — Connection history with favorites, custom aliases, and quick reconnect
- **Remembered passwords** — Optional password storage using Windows DPAPI encryption
- **Session security** — Per-session passwords, approval prompts, view-only mode, and automatic timeouts
- **Modern UI** — Dark/light theme support with native Windows styling

### Control Server & Admin Dashboard

- **Self-hosted architecture** — Your infrastructure, your control; no per-seat licensing or cloud dependencies
- **Admin dashboard** — Browser-based management console for device inventory, live session monitoring, and security policy configuration
- **Enterprise security** — Device enrollment approval, session passwords, permission-based access control, and comprehensive audit trails
- **Permission system** — Granular controls for file transfer, remote audio, and connection authorization
- **Session reliability** — Automatic orphaned session cleanup on server restart, atomic database ownership
- **Server hardening** — Dev/live database isolation, registration rate limiting, unapproved device blocking
- **TURN relay support** — Optional NAT traversal for devices behind restrictive firewalls

---

## 💻 System Requirements

### Desktop Application

| Requirement | Details |
|------------|---------|
| **Operating System** | Windows 10 or Windows 11 |
| **Network** | Access to your control server's WebSocket endpoint (`ws://host:port` or `wss://host:port`) |
| **Firewall** | Outbound WebSocket connections to the control server |

### Control Server

| Requirement | Details |
|------------|---------|
| **Runtime** | Node.js 18 or later |
| **Database** | SQLite 3 (embedded) |
| **Network Ports** | One TCP port for HTTP/HTTPS and WebSocket (default: 7080) |
| **Optional** | TURN relay (only needed for symmetric NAT or restrictive firewalls) |

---

## 🚀 Quick Start

### Desktop Application

1. **Download** the installer from the [Releases page](https://github.com/jmveloso/atlaslink-releases/releases/latest)
2. **Run** the installer and follow the setup wizard
3. **Launch** AtlasLink from the Start Menu or Desktop shortcut
4. **Configure** the Signaling server URL in Settings:
   - Point to your organization's control server (e.g., `ws://office-server:7080`)
5. **Wait for approval** — Your device will appear in the admin dashboard as "Pending"
6. **Connect** once approved by your IT administrator

### Control Server Setup

The control server is deployed and managed by your organization's IT administrator. It provides:

- Device registration and approval workflows
- Session brokering and WebRTC signaling
- Web-based admin dashboard for management
- Policy configuration and audit logging

**Note:** The control server must be self-hosted by your organization's IT department. Contact your IT administrator for your organization's Signaling server URL.

---

## 🔐 Verifying Downloads

Verify the integrity of the installer by checking its SHA-256 checksum against the official checksum published in the release notes.

### Windows PowerShell

```powershell
Get-FileHash -Algorithm SHA256 .\AtlasLink-0.3.22-setup.exe
```

Expected output: `a8fd90c3ecd7a7107cb323e0ea48e4f20f7fc384edfcfaf36d01f158fa8b6e89`

### Windows Command Prompt

```cmd
certutil -hashfile AtlasLink-0.3.22-setup.exe SHA256
```

Expected output: `a8fd90c3ecd7a7107cb323e0ea48e4f20f7fc384edfcfaf36d01f158fa8b6e89`

---

## ⬆️ Upgrading

To upgrade AtlasLink:

1. Download the latest installer from the [Releases page](https://github.com/jmveloso/atlaslink-releases/releases/latest)
2. Run the new installer
3. It will automatically upgrade your existing installation while preserving settings

**Note:** AtlasLink does not include automatic update functionality. Check the Releases page periodically for new versions.

---

## 🔒 Security Notes

- **Default policy:** New installations require device approval and session passwords by default; file transfer and remote audio permissions are disabled by default
- **Password storage (server):** All server-side passwords (admin and user accounts) are hashed with PBKDF2 (210,000 iterations, SHA-256)
- **Remembered passwords (desktop):** Optional remembered session passwords are encrypted using Windows DPAPI (Data Protection API) and stored locally
- **Session passwords:** Use challenge-response protocol (no passwords transmitted over the network)
- **WebRTC encryption:** Peer-to-peer connections use DTLS-SRTP encryption for screen content, input, and audio; text chat and file transfer use encrypted data channels
- **Permission system:** Granular controls for file transfer and remote audio capabilities; unapproved devices cannot initiate connections
- **Admin API:** Bearer token authentication with configurable expiration; rate-limited to 5 failed login attempts per 15-minute window per IP
- **Registration rate limiting:** Device registration attempts are rate-limited to 12 per minute per IP address
- **Audit trail:** All administrative actions are logged with timestamps and attribution
- **TLS recommended:** Use TLS certificates in production to encrypt signaling traffic

**Firewall Note:** Restrict control server access to your office network. Use external firewall rules or VPN for production deployments.

---

## 📜 Version History

### 0.3.21 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.21 adds Wake-on-LAN — a powered-off or sleeping PC can be woken remotely and connected to, straight from the app.

**SHA-256:** `2B4FDE1D32B83EB17EFA5C07C50C677D07103F6BD83CAD27F80EF4ABD7F61759`

---

### 0.3.20 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.20 makes file transfer faster and easier, and turns clipboard sharing into real automatic sync.

**SHA-256:** `8610D53F062006AC49AC9771E59F63C1FB27D6DF61ACB1B3DC449A3EF0748596`

---

### 0.3.19 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.19 fixes the picture flicker seen when viewing a remote PC whose screen is mostly still.

**SHA-256:** `FF11D802EA587428D4029C91852D581BFCF03A925C69E517C0E59D4418621231`

---

### 0.3.18 — 2026-10-26

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.18 makes streaming smoother on fast and slow connections alike, and tidies up the main window.

**SHA-256:** `EC36ACAFAAE5B6F43602E7BB9CDA2DB6FFD94544172C0148AA058348AB2FCF18`

---

### 0.3.17 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.17 moves the session toolbar out of the way of the remote desktop and makes relayed sessions steadier.

**SHA-256:** `40584F01D4589908C7B807013EC581046D6AD9D029086C971BFDF4B74572B0DE`

---

### 0.3.16 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.16 fixes a flickering picture when the remote screen is idle, and makes reconnecting after a remote restart faster.

**SHA-256:** `7A771B9928DC937AE06EE286B36CA3E448B2B567B1D9D95D262CD1CF3F68FEFC`

---

### 0.3.15 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.15 adds remote restart: restart the PC you are connected to, and AtlasLink reconnects when it is back.

**SHA-256:** `26F575CCB35C53C973004B1B2BE912C1788ACE15BA60BDD6F18DB639DE326DC9`

---

### 0.3.14 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.14 adds an enrolment token, so only PCs you set up can join your server. PCs that are already registered keep working without any change.

**SHA-256:** `4E52300C0D325993D76A110E583FCE66AF7D76AF196672D2C42DCE02FD5874E6`

---

### 0.3.13 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.13 makes the TURN relay more dependable and keeps relayed sessions from overloading the relay server.

**SHA-256:** `E21DFDF7672F7609724F087D35139A225909A6EC4B58C3522E6BCCD775F001E3`

---

### 0.3.12 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.12 updates itself. Install it once; from then on new releases download and install automatically.

**SHA-256:** `EF8E7043B353A1D6EEE24B50CB17803FC5C5E2F89AD62EA9655ECAA12BF4E4DE`

---

### 0.3.11 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.11 keeps admin console sessions across server restarts. Redeploying or restarting the control server no longer signs administrators out.

**Compatibility:** Backward compatible: 0.3.10 and older desktop apps work with a 0.3.11 server without changes

**SHA-256:** `BA3DA5751967DF2E8B6C86419749F5D8C1BA694048BF924A93D1EC0CED5FACD9`

---

### 0.3.10 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.10 fixes the most common cause of sessions being interrupted. Short drops of the connection to the control server — even a few seconds — ended the session about 30 seconds later, even though everything had already reconnected.

**SHA-256:** `5ABE07964BFBFF2EEE6B2CEB30C2A1F9160F6D030572B8E29EC4B4E4CFB5D48A`

---

### 0.3.9 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.9 adds a Server information tab to the admin console, connection health insights, and tools to diagnose TURN relay configuration.

- **Server tab** — View server version, uptime, database size, configuration summary, and security warnings with actionable fixes
- **TURN diagnostics** — Test relay tool performs a real allocation attempt to distinguish secret mismatches from unreachable servers
- **Connection health insights** — See which PCs drop connections most often or need the relay, ranked over the last 7 days
- **Outdated app detection** — Devices below the current server version are tagged and filterable
- **Direct/Relay session badges** — Sessions now show whether they used a direct peer connection or relayed through TURN
- **Better error responses** — Wrong-password replies are 403 instead of 401, preventing unintended sign-outs from the admin console

**Compatibility:** Backward compatible: 0.3.8 and older desktop apps work with a 0.3.9 server without changes

**SHA-256:** `B46FF9677A42F0903486BC95BBB462AE6F1DE6A50EBDC20B55DC5ACFF1641FF1`

---

### 0.3.8 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.8 fixes critical bugs that prevented TURN relays from working. Sessions between PCs that cannot reach each other directly (different networks, strict firewalls, mobile hotspots) now fall back to your relay as intended.

- **Fixed: TURN relay never used after app starts** — Relay credentials sent at sign-in were lost before the app was ready, so relays only worked after the first reconnect
- **Fixed: TURN relay stops working after an hour** — Credentials expired but were never renewed, breaking relayed sessions silently

**Compatibility:** Backward compatible: 0.3.8 clients work with 0.3.7 and earlier servers. The startup fix works immediately; periodic renewal is silently disabled if the server doesn't support it

**SHA-256:** `45479D677D6C8E9B4CA85B4059454826561E95E5B2F6187A4C7F87F9695551A0`

---

### 0.3.7 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.7 adds visibility into connection interruptions. When a session shows **Reconnecting**, the desktop app reports the duration and outcome to the control server, and administrators can see these events in the audit log and overview dashboard.

- **Interruption reports in audit log** — New **Session interrupted** event type records what dropped (signaling / peer / ICE), duration, and outcome
- **24-hour interruption metrics** — Admin overview shows interruption counts split by outcome (recovered / ended)
- **Automatic reporting** — Desktop app sends reports when reconnections finish, queued if the control server itself was down

**Compatibility:** Backward compatible: 0.3.7 clients work with 0.3.4, 0.3.5, and 0.3.6 servers. The interruption reporting feature is silently disabled if the server doesn't support it

**SHA-256:** `88E71AEA4E6FB6F314F88E1F52A45078B0019D3E8F0449D6176019CEA3D3CBB2`

---

### 0.3.6 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.6 fixes a critical bug that could end a session immediately after the password was accepted and then leave the remote PC unavailable for about a minute.

- **Fixed: Session ending with "Could not start screen capture" error** — Duplicate password proofs no longer crash the session
- **Fixed: Remote PC stuck "busy" after failed connection** — The host PC now recovers immediately instead of being unavailable for ~60 seconds

**Compatibility:** Backward compatible: 0.3.6 clients work with 0.3.4 and 0.3.5 servers

**SHA-256:** `69C3EB71E83486B075FB4A12119FAC1BAFED1A413FCEAE9CD82BDF1A4C1CFCD6`

---

### 0.3.5 — 2026-10-25

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.5 delivers video quality and session-handling improvements that were described in the 0.3.4 release notes but were not included in that installer. If you installed 0.3.4, upgrade to get these features.

- **Smoother video on lossy networks** — Lost packets are retransmitted instead of forcing keyframes that freeze the picture
- **Faster quality ramp-up** — Sessions start at 2 Mbps (up from ~0.3 Mbps) so the picture is sharp within seconds
- **Sharper text under congestion** — Screen content maintains resolution when bandwidth drops instead of becoming blurry
- **Reliable session cancel** — Outgoing connections can be cancelled immediately, and stale incoming prompts are cleaned up properly
- **Redesigned audit log** — Summary cards with 24-hour counts act as filters, and events expand to show full details

**Compatibility:** Backward compatible: 0.3.5 clients work with 0.3.4 and 0.3.5 servers. The video and session-cancel improvements apply when both PCs are running 0.3.5

**SHA-256:** `42216393F06E0886A26080B310844E881FA9838981061A84FB5D5B173E281035`

---

### 0.3.4 — 2026-10-24

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.4 is a security and reliability release.

- **Hardened admin console sign-in** — Optional TOTP two-factor authentication, cookie-based sessions with token revocation, account-level login throttling
- **Safer defaults for new servers** — Open access between devices is now off by default
- **TURN relay configuration** — Manual TURN/STUN servers in Settings now take effect
- **New default control-server port** — 7080 (previously 8080)

**Compatibility:** Backward compatible: 0.3.3 clients continue to work with 0.3.4 servers

**SHA-256:** `8F9951109783B55F7B2CFDD8433CE15EF8C2E3A63E085F003A9524310F985D7D`

---

### 0.3.3 — 2026-10-23

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.3.3 features a redesigned session viewer, file transfer, multi-monitor support, and remote audio.

- **Redesigned session viewer** — Floating controls with auto-hiding toolbar, side drawers for chat/files/info
- **Bidirectional file transfer** — Transfer files over encrypted WebRTC data channel (permission default-off)
- **Multi-monitor switching** — Switch between displays without reconnecting; supports Fit/Actual/Stretch view modes
- **Remote system audio** — One-way system audio playback from host to viewer (opt-in permission)
- **Server hardening** — Dev/live database isolation, atomic ownership claiming, unapproved devices blocked

**Compatibility:** Backward compatible: Older clients (0.2.x) work with 0.3.3 servers

**SHA-256:** `0AC480E7ACA0375CD040BBB02DA9F1DD041E6AB9F081EFBB4CCA02039E94DD70`

---

### 0.2.2 — 2026-10-23

**[Release Notes](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.22)**

**Installer:** `AtlasLink-0.3.22-setup.exe` (77.7 MB)

AtlasLink 0.2.2 introduces session management, chat, and connection history features.

- **Session Manager** — Manage active sessions with permission profiles and live controls
- **In-session text chat** — Real-time messaging over encrypted WebRTC data channel
- **Recent Connections** — Connection history with favorites, custom aliases, and quick reconnect
- **Remembered passwords** — Optional password storage using Windows DPAPI encryption
- **Automatic cleanup** — Orphaned sessions are cleaned up on server restart

**SHA-256:** `F5DE7549F87F263A37ABCED17FA573DECE58034749CB942946C36292D3B5CAAE`


## 📋 Releases & Changelog

Each release includes detailed release notes on the [**Releases page**](https://github.com/jmveloso/atlaslink-releases/releases) with:

- New features and improvements
- SHA-256 checksums for verification
- Installation and upgrade instructions
- Known issues and limitations

---

## 📞 Support

For installation assistance, configuration questions, or technical support, please contact your organization's IT administrator.

---

## ⚠️ Important Notes

- **Source code:** The AtlasLink source code is maintained in a separate private repository. This repository hosts only the public release installers.
- **Self-hosted only:** AtlasLink requires a self-hosted control server managed by your organization's IT department.
- **Windows only:** This release supports Windows 10 and Windows 11. macOS and Linux support is planned for future releases.

---

**Repository maintained by:** AtlasLink Release Engineering  
**License:** Proprietary — See release notes for terms
