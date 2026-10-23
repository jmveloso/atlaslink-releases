# AtlasLink

**Self-hosted remote desktop for Windows** — complete control, zero cloud dependencies.

AtlasLink is a self-hosted remote desktop solution offering full remote control capabilities through a Windows desktop application paired with a self-hosted control server, giving your organization complete ownership and control of your remote access infrastructure.

---

## 🆕 What's New in 0.2.2

- **Session Manager** — Desktop UI for managing active sessions with permission profiles and live session controls
- **In-session text chat** — Real-time messaging over the encrypted WebRTC data channel during remote sessions
- **Recent Connections** — Enhanced connection history with favorites, custom aliases, and quick reconnect
- **Remembered passwords** — Optional password storage using Windows DPAPI encryption for quick reconnects
- **Improved reliability** — Server automatically marks orphaned sessions as ended on restart for accurate session state

---

## 📥 Download

Download the latest Windows installer from the [**Releases page**](https://github.com/jmveloso/atlaslink-releases/releases/latest).

**Installer naming:** `AtlasLink-<version>-setup.exe` (e.g., `AtlasLink-0.2.2-setup.exe`)

---

## ✨ Key Features

### Desktop Application

- **Full remote control** — Complete screen, keyboard, and mouse control over WebRTC
- **In-session text chat** — Real-time messaging over the encrypted WebRTC data channel
- **Session Manager** — Manage active sessions with permission profiles and live controls
- **Recent Connections** — Connection history with favorites, custom aliases, and quick reconnect
- **Remembered passwords** — Optional password storage using Windows DPAPI encryption
- **Multi-monitor support** — Select which display to share during sessions
- **Session security** — Per-session passwords, approval prompts, view-only mode, and automatic timeouts
- **Modern UI** — Dark/light theme support with native Windows styling

### Control Server & Admin Dashboard

- **Self-hosted architecture** — Your infrastructure, your control; no per-seat licensing or cloud dependencies
- **Admin dashboard** — Browser-based management console for device inventory, live session monitoring, and security policy configuration
- **Enterprise security** — Device enrollment approval, session passwords, permission-based access control, and comprehensive audit trails
- **Session reliability** — Automatic orphaned session cleanup on server restart
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
| **Network Ports** | One TCP port for HTTP/HTTPS and WebSocket (default: 8080) |
| **Optional** | TURN relay (only needed for symmetric NAT or restrictive firewalls) |

---

## 🚀 Quick Start

### Desktop Application

1. **Download** the installer from the [Releases page](https://github.com/jmveloso/atlaslink-releases/releases/latest)
2. **Run** the installer and follow the setup wizard
3. **Launch** AtlasLink from the Start Menu or Desktop shortcut
4. **Configure** the Signaling server URL in Settings:
   - Point to your organization's control server (e.g., `ws://office-server:8080`)
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

Verify the integrity of the installer by checking its SHA-256 checksum. The official checksum is published in each release's notes on the [Releases page](https://github.com/jmveloso/atlaslink-releases/releases).

### Windows PowerShell

```powershell
Get-FileHash -Algorithm SHA256 .\AtlasLink-<version>-setup.exe
```

### Windows Command Prompt

```cmd
certutil -hashfile AtlasLink-<version>-setup.exe SHA256
```

Compare the output with the checksum in the release notes.

---

## ⬆️ Upgrading

To upgrade AtlasLink:

1. Download the latest installer from the [Releases page](https://github.com/jmveloso/atlaslink-releases/releases/latest)
2. Run the new installer
3. It will automatically upgrade your existing installation while preserving settings

**Note:** AtlasLink does not include automatic update functionality. Check the Releases page periodically for new versions.

---

## 🔒 Security Notes

- **Default policy:** New installations require device approval and session passwords by default
- **Password storage (server):** All server-side passwords (admin and user accounts) are hashed with PBKDF2 (210,000 iterations, SHA-256)
- **Remembered passwords (desktop):** Optional remembered session passwords are encrypted using Windows DPAPI (Data Protection API) and stored locally
- **Session passwords:** Use challenge-response protocol (no passwords transmitted over the network)
- **WebRTC encryption:** Peer-to-peer connections use DTLS-SRTP encryption for screen content and input; text chat uses the encrypted data channel
- **Admin API:** Bearer token authentication with configurable expiration; rate-limited to 5 failed login attempts per 15-minute window per IP
- **Audit trail:** All administrative actions are logged with timestamps and attribution
- **TLS recommended:** Use TLS certificates in production to encrypt signaling traffic

**Firewall Note:** Restrict control server access to your office network. Use external firewall rules or VPN for production deployments.

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
