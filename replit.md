# Replit.md

## Overview

This is a Minecraft AFK (Away From Keyboard) bot built with Node.js. It uses the `mineflayer` library to connect a bot account to a Minecraft server and keep it online automatically. The bot supports features like auto-authentication, anti-AFK mechanisms, automatic chat messages, and auto-reconnection. A minimal Express web server runs alongside the bot to keep the process alive (commonly used on platforms like Replit or Heroku).

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Bot Core (`bot.js`)
- The main bot logic is in `bot.js`, which is obfuscated JavaScript. It creates a Minecraft bot using `mineflayer` and connects to a server defined in `settings.json`.
- The bot uses `mineflayer-pathfinder` for navigation/movement capabilities.
- Key features controlled via config:
  - **Auto-auth**: Automatically sends login/register commands with a password when joining a server (for offline-mode servers with auth plugins).
  - **Anti-AFK**: Performs actions (like sneaking) to avoid being kicked for inactivity.
  - **Chat messages**: Sends repeating chat messages at configurable intervals.
  - **Auto-reconnect**: Reconnects to the server automatically after disconnection with a configurable delay.

### Web Server (Keep-Alive)
- A simple Express server listens on port 5000 (or `process.env.PORT`).
- It serves a basic response on `/` — this exists solely to keep the process running on platforms that require an HTTP listener.

### Configuration (`settings.json`)
- All bot settings are stored in `settings.json`, which acts as the single configuration file.
- Sections include:
  - `bot-account`: Username, password, and auth type (mojang/microsoft).
  - `server`: IP, port, and Minecraft version to connect to.
  - `position`: Optional fixed position for the bot to navigate to.
  - `utils`: Feature toggles and settings for auto-auth, anti-AFK, chat messages, chat logging, and auto-reconnect.

### Design Decisions
- **Obfuscated source code**: The `bot.js` file uses hex-encoded string arrays for obfuscation. This makes it harder to read but doesn't change functionality. If modifications are needed, the code should ideally be deobfuscated or rewritten cleanly.
- **No database**: The bot is stateless and configuration-driven. No database is used or needed.
- **No frontend**: There is no real frontend — just the keep-alive Express endpoint.

## External Dependencies

### NPM Packages
- **express** (`^4.18.1`): Minimal web server for keep-alive functionality.
- **mineflayer** (`^4.3.0`): Core Minecraft bot framework that handles protocol communication.
- **mineflayer-pathfinder** (`^2.1.1`): Pathfinding plugin for mineflayer, enabling bot movement/navigation.
- **minecraft-data** (`^3.5.0`): Provides Minecraft game data (blocks, items, etc.) used by mineflayer and pathfinder.

### External Services
- **Minecraft Server**: The bot connects to a Minecraft server specified in `settings.json`. Currently configured for an Aternos-hosted server.
- **Mojang/Microsoft Auth**: Supports Mojang and Microsoft authentication for Minecraft accounts (configured via `bot-account.type` in settings).

### Runtime
- **Node.js 16+**: The project targets Node.js v16+ as indicated by devDependencies.
- **Entry point**: `bot.js` (defined in `package.json` as `main`).