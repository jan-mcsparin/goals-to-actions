# ADR-004: Today opens instantly from data stored on the phone

**Date:** 2026-11-09 · **Status:** Accepted (requirement)

## Context

The app is used mostly on the phone, often many times a day, and the Today screen is the main thing opened. Any wait on the network when opening it is friction that works against the core goal. A PWA can feel as fast as a native app, but only if the network is kept out of the way on open.

## Decision

Today must open instantly, without waiting on the network:

- **App shell from cache.** The service worker caches the app's code, so opening it downloads nothing.
- **Last-known plan stored on the device** (e.g. IndexedDB). Today renders immediately from it, then refreshes from the API in the background and updates if anything changed.
- **Optimistic check-offs.** Tapping a task updates the screen immediately and syncs in the background. Offline changes are queued and sent the next time the app opens (Safari has no background sync).
- **Remember where you were.** iOS may restart a backgrounded PWA, so the current screen and view toggle are persisted and restored.

## Consequences

- Built in from the click-through onward, not retrofitted.
- The server stays the source of truth; the device keeps a cache plus a queue of pending changes. Conflicts are rare with one user, but the sync design should handle a change made on the phone and the laptop at once (e.g. last write wins per task).
- Remaining difference from native: no home-screen or lock-screen widget showing today's list without opening the app.
