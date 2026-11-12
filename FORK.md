# House fork of Modern Web Guidance

Upstream: https://github.com/GoogleChrome/modern-web-guidance (Apache-2.0, Google Chrome). Forked at v1.0.0 (2026-11-10).

Used by our client-app template as a Claude Code plugin:

```json
// .claude/settings.json in the app repo
{
  "extraKnownMarketplaces": {
    "house-skills": { "source": { "source": "github", "repo": "ikztnihc/modern-web-guidance" } }
  },
  "enabledPlugins": { "modern-web-guidance@house-skills": true }
}
```

## What we changed (keep this list current)

| File | Change | Why |
|---|---|---|
| `skills/modern-web-guidance/SKILL.md` | Rewritten. Adds Rule 0 (shadcn/ui always wins), an iPhone-aware browser policy, local-file search instead of the `npx` CLI, a narrower trigger, one-line reporting | Upstream guides tell agents to replace components with native `<dialog>`, `<select>` etc. Upstream CLI downloads from npm, sends telemetry and is blocked in our cloud agent threads |
| `skills/modern-web-guidance/INDEX.md` | New, generated | Lets agents grep a list of all guides without the CLI |
| `scripts/build-index.mjs` | New | Regenerates `INDEX.md`; holds the list of shadcn-owned guide ids |
| `skills/chrome-extensions/` | Deleted (and removed from `kimi.plugin.json`) | Not our use case; its trigger matches "service worker", which our PWAs use |
| `.claude-plugin/marketplace.json`, `plugin.json` | Marketplace renamed `house-skills`, version `1.0.0-house.N` | Avoids clashing with Google's own marketplace |

The guide files under `skills/modern-web-guidance/guides/` are untouched, so upstream updates merge cleanly.

## Syncing upstream (monthly, or when Google ships a notable release)

1. On GitHub, open the fork and press **Sync fork**, or locally: `git fetch https://github.com/GoogleChrome/modern-web-guidance main && git merge FETCH_HEAD`.
2. Conflicts: keep **our** `SKILL.md`, `FORK.md`, manifests; for `skills/chrome-extensions/` keep it deleted (`git rm -r`).
3. Run `node scripts/build-index.mjs`. If it reports missing shadcn ids, a guide was renamed: update `SHADCN_OWNED`.
4. Skim upstream's new `SKILL.md` for workflow changes worth copying (not its CLI steps).
5. Bump `1.0.0-house.N` in both manifests, commit, push to `main`.
