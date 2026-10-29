# SyncClipboard server for LazyCat

- Upstream: https://github.com/Jeric-X/SyncClipboard
- Package: `community.lazycat.app.syncclipboard`; version follows stable upstream tags.
- Image: `jericx/syncclipboard-server:v3.3.3`, delivered via `docker.1ms.run` with source/mirror digest checks.
- Target Linux amd64, LazyCat OS 1.5.0 or later.
- Persistent state: `/lzcapp/var/data` mounted at upstream `/app/data`.
- This is the server component; clients must be installed separately. All clients share one instance account.

## Authentication

Native clients cannot perform interactive LazyCat SSO. Only the documented protocol routes bypass that outer login; the upstream service still requires HTTP Basic credentials. Deployment requires a username and a randomly generated secret password. Do not use colons in either credential (upstream parser splits at colons).

The `/help` launcher guide remains behind LazyCat authentication. For authenticated browser requests, request-stage injection supplies Basic Auth, with the complete value base64-encoded and JSON-escaped during template rendering. Requests without a LazyCat identity are not given server credentials and must present their own Basic Auth.

No file-chooser interception is bundled, as requested for the community store. Official publication is disabled.

## Verification and release

GitHub Actions tests the exact pinned upstream image: anonymous and incorrect-password requests must receive 401; correct credentials must permit version retrieval, text/file round trips, and persistence over restart. Initial community publication remains disabled until those checks and the LPK build pass.

No LazyCat device or desktop/mobile client integration test has been performed; SignalR connectivity through a LazyCat gateway and automatic browser authorization require device-side verification. The checks must not be presented as a complete end-to-end client test.

Existing organization secrets `APPSTORE_URL`, `APPSTORE_TOKEN`, and optional `PRIVATE_STORE_GROUP_CODES` are referenced by name only. Never commit credential values.
