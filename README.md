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

Then install individual plugins with:

```bash
/plugin install <plugin-name>@ops-claude
# or in Grok:
grok plugin install <plugin-name> --trust
```

## Structure

```
ops-claude/
├── .claude-plugin/
│   └── marketplace.json     # Catalog of available plugins
├── plugins/                 # One folder per plugin (coming soon)
├── README.md
└── LICENSE
```

## Current plugins

None yet — this marketplace is being bootstrapped.

The first planned plugin will package the portable **whatsapp-channels** skill for message aggregation, time-bounded summarization, and automation across Claude Code and Grok Build.

## Contributing

1. Add a new plugin under `plugins/<plugin-name>/`
2. Include `.claude-plugin/plugin.json`
3. Add skills, commands, agents, etc. as needed
4. Update `.claude-plugin/marketplace.json` to list the new plugin
5. Open a PR

## License

MIT (or update as needed)
