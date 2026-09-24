# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.1] - 2026-09-24

### Fixed

- Gate native Anthropic `web_search` injection to first-party `api.anthropic.com` (or an explicit `compat.supportsWebSearch` opt-in). Strip hook-injected native `web_search_*` tools on Anthropic-compatible endpoints that reject replayed server-tool blocks.

### Changed

- Migrate peer dependencies and imports from `@mariozechner/pi-*` to `@earendil-works/pi-ai` and `@earendil-works/pi-coding-agent`.
- Pin development toolchain: `@biomejs/biome` 2.5.14, `vitest` 5.0.1, `typescript` 7.0.2, `@types/node` 26.6.2, `@typescript/native-preview` 7.0.0-dev.20260707.2.
- Add `@earendil-works/pi-ai` and `@earendil-works/pi-coding-agent` 0.87.1 as exact devDependencies.
- Require Node.js `>=22.19.0`.
- Add Bun 1.4.2 GitHub Actions CI (ubuntu/macOS × Node 22/24) plus an `npm ci` consumer job.

## [0.1.0] - 2026-05-07

### Added

- Initial pi coding-agent extension that mirrors senpi builtin `anthropic-web-search` and injects Anthropic native `web_search_*` tools when appropriate.
