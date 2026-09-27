# Stage 2: Factory Scaffold Complete

**Date:** 2026-09-27  
**Status:** ✅ Scaffolded and committed

## What Was Set Up

- **Factory repo:** `mm-mazhar/software-factory` (your account)
- **App repo:** `mm-mazhar/cli` (forked from httpie/cli)
- **Agent:** Claude Code (subscription sign-in)
- **Model:** Haiku (budget-conscious)
- **Effort:** Low (minimal reasoning for simple fixes)
- **Workers:** 1 (light usage)
- **Allowed triggers:** Only `mm-mazhar` (you)

## Configuration

See `factory.config.json`:
- Setup command: `make all` (creates venv + installs dependencies)
- Check commands: `make codestyle && make test`
- Default branch: `master`

## Next Steps: Stage 3 (Accounts & Secrets)

1. **Copy `.env.example` to `.env`:**
   ```bash
   cp .env.example .env
   ```

2. **Create Upstash Box API Key:**
   - Go to https://console.upstash.com/box
   - Create a new API key
   - Add to `.env` as `UPSTASH_BOX_API_KEY`

3. **Create GitHub Fine-Grained Token:**
   - Go to https://github.com/settings/tokens
   - Create new (beta) fine-grained personal access token
   - Resource owner: `mm-mazhar`
   - Repository access: `mm-mazhar/software-factory` + `mm-mazhar/cli`
   - Permissions: Contents (R/W), Pull requests (R/W), Issues (R/W)
   - Add to `.env` as `FACTORY_GITHUB_TOKEN`

4. **Get Claude Code OAuth Token:**
   - Open a real terminal (not this cloud one)
   - Run: `claude setup-token`
   - It will open a browser sign-in
   - Copy the token and add to `.env` as `CLAUDE_CODE_OAUTH_TOKEN`

5. **Verify setup:**
   ```bash
   node --env-file=.env scripts/check-setup.mjs
   ```

## Known Issues / To Verify

- **Python environment:** Stock Upstash Box has Python 3.11 but may need setup fixes
  - Will test in Stage 4 (smoke test)
  - If `make all` fails, we'll build a custom worker image

## Notes

- `.env` is in `.gitignore` — never commit it
- `factory.config.json` is committed (contains no secrets)
- All scripts are templated and ready to use
