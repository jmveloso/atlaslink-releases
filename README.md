# AtlasLink

**Self-hosted remote desktop for Windows** — complete control, zero cloud dependencies.

AtlasLink is a self-hosted remote desktop solution offering full remote control capabilities through a Windows desktop application paired with a self-hosted control server, giving your organization complete ownership and control of your remote access infrastructure.

---

## 🆕 What's New in 0.3.8

- **Redesigned session viewer** — Floating controls with auto-hiding toolbar, side drawers for chat/files/info, and keyboard isolation for better UX
- **Bidirectional file transfer** — Transfer files between host and viewer over encrypted WebRTC data channel (permission default-off)
- **Multi-monitor switching** — Switch between displays without reconnecting; supports Fit/Actual/Stretch view modes with precise pointer mapping
- **Remote system audio** — One-way system audio playback from host to viewer (opt-in permission, not enabled by default)
- **Server hardening** — Dev/live database isolation, atomic ownership claiming, unapproved devices blocked from initiating connections, registration rate limiting

---

## 📥 Download

**Latest Release: [AtlasLink 0.3.8](https://github.com/jmveloso/atlaslink-releases/releases/tag/v0.3.8)** (77.7 MB)

🔗 **[Download AtlasLink-0.3.8-setup.exe](https://github.com/jmveloso/atlaslink-releases/releases/download/v0.3.8/AtlasLink-0.3.8-setup.exe)**

**SHA-256:** `45479D677D6C8E9B4CA85B4059454826561E95E5B2F6187A4C7F87F9695551A0`

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
Get-FileHash -Algorithm SHA256 .\AtlasLink-0.3.8-setup.exe
```

Expected output: `45479d677d6c8e9b4ca85b4059454826561e95e5b2f6187a4c7f87f9695551a0`

### Windows Command Prompt

```cmd
certutil -hashfile AtlasLink-0.3.8-setup.exe SHA256
```

Expected output: `45479d677d6c8e9b4ca85b4059454826561e95e5b2f6187a4c7f87f9695551a0`

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

### 0.3.3 (Latest) — 2026-10-23

Session viewer redesign with floating controls, bidirectional file transfer, multi-monitor switching, remote system audio, and server hardening.

### 0.2.2 — 2026-10-23

Session Manager, in-session text chat, Recent Connections with favorites, remembered passwords, and automatic orphaned session cleanup.

---

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
