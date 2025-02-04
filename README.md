<div align="center">

<img src="./banner.jpg" alt="Claude Telegram Supercharged" width="100%" />

<h3>The official Claude Code Telegram plugin is good. This one is better.</h3>

<br />

<a href="https://github.com/k1p1l0/claude-telegram-supercharged/blob/master/LICENSE"><img src="https://img.shields.io/github/license/k1p1l0/claude-telegram-supercharged?style=flat" alt="License" /></a>
<a href="https://github.com/k1p1l0/claude-telegram-supercharged/stargazers"><img src="https://img.shields.io/github/stars/k1p1l0/claude-telegram-supercharged?style=flat" alt="GitHub Stars" /></a>
<a href="https://github.com/k1p1l0/claude-telegram-supercharged/commits/master"><img src="https://img.shields.io/github/last-commit/k1p1l0/claude-telegram-supercharged?style=flat" alt="Last Commit" /></a>

<br />

<a href="#getting-started">Getting Started</a>
<span>&nbsp;&nbsp;&bull;&nbsp;&nbsp;</span>
<a href="#features">Features</a>
<span>&nbsp;&nbsp;&bull;&nbsp;&nbsp;</span>
<a href="#agentic-mode">Agentic Mode</a>
<span>&nbsp;&nbsp;&bull;&nbsp;&nbsp;</span>
<a href="#tools-exposed-to-the-assistant">Tools Reference</a>
<span>&nbsp;&nbsp;&bull;&nbsp;&nbsp;</span>
<a href="#contributing">Contributing</a>

<br />
<hr />
</div>

https://github.com/user-attachments/assets/51f9f8c4-886d-4aec-bf2a-dddc8ef15cfc

## Claude Telegram Supercharged

