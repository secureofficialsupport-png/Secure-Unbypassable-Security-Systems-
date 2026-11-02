# Secure

A security-first Discord bot. Secure watches your server for nukes, raids, rogue bots and compromised admins, stops dangerous actions as they happen, and restores what was changed.

Support server: https://discord.gg/pexjbguSfp

---

## What it does

Secure is built around one job: keeping a server safe. The moderation, logging and management tools around it are there to support that.

**Anti-Nuke**
- 28 protection modules on by default, adjustable per server with the `ss` command
- Dangerous actions are validated before they run, so one account can't quietly weaken your protections
- Panic mode locks the server down instantly
- Baseline snapshots of roles, channels and permissions, used to detect nuking and restore the original setup

**Trust and access control**
- Whitelist for users, roles and bots that may bypass protections
- Extra Owners with limited, configurable authority, who cannot whitelist dangerous permissions
- Gmail 2FA approval for high-risk actions such as disabling Anti-Nuke (`ss authorize` / `ss cancel`)

**Bot-add protection**
- A newly added bot must be in 1000+ servers, be at least 1 year old and be verified by Discord
- Bots that fail, and whoever added them, are banned
- The server owner can whitelist a trusted bot with `/wl bot add`

**Quarantine and moderation**
- Quarantine isolates suspicious members in restricted holding channels
- Ban, kick, mute, warn, timeout, lock, bulk clear and case history
- Native AutoMod rules, anti-spam and honeypots
- Rogue permission scans for bots and roles

**Logging and recovery**
- Multi-channel audit logs for bans, kicks, role and channel changes and bot additions
- Daily email digest of security events
- Full server backups and templates with instant restore

**Everything else**
Tickets, applications, giveaways, verification with captchas, welcome and leave cards, temp voice channels, text-to-speech, voice recording, economy, leaderboards, QR codes, custom commands, and a Container Builder (`/container-builder`) for custom containers with link buttons. Run `/help` or `!help` in Discord to see every module.

---

## Requirements

- Node.js 20 or newer (discord.js 14)
- A Discord application with a bot token
- One database: SQLite (default, nothing to set up), MySQL/MariaDB, or PostgreSQL
- Optional: MongoDB, to keep the bot's JSON data files backed up and restore them on a fresh host
- Optional: Redis, ffmpeg (bundled through `ffmpeg-static`), and a Gmail account for 2FA and email digests

---

## Setup

```bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPO.git
cd YOUR-REPO
npm install
```

Create a `.env` file in the project root (or at `data/.env`):

```env
# Required
TOKEN=your-bot-token
CLIENT_ID=your-application-id
BOT_OWNER_ID=your-discord-user-id

# Database (pick one; leave all empty to use SQLite)
# DATABASE_URL=mysql://user:password@host:3306/dbname
# DATABASE_URL=postgres://user:password@host:5432/dbname
# MYSQL_HOST=
# MYSQL_PORT=3306
# MYSQL_USER=
# MYSQL_PASSWORD=
# MYSQL_DATABASE=

# Optional
# MONGO_URL=            # backup and restore of the JSON data files
# REDIS_URL=
# SHARD_COUNT=          # number, or leave empty for auto
# GMAIL_USER=           # sender for 2FA codes and the daily digest
# AZURE_TTS_KEY=
# AZURE_TTS_REGION=
# WEATHER_API_KEY=
# KLIPY_API_KEY=
# PORT=
```

The token can also be set as `DISCORD_BOT_TOKEN` or `DISCORD_TOKEN`.

**Never commit your `.env` file or the JSON data files in the project root.** The included `.gitignore` already excludes them.

### Invite the bot

```
https://discord.com/oauth2/authorize?client_id=YOUR_CLIENT_ID&scope=bot%20applications.commands&permissions=8
```

Secure asks for Administrator because it needs to act above every other role to stop attacks.

### Register the slash commands

Slash commands are not registered automatically on startup. Register them once, and again whenever commands change:

- In Discord, as the bot owner: `!registercommands`, or
- From the terminal: `node scripts/sync.js`

New global commands can take a few minutes to show up.

### Run

```bash
node shard.js     # sharded, recommended for production
node index.js     # single process, good for development
```

---

## Using it in a server

1. Invite the bot and move its roles to the very top, in this order: **Secure**, then **Secure Antinuke**, then **Secure Unbypassable**, above every other role.
2. Anti-Nuke needs a server with at least 100 members.
3. Run `/help` or `!help`, and use `ss` to review the security status and modules.
4. Add the people and bots you trust with `wl`, and set up Extra Owners with `extraowner`.
5. Link a Gmail address for 2FA with `ss email <address>`.

The default prefix is `!`. Change it with `@Secure prefix <new prefix>` or `/prefix`.

---

## Project layout

```
index.js            launcher (loads env, restores data, starts the bot)
shard.js            sharding manager entry
src/
  index.js          main bot file
  config/           paths and owner settings
  core/             auth, errors, stats, uptime, eval mode
  database/         drivers (sqlite, mysql, postgres, mongo), schema, repositories
  security/         security manager, automod, quarantine, blacklist, verification, backups
  commands/         slash and prefix handlers, help menu, utilities, economy
  events/           messageCreate and other event registrars
  features/         voice, music, welcome, roles, community, tickets, logging, channels
utils/              shared helpers
scripts/            one-off tools (command sync, verify-structure, and others)
```

---

## Privacy

Secure stores only what it needs to do its job: server and owner IDs, security settings, role and channel baselines, audit events and ban or blacklist records. Data is not sold. The full Terms of Service and Privacy Policy are shown by the bot when it joins a server.

To ask what is stored about your server or to request removal, join the support server.

---

## Reporting security issues

Please don't open a public issue for a vulnerability. Report it privately in the support server or by contacting the maintainer directly. Valuable reports can earn the Bug Hunter role.

---

## Contributing

Issues and pull requests are welcome. Before opening a pull request:

1. Keep changes focused on one thing.
2. Run `node scripts/verify-structure.js` to check that files load and requires resolve.
3. Describe what you changed and how you tested it.

---

## License

Add your license here (for example MIT or GPL-3.0) and include a `LICENSE` file in the repo.

---

Maintained by the Secure team. Join us at https://discord.gg/pexjbguSfp
