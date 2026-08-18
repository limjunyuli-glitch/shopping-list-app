# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A single-file shopping list web app (`index.html`) — vanilla HTML/CSS/JS, no build step, no dependencies, no package manager. All markup, styles, and logic live in the one file.

## Running

There is no build/lint/test tooling. Open `index.html` directly in a browser, or serve the directory with any static file server (e.g. `npx serve .`) to test.

## Architecture

- State is a single in-memory `items` array of `{ id, text, checked, created_at }`, persisted to a Supabase Postgres table (`shopping_items`) via the `@supabase/supabase-js` client (loaded from CDN, no build step). `loadItems()` fetches all rows on startup.
- All mutations (`addItem`, `toggleItem`, `deleteItem`, `clearChecked`) update the local `items` array, call `render()`, and issue the corresponding Supabase insert/update/delete against `shopping_items`.
- The Supabase project URL and publishable (anon) key are hardcoded in `index.html`. The `shopping_items` table has RLS enabled with an open "allow all" policy (no auth in this app), so treat the anon key as public and don't put sensitive data in this table.
- `render()` fully rebuilds the `<ul id="list">` DOM from `items` on every change (no diffing/virtual DOM) — it also toggles the empty-state message and updates the item count label.
- Dark mode is handled purely via CSS custom properties in `:root` overridden under `@media (prefers-color-scheme: dark)`; there is no JS-driven theme toggle.
- UI text/labels are in Korean.
