# pi-anthropic-web-search

[![ci](https://github.com/code-yeongyu/pi-anthropic-web-search/actions/workflows/ci.yml/badge.svg)](https://github.com/code-yeongyu/pi-anthropic-web-search/actions/workflows/ci.yml) [![license: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Anthropic native web search extension for the [pi coding agent](https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent).

This package is the standalone extraction of senpi's former builtin `anthropic-web-search` extension.

## Behavior

The extension does not register a new tool. It intercepts Anthropic requests before they are sent and ensures a native `web_search_*` tool is present for first-party Anthropic `anthropic-messages` payloads.

| Case | Result |
|------|--------|
| API is `anthropic-messages` on `api.anthropic.com` (or `compat.supportsWebSearch: true`) and no native `web_search_*` tool exists | injects `{ type: "web_search_20250305", name: "web_search", max_uses: 8 }` |
| Existing native `web_search_*` tool exists on a supported endpoint | preserves it (no duplication) |
| Function variant named `web_search` is present on a supported endpoint | strips function variant and keeps native variant |
| Anthropic-compatible endpoint without `compat.supportsWebSearch` | strips native `web_search_*` tools; leaves function-tool `web_search` untouched |
| Non-Anthropic API payload | leaves payload unchanged |

`max_uses` is hardcoded to `8`, matching Claude Code/free-code's native web search schema. Optional domain filters can be supplied with comma-separated environment variables:

- `PI_ANTHROPIC_WEB_SEARCH_ALLOWED_DOMAINS`
- `PI_ANTHROPIC_WEB_SEARCH_BLOCKED_DOMAINS`

Set `PI_ANTHROPIC_WEB_SEARCH=0` (or `false` / `off` / `no`) to disable injection on supported endpoints.

It also appends a system-prompt section for supported Anthropic sessions indicating native `web_search` availability.

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
bun install
bun run check
bun test
pi -e ./src/index.ts
```

npm consumers (and the CI smoke job) can use the lockfile instead:

```bash
npm ci
npm test
```

The test suite uses vitest. TypeScript is strict, Node-only, and uses ESM imports with `.js` suffixes. Development targets Bun 1.4.2.

## Origin

Ported from `packages/coding-agent/src/core/extensions/builtin/anthropic-web-search/index.ts` in `code-yeongyu/senpi-mono`.

## License

[MIT](LICENSE).

## Related

- [senpi](https://github.com/code-yeongyu/senpi) — the fork/runtime these extensions are extracted from.
- [Ultraworkers Discord](https://discord.gg/PUwSMR9XNk) — community link from the senpi README.
- [Dori](https://sisyphuslabs.ai) — the product powered by senpi under the hood.
