---
name: whatsapp
description: WhatsApp operations and automation — Channels, groups, message aggregation, time-bounded summarization, posting, bots, Business API patterns, and integrations with GitHub Actions, CI, or other tools. Trigger on any WhatsApp-related task involving channels, groups, messaging automation, or tooling.
---

# WhatsApp

Specialize in WhatsApp operations (Channels, groups, and related automation). Prefer self-hosted unofficial clients such as WAHA for full read/write access to channels and groups, as the official Meta Cloud API has limited Channel support (mostly send via business tools, no reliable public read aggregation for arbitrary channels).

This skill currently focuses on Channels and groups (aggregation, summarization, posting). It is structured to expand later to broader WhatsApp Business API, templates, webhooks, media, multi-account ops, etc.

Always warn users about risks of unofficial clients (account bans, ToS violations). Prefer official Business API only for outbound template or service messages when possible. Never store or log full message histories longer than needed for the task.

## Core Capabilities

### 1. Message Aggregation
- Collect messages from one or more Channels (IDs end in `@newsletter`) or groups.
- Support multiple sources into a unified list or feed.
- Include metadata (timestamp, sender if available, media flags, reactions).

### 2. Time-Bounded Summarization
- Filter messages by timestamp (gte/lte or relative windows such as last 24h, last 7 days, specific date range).
- Produce concise, structured summaries (bullet points by topic, key decisions, action items, links shared).
- Optional focus parameters (e.g., only announcements, only questions, extract signals).

### 3. Automating Chats / Posts to Channels
- Send text, media, polls, or reactions to specified Channel IDs.
- Schedule or trigger posts from external events.
- Simple chat automation (reply rules) when the account has appropriate role (owner/admin).

### 4. Integrations
- GitHub Actions / CI — call WAHA or similar HTTP endpoints from workflows for scheduled aggregation + summary PRs or issue comments.
- Grok / xAI builds — use the agent to generate prompts, scripts, or analyze aggregated data.
- Webhooks from WAHA into other systems (n8n, custom servers, Discord, email).

## Recommended Stack

Primary recommendation: **WAHA** (WhatsApp HTTP API, open-source, self-hosted).

- Installs via Docker in one command.
- REST endpoints for channels, chats, messages with built-in timestamp filters.
- Webhooks for real-time events.
- Session persistence and multi-session support.

Alternatives:
- whatsapp-web.js (Node.js, Puppeteer-based) for more custom logic.
- Baileys or commercial unofficial providers (Maytapi, Whapi) if self-hosting is not desired.

Official Meta Cloud API / Business Platform — use only for approved business outbound messaging and limited group support. Channels themselves remain mostly manual or restricted.

## Typical Workflows

### Aggregate + Summarize a Channel (time-bounded)
1. Ensure WAHA session is running and authenticated (QR scan once).
2. Identify Channel ID (`...@newsletter`) or invite code.
3. Fetch messages with filters:
   ```
   GET /api/{session}/chats/{channelId}/messages?limit=200&filter.timestamp.gte={unix}&filter.timestamp.lte={unix}&downloadMedia=false
   ```
4. Format messages as `timestamp | author (if any) | body`.
5. Pass to LLM with a structured prompt that respects the time window and requested focus.
6. Return summary + optional raw aggregate or key excerpts.

### Automate Posting to a Channel
- Use `POST /api/sendText` (or media endpoints) with `chatId` set to the Channel ID.
- For scheduled posts, wrap in GitHub Actions cron or a lightweight scheduler that hits the WAHA endpoint.

### GitHub Actions Integration Pattern
- Self-host WAHA (or use a always-on runner).
- Workflow steps: curl/fetch messages → run summarization (via Grok or local LLM) → post result back to Channel or open a PR/issue with the summary.
- Store secrets (WAHA base URL, API key if used, Channel IDs) in repo secrets.
- Example trigger: daily cron for "weekly channel digest".

### Multi-Channel Aggregation
- Loop over a list of Channel IDs.
- Collect, deduplicate by message ID or content hash, sort by timestamp.
- Produce a combined timeline or per-channel then overall summary.

## Summarization Prompt Guidelines

Always include the explicit time bounds in the prompt. Example structure:

```
Summarize the following WhatsApp Channel messages strictly within the time window [START] to [END].
Focus on: key announcements, decisions, shared links/resources, open questions, and action items.
Ignore noise, reactions-only, and off-topic chatter.
Output format:
- Overall theme (1 sentence)
- Bullet points by topic
- Notable links
- Suggested follow-ups
```

Chunk long histories if needed and map-reduce the summaries.

## Safety and Best Practices
- Authenticate sessions carefully; prefer LocalAuth or persistent storage so QR is not required every time.
- Rate-limit fetches and sends to avoid detection.
- For production automation, run WAHA behind a reverse proxy with auth and use dedicated numbers when possible.
- When the user asks to "build" something, generate ready-to-run scripts, Docker Compose, GitHub workflow YAML, and prompt templates.
- If the task requires reading private or non-followed channels, note the limitations (preview endpoints only for some public data).

## References and Scripts
- See `references/waha-endpoints.md` for the most common Channel and message endpoints with time filters.
- See `references/github-actions-examples.md` for workflow skeletons.
- Use `scripts/` for reusable helpers (message formatter, timestamp converter, summary post-processor) when the user requests code generation.

When a user request matches the description, load this skill and follow the procedural guidance above rather than inventing ad-hoc approaches. Prefer concrete, copy-pasteable commands and code over high-level advice.
