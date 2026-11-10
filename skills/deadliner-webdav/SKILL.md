---
name: deadliner-webdav
description: Configure and run Deadliner CLI WebDAV synchronization and diagnose synchronization failures. Use when the user requests syncing Deadliner tasks with a WebDAV account or another Deadliner client.
---

# Deadliner WebDAV

Use `--json` on all Agent calls to avoid terminal-dependent human formatting. Run `deadliner --json schema` and `deadliner --json sync status` with the intended `--data-dir`. Status only reports whether a URL is supplied; it does not test authentication or server reachability.

Configuration is per-process environment:
- `DEADLINER_WEBDAV_URL`: HTTP(S) base collection URL, without embedded credentials, query or fragment. Prefer HTTPS for remote accounts.
- `DEADLINER_WEBDAV_USERNAME` and `DEADLINER_WEBDAV_PASSWORD`: credentials from the user's approved secret source; never print them or put them in task notes, command arguments or committed files.
- `DEADLINER_WEBDAV_TIMEOUT_MS`: optional 1..300000, default 15000 per request.

`deadliner sync run` performs a two-way merge and upload. It can change both the local store and the remote account. Run it when the user has requested this operation; do not treat a request to inspect configuration as permission to sync.

The CLI directly invokes DeadlinerCore V3 sync at `Deadliner/kmp-changelog-v1.json`, including snapshots and change logs for Core entities. The remote root and protocol must match the other clients. It does not automatically migrate legacy V2 files or selectively sync only tasks. Do not hand-edit remote JSON, strip entities, delete snapshots, or use unconditional uploads to fix conflicts.

Read the JSON result and exit status. Exit 5 is sync failure: local edits remain available; a pull may already have been applied before a failed upload. Core retries one ETag precondition conflict internally. Investigate credentials/URL/connectivity and use a bounded retry after a transient failure is resolved; avoid endless retries or claiming success. Exit 4 BUSY means another CLI process owns this profile; wait for it to finish rather than removing the lock file.

For a reproducible diagnostic, use two temporary data directories and an isolated WebDAV test collection. Do not run destructive tests against the user's normal account. After successful sync, query the relevant task UID and report what is confirmed; a local fixture test alone does not prove a particular production server is compatible.
