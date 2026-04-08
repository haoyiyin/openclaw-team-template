# Security Notes

This repository is intentionally published as a sanitized template.

## Never commit

- real API keys or tokens
- bot credentials
- device identity or pairing data
- runtime databases
- memory stores
- logs or session transcripts
- local `.env` files
- production config snapshots

## Recommended release process

Before every public push, scan for:

- `sk-`
- `BEGIN PRIVATE KEY`
- `apiKey`
- `token`
- `secret`
- `.env`
- session or log exports

## Scope of this repo

This repo is meant to share workflow structure, not private runtime state.

If a secret is ever committed by mistake:

1. rotate it immediately
2. remove it from the repository history
3. audit related credentials and tokens
