# Minecraft Server Hub

Minecraft Server Hub is a desktop application concept for creating, launching, backing up, and monitoring Minecraft servers from a modern local interface.

## What is included

- Guided server creation wizard
- Working Vanilla, Paper, Fabric, and Purpur setup flows
- Forge and Quilt are shown as planned provider integrations
- Resource sliders and live performance estimates
- Small, Medium, Large, and Massive server profiles
- Networking panel with UPnP status, public IP, and connection address
- Server dashboard with status, CPU, RAM, TPS, players, uptime, and logs
- One-click start, stop, restart, backup, restore, and open-folder controls
- Backup scheduling and retention settings
- Expert Mode for advanced settings
- Clean separation between UI, server management, networking, monitoring, and backups
- Extension point for a future global hotkey

## What actually works now

- Creates persistent server records under `~/MinecraftServerHub`
- Downloads real server JARs from Mojang, PaperMC, Fabric, or PurpurMC
- Writes `server.properties` and `eula.txt`
- Starts the server with the local `java` runtime
- Sends `stop` to the running Minecraft process
- Streams real process logs into the dashboard
- Copies real server folders for backups, excluding the downloaded server JAR
- Restores backups by replacing the server folder
- Looks up the public IP address
- Attempts UPnP port mapping through the router's WANIP/WANPPP service

Minecraft servers require Java on the host machine. If Java is missing, the app will show the start failure in the server logs.

## Run locally

Install dependencies, then start the desktop app:

```powershell
npm install
npm start
```

The renderer can also be opened directly for UI review:

```powershell
start src\renderer\index.html
```

## Architecture

```text
src/main
  main.js                 Electron shell and IPC wiring
  preload.js              Safe bridge from UI to desktop services
  services
    serverManager.js      Create, configure, launch, stop, and restore servers
    networkService.js     Network detection and UPnP status
    monitorService.js     Runtime status, parsed logs, and uptime snapshots
    backupService.js      Manual and scheduled backup metadata
    versionCatalog.js     Server types, versions, and profiles

src/renderer
  index.html              App shell
  styles.css              Desktop UI styling
  app.js                  Wizard, dashboard, controls, and state

tests
  profile.test.js         Coverage for profile/resource calculations
```

The current implementation uses real local server files and Java process management. Forge and Quilt still need installer-specific automation before they can be enabled.
