# ops-claude

Portable skills and plugins marketplace for **Claude Code**, **Grok Build**, and other coding agents.

Focused on operations, automation, messaging channels (WhatsApp, etc.), and cross-harness workflows.

## Add this marketplace

### Claude Code

```bash
/plugin marketplace add ZagaUS/ops-claude
```

### Grok Build

```bash
grok plugin marketplace add ZagaUS/ops-claude
# or
grok plugin marketplace add https://github.com/ZagaUS/ops-claude.git
```

Then install the plugin:

```bash
# Claude Code
/plugin install whatsapp@ops-claude

# Grok Build
grok plugin install whatsapp --trust
```

## Current plugins

| Plugin     | Description                                                                 | Version |
|------------|-----------------------------------------------------------------------------|---------|
| **whatsapp** | WhatsApp operations — Channels/groups aggregation, time-bounded summarization, posting, bots, and CI integrations. Portable across Claude Code & Grok Build. | 0.1.0   |

## Structure

```
ops-claude/
├── .claude-plugin/
│   └── marketplace.json
├── plugins/
│   └── whatsapp/
│       ├── .claude-plugin/
│       │   └── plugin.json
│       └── skills/
│           └── whatsapp/
│               ├── SKILL.md
│               └── references/
├── README.md
└── ...
```

## Contributing

1. Add a new plugin under `plugins/<plugin-name>/`
2. Include `.claude-plugin/plugin.json`
3. Add skills, commands, agents, etc. as needed
4. Update `.claude-plugin/marketplace.json` to list the new plugin
5. Open a PR

## License

MIT (or update as needed)
