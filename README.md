# ☁️ DEVXENX CLOUD HOST — Advanced Hosting Panel

🚀 Telegram-powered multi-user project hosting controller with a responsive owner dashboard.

## ✨ Included features

- 📤 Telegram upload: `.py`, `.js`, `.zip`
- 🐍 Python 3.12 runtime and 🟩 Node.js 22 runtime
- 📦 Automatic install from `requirements.txt` / `package.json`
- 🐳 Per-project Docker containers with RAM, CPU and PID limits
- 🔒 Dropped Linux capabilities, `no-new-privileges`, read-only container root and temporary `/tmp`
- ▶️ Start / 🛑 Stop / 📋 Output logs
- 🖥️ **Live Terminal tab in the web dashboard** — read-only container stdout/stderr, auto-refreshes every 2 seconds
- 📁 File manager, ZIP extraction protections and upload quotas
- 👥 User management, ban/unban, broadcast and admin controls
- 📊 Dashboard statistics, active bots, RAM/CPU, uptime and health endpoint
- 💾 SQLite database and persistent uploaded project storage
- 📱 Responsive dark dashboard and public landing page
- 🔗 Owner social links and channel links shown on the public landing page

## 🌐 Owner / community links

- 👑 Owner: `@devxenx`
- ✈️ Telegram Channel: https://t.me/+zaKlThzP5iJiMDE1
- 🟢 WhatsApp Channel: https://whatsapp.com/channel/0029VbDpg0e7j6g0DZrTLh2X
- ▶️ YouTube: https://youtube.com/@devxenx
- 📸 Instagram: https://instagram.com/devxenx
- 🎮 Discord username: `@devxenx` (profile URL is not assumed because Discord usernames are not universal profile IDs)
- 𝕏 X: https://x.com/devxenx

## ⚠️ Runtime support — read before deploying

This release runs **Python and Node.js projects only**. ZIP upload is a packaging method, not a separate language runtime. It does not yet run every language, arbitrary native binaries, PHP websites, databases, static sites, Java, Go, Rust, Ruby, or .NET projects. Those require separately designed runtimes/buildpacks and additional isolation. Do not advertise “every language” until those runtimes are implemented and tested.

The dashboard's Live Terminal is a **read-only log console** for hosted containers; it is not an interactive shell and cannot execute arbitrary commands on the VPS. This is intentional to reduce the risk of exposing host-level command execution.

## 🛡️ Deployment requirements

Use an Ubuntu 22.04/24.04 VPS or dedicated Linux host with Docker Engine and Docker Compose plugin. Normal cPanel shared hosting cannot run Docker containers. Suggested starting size: 2 vCPU / 4 GB RAM for a small test deployment; capacity depends on the workloads you allow.

The controller needs access to the Docker socket. A compromised controller with Docker socket access can compromise the host. Run on a dedicated VPS, restrict dashboard access, keep the host patched, and do not treat Docker as a perfect security boundary for hostile public workloads. For a public multi-tenant commercial service, use separate worker hosts/VMs, network egress controls, per-user quotas, monitoring, backups, and an independent security review.

## 🚀 Install on Ubuntu VPS

1. Install Docker Engine and the Compose plugin using Docker's official Ubuntu instructions: https://docs.docker.com/engine/install/ubuntu/
2. Upload and extract this ZIP on the VPS, e.g. `/opt/devxenx-cloud`.
3. From the project directory run:

   ```bash
   cp .env.example .env
   nano .env
   ```

4. Set a fresh Telegram bot token, your numeric Telegram `OWNER_ID` and `ADMIN_ID`, and a strong `ADMIN_PASSWORD` of at least 16 characters. Never publish `.env`.
5. Set `HOST_UPLOADS_PATH` to an absolute path on the VPS host if you change the default storage location. The container bind mount and host path must match.
6. Start:

   ```bash
   docker compose up -d --build
   ```

7. Follow controller logs:

   ```bash
   docker compose logs -f
   ```

8. Open `http://YOUR_VPS_IP:5000/admin`. The browser Basic Auth prompt uses `ADMIN_PASSWORD` as the password; use any non-empty username.
9. For production, put the panel behind Nginx + HTTPS and a VPN/IP allowlist. Do not expose port 5000 directly to the public internet.

## 📦 Project formats

### Python

```text
my-python-project/
├── main.py
└── requirements.txt
```

### Node.js

```text
my-node-project/
├── index.js
└── package.json
```

Upload a supported `.py` or `.js` file, or a ZIP containing the project and dependency manifest at its root. Start it from the Telegram bot controls. The dashboard's 🖥️ Live Terminal tab shows recent container output and refreshes automatically.

## ⚙️ Fair-use settings

Edit `.env`:

- `MAX_FILES_PER_USER=25`
- `MAX_UPLOAD_MB=20`
- `MAX_RUNNING_PER_USER=2`
- `RUNNER_MEMORY=256m`
- `RUNNER_CPUS=0.50`
- `RUNNER_PIDS=128`

Tune limits to the VPS capacity. Restart after changes:

```bash
docker compose up -d --build
```

## 🔐 Security checklist

- Revoke and replace any bot token that has ever been exposed.
- Use a unique strong dashboard password.
- Keep `.env`, `inf/`, and uploaded projects outside any public web root.
- Restrict panel access behind HTTPS and VPN/IP allowlist.
- Keep Docker and Ubuntu updated; back up SQLite and uploaded data off-server.
- Set strict upload quotas and monitor resource consumption.
- 24/7 uptime depends on VPS provider, host health, network and correct operations; it is not guaranteed by the software.

## 🧪 Validation

```bash
python -m py_compile app.py
```

Docker deployment and real-world workload isolation must still be tested on the target VPS before public launch.
