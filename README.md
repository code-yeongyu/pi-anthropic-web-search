# pi-anthropic-web-search

[![ci](https://github.com/code-yeongyu/pi-anthropic-web-search/actions/workflows/ci.yml/badge.svg)](https://github.com/code-yeongyu/pi-anthropic-web-search/actions/workflows/ci.yml) [![license: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Anthropic native web search extension for the [pi coding agent](https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent).

This package is the standalone extraction of senpi's former builtin `anthropic-web-search` extension.

## Behavior

The extension does not register a new tool. It intercepts Anthropic requests before they are sent and ensures a native `web_search_*` tool is present for `anthropic-messages` payloads.

| Case | Result |
|------|--------|
| API is `anthropic-messages` and no native `web_search_*` tool exists | injects `{ type: "web_search_20250305", name: "web_search", max_uses: 8 }` |
| Existing native `web_search_*` tool exists | preserves it (no duplication) |
| Function variant named `web_search` is present | strips function variant and keeps native variant |
| Non-Anthropic API payload | leaves payload unchanged |

`max_uses` is hardcoded to `8`, matching Claude Code/free-code's native web search schema. Optional domain filters can be supplied with comma-separated environment variables:

- `PI_ANTHROPIC_WEB_SEARCH_ALLOWED_DOMAINS`
- `PI_ANTHROPIC_WEB_SEARCH_BLOCKED_DOMAINS`

It also appends a system-prompt section for Anthropic sessions indicating native `web_search` availability.

## Installation

The package targets the [`pi`](https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent) coding agent. Pi loads extensions from `~/.pi/agent/extensions/`, project `.pi/extensions/`, or via the `--extension` / `-e` CLI flag.

```bash
# From npm (once published)
pi install npm:pi-anthropic-web-search

# From git
pi install git:github.com/code-yeongyu/pi-anthropic-web-search

# Manual placement
git clone https://github.com/code-yeongyu/pi-anthropic-web-search ~/.pi/agent/extensions/pi-anthropic-web-search
cd ~/.pi/agent/extensions/pi-anthropic-web-search && npm install

# Dev / one-shot test
pi -e /path/to/pi-anthropic-web-search/src/index.ts
```

After installation, restart pi or run `/reload` inside an interactive session.

## Development

```bash
npm install
npm run build
npm test
npm run typecheck
npm run check
pi -e ./src/index.ts
```

The test suite uses vitest. TypeScript is strict, Node-only, and uses ESM imports with `.js` suffixes.

## Origin

Ported from `packages/coding-agent/src/core/extensions/builtin/anthropic-web-search/index.ts` in `code-yeongyu/senpi-mono`.

## License

[MIT](LICENSE).
