# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A single-file shopping list web app (`index.html`) — vanilla HTML/CSS/JS, no build step, no dependencies, no package manager. All markup, styles, and logic live in the one file.

## Running

There is no build/lint/test tooling. Open `index.html` directly in a browser, or serve the directory with any static file server (e.g. `npx serve .`) to test.

## Architecture

- State is a single in-memory `items` array of `{ id, text, checked }`, persisted to `localStorage` under the key `shopping-list-items` via `loadItems()` / `saveItems()`.
- All mutations (`addItem`, `toggleItem`, `deleteItem`, `clearChecked`) follow the same pattern: mutate `items`, call `saveItems()`, call `render()`.
- `render()` fully rebuilds the `<ul id="list">` DOM from `items` on every change (no diffing/virtual DOM) — it also toggles the empty-state message and updates the item count label.
- Dark mode is handled purely via CSS custom properties in `:root` overridden under `@media (prefers-color-scheme: dark)`; there is no JS-driven theme toggle.
- UI text/labels are in Korean.
