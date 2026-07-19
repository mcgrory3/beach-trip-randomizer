# Beach Trip Randomizer — fixed preview

**Version ID:** `ce51526`  
**Replaces broken:** `8bfe99a`  
**Live preview:** https://mcgrory3.github.io/beach-trip-randomizer/

## What you should see
- Sky-gradient background
- Two reel-style selectors (Destination + Stay Style)
- Yellow **🎲 SPIN MY TRIP** button
- Spin animation + result card with booking links

## Root cause of blank version-card preview
Kimi version cards serve the app under a **URL subpath**. The original build:

1. Used `BrowserRouter` with (originally) a root-only route → React rendered **nothing** under `/v/<id>/…`
2. Relied on sandbox `localhost:8931`, which is not reachable outside the builder

## Fix
- Render the game at **any path** (no router dependency)
- Vite `base: './'` so asset URLs stay relative under subpaths
- Removed Kimi-only `inspectAttr` plugin from production config
- Preview/server bind `0.0.0.0`

## Verify
```bash
# assets are relative
grep assets index.html   # ./assets/...

# production build
cd app && npm ci && npm run build
npx serve dist
```
