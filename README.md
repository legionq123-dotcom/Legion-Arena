# ЛЕГИОН АРЕНА — v24.38

Cross-platform MOBA prototype source (Android / Windows PC / iOS target direction). This archive contains the Node.js browser prototype, authoritative match simulation modules, matchmaking and server registry scaffolding, SQL persistence schema, and automated tests.

## v24.38 — stale lobby recovery

- Added `src/lobby-recovery-worker.mjs`, a stoppable periodic worker with overlapping-run coalescing and retry-after-error behavior.
- Added `LegionPersistence.recoverStaleCreatedLobbies()` which transactionally finds old lobbies still in `created`, locks rows with `SKIP LOCKED`, restores players to matchmaking queue without overwriting any newer queue ticket, marks the lobby `cancelled`, and marks the still-created match `aborted`.
- Defaults are deliberately conservative: lobby must be at least 60 seconds old, and each run processes at most 100 lobbies.
- This recovery only handles stale, not-yet-started lobbies. Active-match snapshot/WAL recovery is a separate path.

## Requirements

- Node.js 20+
- `npm install`
- PostgreSQL is required for durable matchmaking, server registry, and recovery operations. Configure `DATABASE_URL` and the environment variables described in `.env.example` when integrating the persistence bootstrap.

## Run checks

```sh
npm run check
npm test
npm start
```

The local gateway can run in local/demo mode without a durable database; that does not validate PostgreSQL SQL or distributed production behavior. `npm start` serves the browser prototype on port 8080 by default.

## Important status

This is a development prototype/source archive, not a production deployment. A real PostgreSQL instance was not available for integration testing here. Android APK, Windows Setup.exe, and iOS IPA are not produced by this Node.js archive. Native platform packaging and device testing remain separate tasks.