Drop-in upgrade for the [official Claude Code Telegram plugin](https://github.com/anthropics/claude-plugins-official/tree/main/external_plugins/telegram). Install once, get 15+ features the official plugin doesn't have. Built on top of the [official plugin](https://github.com/anthropics/claude-plugins-official/tree/main/external_plugins/telegram) -- everything works, just better.

2 minutes to install. Zero config. Your existing bot and pairing keep working.

### Install in one line

Install the official plugin first (in Claude Code: `/plugin install telegram@claude-plugins-official`), then:

```sh
curl -fsSL https://raw.githubusercontent.com/k1p1l0/claude-telegram-supercharged/master/install.sh | bash
```

Run it again any time to update. The full walkthrough is in [Getting Started](#getting-started).

> **Security:** the only official source is [github.com/k1p1l0/claude-telegram-supercharged](https://github.com/k1p1l0/claude-telegram-supercharged). This project never ships zip or exe downloads. Copies elsewhere that offer a zip file are malware; please don't run them, and report them to GitHub.

### Live demo

One take in a real chat on a Mac, 52 seconds. A voice message goes in and local whisper transcribes it. Agentic Mode shows each tool call while Claude checks GitHub, and the answer comes back as an ElevenLabs voice message. Then `/agents` opens the Agents Board. Waits and typing are sped up, and the video shows by how much.

https://github.com/user-attachments/assets/4c5a5968-b5cc-43f1-a756-4282d3eedae9

### Official vs Supercharged

| | Official plugin | Supercharged |
| --- | --- | --- |
| Text, photos, files, stickers | ✅ | ✅ |
| Formatted replies, long-message splitting | ✅ | ✅ |
| Voice messages | Raw audio file | ✅ Transcribed (OpenAI, Groq, Deepgram or local Whisper) |
| Voice replies | ❌ | ✅ ElevenLabs |
| Live progress while Claude works | ❌ | ✅ [Agentic Mode](#agentic-mode) |
| History and memory that survive restarts | ❌ | ✅ SQLite + memory file |
| Knows which message you replied to | ❌ | ✅ Reply context, quotes, forwards |
| Questions with inline buttons | ❌ | ✅ `ask_user` |
| Scheduled messages and reminders | ❌ | ✅ Also syncs to Apple Reminders |
| Google Calendar | ❌ | ✅ |
| 24/7 daemon with auto-restart | ❌ | ✅ launchd supervisor |
| Model routing (Haiku / Sonnet / Opus) | ❌ | ✅ |
| Long answers as Telegraph articles | ❌ | ✅ |
| Approve permission prompts from Telegram | ✅ | ✅ Buttons or "yes <id>", plus an auto-approve list |
| Live board of sessions, subagents and tokens | ❌ | ✅ [`/agents` Mini App](#agents-board-mini-app) |

## Features

### Voice & Audio

| Feature | What it does |
| --- | --- |
| **🎤 Voice Messages** | Talk to Claude. Provider fallback chain: OpenAI Whisper → Groq → Deepgram → local whisper-cli. Works from your phone while walking. |
| **🔊 Voice Replies (TTS)** | Claude replies with voice messages via ElevenLabs TTS. OGG/Opus native format. Auto-fallback to audio file if voice is restricted. |
| **🎤 Auto-transcribe** | ALL voice messages in group chats are transcribed, even without mentioning the bot. Configurable via `autoTranscribe`. |

### Messages & Media

| Feature | What it does |
| --- | --- |
| **📨 Forwarded Messages** | Full forwarding context preserved -- Claude sees who originally sent it and from which chat/channel. |
| **📦 Message Batching** | Forward 20+ messages at once -- collected into one batch (5s debounce), auto-summarized instantly, then Claude responds to the whole conversation in one reply. |
| **📄 Document Support** | Send PDFs, DOCX, CSV, TXT, JSON -- Claude downloads, reads, and summarizes. 10MB file size limit. |
| **🎨 MarkdownV2 Auto-escaping** | Special characters auto-escaped server-side -- Claude writes natural text, no manual `\.` escaping needed. |
| **🔤 Incoming Formatting** | Bold, italic, strike, underline, spoiler, code, code blocks with language, quotes and hidden links in your messages reach Claude as Markdown, instead of being dropped by Telegram's plain-text `text` field. Captions and replied-to messages too. |
| **😎 Sticker & GIF Support** | Claude sees stickers and GIFs. Static as images, animated as multi-frame collages. |
| **📰 Telegraph Instant View** | Long research (3000+ chars) published to telegra.ph as Instant View. Disabled by default -- opt-in via `TELEGRAPH_ENABLED=true`. |

### Conversations & Groups

| Feature | What it does |
| --- | --- |
| **💬 Message History** | SQLite-backed rolling store. Claude has context across restarts. `get_history` + `search_messages` tools. |
| **🧠 Conversation Memory** | `/clean` saves a summary before clearing. Memory persists across sessions. Claude never forgets. |
| **🧵 Conversation Threading** | Follows reply chains in groups, sees who said what, responds in the correct thread. Up to 3 levels deep. |
| **📋 Forum Topics** | Telegram Forum topics fully supported. Each topic isolated with persistent `thread_id`. |
| **👥 Group Pairing** | Add bot to group, mention it, get pairing code. No hunting for numeric chat IDs. |
| **🎯 Inline Buttons** | `ask_user` tool -- tappable buttons for confirmations and choices. |
| **👍 Reaction Status** | 👀 read → 🔥 working → 👍 done. Voice messages get ✍ for transcription. |

### Daemon & Infrastructure

| Feature | What it does |
| --- | --- |
| **⚡ Two-Tier Model Routing** | Configurable router: Haiku (fast, 200K), Sonnet (balanced, 1M), or Opus (deep, 1M). Set via `TELEGRAM_ROUTER_MODEL`. Complex tasks auto-escalate to Opus via subagents (change the target with `TELEGRAM_ESCALATION_MODEL`). |
| **🛠 Agentic Mode** | Watch Claude work: "typing…" the whole time, plus a live "Working… (Ns)" message with Claude's notes and each tool call as it runs. The answer replaces it in place. `/verbose 0\|1\|2`, `/status`, `/new`. [Details](#agentic-mode) |
| **📊 Agents Board** | `/agents` opens a Mini App in Telegram: every Claude Code session on the Mac, its subagents, context use, tokens per hour and the latest tool calls. [Details](#agents-board-mini-app) |
| **🔄 Daemon Mode** | Supervisor auto-restarts Claude on crash or context reset. Memory preserved, zero downtime. |
| **🛡 Context Watchdog** | Auto-restarts when context exceeds 50%, or after 2 hours of uptime, to keep sessions responsive. SQLite history and memory survive restarts. |
| **🔒 Single-Instance Lock** | PID-based lock file prevents duplicate bot instances competing for Telegram updates. || **🖥 Daemon Management** | `/telegram:daemon start\|stop\|restart\|status\|logs` -- full lifecycle. `/telegram:monitor` for health dashboard with remote control URL. |
| **⏰ Scheduled Messages** | `schedule` tool for reminders and recurring tasks. "at" (one-shot) and "every" (interval) types. Persists across restarts. |
| **📅 Google Calendar** | Check schedule, create events, daily briefings from Telegram. Multi-account support. Proactive — Claude uses calendar context when answering. |
| **📸 Headless Screenshots** | Playwright-based page capture -- works in daemon mode where Chrome isn't available. |
| **✅ Reaction Validation** | Client-side emoji whitelist prevents cryptic Telegram API errors. |
| **🔒 Shell Injection Protection** | All subprocess calls use `spawnSync` with array args. No shell interpretation. |
| **📊 Smart Caching** | Voice/audio cached between middleware and handlers. No double downloads or transcriptions. |

## Getting Started

> Default pairing flow for a single-user DM bot. See [ACCESS.md](./ACCESS.md) for groups and multi-user setups.

### Prerequisites

- [Bun](https://bun.sh) -- the MCP server runs on Bun. Install with `curl -fsSL https://bun.sh/install | bash`.

### 1. Create a bot with BotFather

Open a chat with [@BotFather](https://t.me/BotFather) on Telegram and send `/newbot`. BotFather asks for two things:

- **Name** -- the display name shown in chat headers (anything, can contain spaces)
- **Username** -- a unique handle ending in `bot` (e.g. `my_assistant_bot`). This becomes your bot's link: `t.me/my_assistant_bot`.

BotFather replies with a token that looks like `123456789:AAHfiqksKZ8...` -- that's the whole token, copy it including the leading number and colon.

### 2. Install the [official plugin](https://github.com/anthropics/claude-plugins-official/tree/main/external_plugins/telegram)

These are Claude Code commands -- run `claude` to start a session first.

```
/plugin install telegram@claude-plugins-official
```

### 3. Apply the supercharged version

Clone this repo and install both the supercharged server and the daemon supervisor:

```sh
git clone https://github.com/k1p1l0/claude-telegram-supercharged.git
PLUGIN_DIR=~/.claude/plugins/cache/claude-plugins-official/telegram/$(ls ~/.claude/plugins/cache/claude-plugins-official/telegram/ | sort -V | tail -1)
cp claude-telegram-supercharged/server.ts claude-telegram-supercharged/miniapp.ts $PLUGIN_DIR/
cp -R claude-telegram-supercharged/miniapp $PLUGIN_DIR/
mkdir -p ~/.claude/scripts
cp claude-telegram-supercharged/supervisor.ts ~/.claude/scripts/telegram-supervisor.ts
cp claude-telegram-supercharged/scripts/claude-daemon-wrapper.exp ~/.claude/scripts/claude-daemon-wrapper.exp
chmod +x ~/.claude/scripts/claude-daemon-wrapper.exp
cp claude-telegram-supercharged/scripts/telegram-progress-hook.ts ~/.claude/scripts/telegram-progress-hook.ts
```

### 4. Give the server the token

```
/telegram:configure 123456789:AAHfiqksKZ8...
```

Writes `TELEGRAM_BOT_TOKEN=...` to `~/.claude/channels/telegram/.env`. You can also write that file by hand, or set the variable in your shell environment -- shell takes precedence.

### 5. Relaunch with the channel flag

The server won't connect without this -- exit your session and start a new one:

```sh
claude --channels plugin:telegram@claude-plugins-official
```

Or use the **daemon supervisor** for always-on operation with auto-restart and context reset from Telegram (see [Daemon Mode](#daemon-mode)):

```sh
bun ~/.claude/scripts/telegram-supervisor.ts
```

### 6. Pair

With Claude Code running from the previous step, DM your bot on Telegram -- it replies with a 6-character pairing code. If the bot doesn't respond, make sure your session is running with `--channels`. In your Claude Code session:

```
/telegram:access pair <code>
```

Your next DM reaches the assistant.

> Unlike Discord, there's no server invite step -- Telegram bots accept DMs immediately. Pairing handles the user-ID lookup so you never touch numeric IDs.

### 7. Lock it down

Pairing is for capturing IDs. Once you're in, switch to `allowlist` so strangers don't get pairing-code replies. Ask Claude to do it, or `/telegram:access policy allowlist` directly.

### Updating

> **Important:** The official plugin auto-updates and will overwrite your supercharged `server.ts`. When the bot suddenly stops working after an update, this is why.

The quickest way to update (also after an official plugin update) is to re-run the [one-line installer](#install-in-one-line). To do it by hand:

When the official plugin updates (check for new version directories in `~/.claude/plugins/cache/claude-plugins-official/telegram/`):

```sh
cd claude-telegram-supercharged
git pull
# Find the current version (e.g. 0.0.4)
PLUGIN_VERSION=$(ls ~/.claude/plugins/cache/claude-plugins-official/telegram/ | sort -V | tail -1)
echo "Updating to version: $PLUGIN_VERSION"
cp server.ts miniapp.ts ~/.claude/plugins/cache/claude-plugins-official/telegram/$PLUGIN_VERSION/
cp -R miniapp ~/.claude/plugins/cache/claude-plugins-official/telegram/$PLUGIN_VERSION/
cp supervisor.ts ~/.claude/scripts/telegram-supervisor.ts
# Copy skills
cp -r skills/* ~/.claude/plugins/cache/claude-plugins-official/telegram/$PLUGIN_VERSION/skills/
# Copy scripts
cp scripts/claude-daemon-wrapper.exp ~/.claude/scripts/claude-daemon-wrapper.exp
cp scripts/telegram-progress-hook.ts ~/.claude/scripts/telegram-progress-hook.ts
```

Then restart your daemon or Claude Code session.

## Tools Exposed to the Assistant

| Tool | Purpose |
| --- | --- |
| `reply` | Send to a chat. Takes `chat_id` + `text`, optionally `reply_to` (message ID) for native threading, `files` (absolute paths) for attachments, and `parse_mode` (MarkdownV2/HTML/plain, defaults to MarkdownV2). Images (`.jpg`/`.png`/`.gif`/`.webp`) send as photos with inline preview; other types send as documents. Max 50MB each. Auto-chunks text; files send as separate messages after the text. Returns the sent message ID(s). |
| `react` | Add an emoji reaction to a message by ID. **Only Telegram's fixed whitelist** is accepted (👍 👎 ❤ 🔥 👀 🎉 😂 🤔 etc). Also used for status indicators (👀 read → 👍 done). |
| `edit_message` | Edit a message the bot previously sent. Supports `parse_mode` (MarkdownV2/HTML/plain). Useful for "working..." → result progress updates. Only works on the bot's own messages. |
| `ask_user` | Send a question with inline keyboard buttons and wait for the user's choice. Takes `chat_id`, `text`, `buttons` (array of labels), optional `parse_mode` and `timeout` (default 120s). Returns the label of the tapped button. |
| `get_history` | Retrieve recent message history from a chat. Takes `chat_id`, optional `limit` (default 50, max 200), optional `before` (unix timestamp for pagination). Returns formatted messages with timestamps, senders, and content. |
| `search_messages` | Search message history by text pattern. Takes `chat_id`, `query` (substring match), optional `limit` (default 20, max 100). Returns matching messages. |
| `clear_history` | Clear all message history for a chat. Always confirm with `ask_user` first, and call `save_memory` before clearing to preserve context. Pass `restart_context: true` to signal the supervisor daemon to restart Claude for a full context reset. |
| `save_memory` | Save a conversation summary to persistent memory. Loaded into Claude's instructions on every startup. Use before `clear_history` so context survives across sessions. |
| `create_telegraph_page` | Publish long-form content to Telegraph (telegra.ph) and return a URL. Telegram renders it as Instant View -- a native article reader. Takes `title`, `content` (Markdown), optional `author_name` and `author_url`. Auto-creates Telegraph account on first use. |

### Inbound Events

| Event | Description |
| --- | --- |
| Text message | Forwarded to Claude as a channel notification with `chat_id`, `message_id`, `user`, `ts`. |
| Photo | Downloaded to inbox, path included in notification so Claude can `Read` it. |
| Emoji reaction | When a user reacts to a bot message, Claude receives a notification with `event_type: "reaction"`, the emoji, and the `message_id`. Use as lightweight feedback. |
| Voice message | Downloaded to inbox as `.ogg`, auto-transcribed by the server if whisper is installed. Transcription replaces "(voice message)" in the notification text. Audio path still included as `audio_path`. |
| Audio file | Forwarded audio files (`.mp3`, etc.) downloaded to inbox, path included as `audio_path`. |
| Sticker | Static `.webp` passed directly as `image_path`. Animated (`.tgs`) and video (`.webm`) stickers converted to multi-frame collage. Emoji and pack name included in text. |
| GIF / Animation | Downloaded and converted to a multi-frame horizontal collage so Claude can see the animation content. |

Inbound messages trigger a typing indicator automatically -- Telegram shows "botname is typing..." while the assistant works on a response.

## Voice & Audio Messages

Voice messages and audio files are downloaded to `~/.claude/channels/telegram/inbox/` and automatically transcribed by the server. The transcription text replaces "(voice message)" in the notification, so Claude receives the spoken text directly.

Think of it as **Wispr Flow for Claude Code**. Open Telegram, hold the mic button, say "refactor the auth middleware to use JWT" -- Claude gets that as text and starts working. No typing, no desktop app needed, works from your phone.

### Transcription Setup

The server tries transcription methods in this order:

1. **OpenAI Whisper API** (recommended) -- fastest, highest quality, non-blocking. Set your API key in `~/.claude/channels/telegram/.env`:
   ```
   OPENAI_API_KEY=sk-proj-...
   ```
   Uses `whisper-1` by default ($0.006/min). You can switch to a different model:
   ```
   OPENAI_WHISPER_MODEL=gpt-4o-transcribe
   ```
   No local install needed. The active transcription method is logged at startup.

2. **[whisper.cpp](https://github.com/ggml-org/whisper.cpp)** (local fallback) -- `brew install whisper-cpp`. Fast C++ port, runs fully offline. Requires a model file:
   ```sh
   # Download the small multilingual model (465MB, good quality/speed balance)
   mkdir -p /usr/local/share/whisper-cpp/models
   curl -L -o /usr/local/share/whisper-cpp/models/ggml-small.bin \
     "https://huggingface.co/ggerganov/whisper.cpp/resolve/main/ggml-small.bin"
   ```
3. **[openai-whisper](https://github.com/openai/whisper)** (local fallback) -- `pip install openai-whisper`. Python-based, slower but also works offline.
4. **No transcriber** -- voice messages are still downloaded and the `audio_path` is included in the notification, but no transcription is provided.

Local options (2 & 3) require **ffmpeg** (`brew install ffmpeg`) for audio format conversion. If you have an OpenAI API key, option 1 is recommended -- it's async (doesn't block the event loop), faster, and more accurate.

### Auto-transcription in history

When `autoTranscribe` is enabled (the default), the server transcribes **all** voice/audio messages in group chats -- even ones that don't mention the bot. This means `get_history` and the auto-injected context always show the spoken text (prefixed with 🎤) instead of `[voice]`. Claude gets full conversational context including what people said in voice messages.

To disable (e.g. to save CPU on busy groups):

```sh
/telegram:access set autoTranscribe false
```

To re-enable:

```sh
/telegram:access set autoTranscribe true
```

## Telegraph (Instant View Articles)

Claude can publish long-form content to [Telegraph](https://telegra.ph) and send it as Instant View links in Telegram -- a native full-screen article reader. Telegraph is **disabled by default** because posts are publicly accessible by URL.

To enable, add to `~/.claude/channels/telegram/.env`:

```
TELEGRAPH_ENABLED=true
```

When enabled, Claude only uses Telegraph for truly long content (3000+ characters with multiple sections) -- research reports, comprehensive analyses, detailed guides. Regular replies always stay in chat.

When disabled (default):
- The `create_telegraph_page` tool is hidden from Claude
- Claude sends all content directly in chat message