# ADR-002: React with Vite, client-rendered

**Date:** 2026-11-02 · **Status:** Accepted

## Context

The app is personal and behind sign-in, opened from the home screen, and almost all interaction is client state (checking off tasks, moving tasks between days). Search indexing and first-visit load speed don't matter.

## Decision

React with Vite, rendered on the client, with no server-side rendering. PWA support (manifest and service worker) via vite-plugin-pwa, from the click-through onward.

## Consequences

- A natural fit for an installable, offline-capable PWA.
- If public pages (e.g. a landing page) are ever needed, SSR or a static site can be added for those alone.
