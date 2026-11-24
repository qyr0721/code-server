# code-server (fork)

This is a fork of [coder/code-server](https://github.com/coder/code-server).
It is based on **code-server 4.146.0** (VS Code 1.146.0).

## What is changed

The goal of this fork is to make GitHub sign-in shared across all devices that
connect to the same code-server instance, instead of being stored separately in
each browser.

- **Server-side shared secret storage** (`src/node/routes/index.ts`): three
  authenticated endpoints — `GET /__cs_secret`, `POST /__cs_secret`, and
  `GET /__cs_secret_keys` — persist VS Code secrets to a JSON file under the
  user data dir (`shared-secrets.json`) instead of the browser.
- **Shared GitHub auth in the workbench** (`patches/shared-github-auth.diff`):
  `BaseSecretStorageService.get/set/delete/keys` now read and write through
  those endpoints. The patch is applied with the project's standard `quilt`
  patch series (appended to `patches/series`).
- **Effect**: sign in with GitHub on one device and every other device sharing
  this server uses the same account; signing out on one device signs out
  everywhere. This also covers the GitHub Accounts panel used by Git/PR and
  Copilot sessions.

Everything else is identical to upstream. To sync with upstream, merge the new
upstream tag and rebase the two changes above.
