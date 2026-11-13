# AGENTS.md

GitHub **profile repo** (`afperdomo2/afperdomo2`). Only tracked files are `README.md` and `README.en.md`; `README.md` is what renders at https://github.com/afperdomo2. There is no build, test, lint or codegen tooling — "verification" means checking that the Markdown renders and that every embedded URL works.

## Editing rules

- **Keep both files in sync.** `README.md` is Spanish, `README.en.md` is English. Same sections, same order, same images. Update the language switcher at the top of each file when adding/removing content.
- All content is Markdown + inline HTML; GitHub sanitizes it. Supported and used here: `<picture>`, `<details>`, `<div align="center">`, tables, `<sub>`.
- **Dark/light support is done with `<picture>`**, never with a single `<img>`:
  ```html
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="...theme=github_dark">
    <img src="...theme=default" alt="...">
  </picture>
  ```
- No duplicated tech badges. The stack is one `skillicons.dev` row + seven `<details>` categories; add a technology to the category list instead of appending another badge.

## Never reintroduce these

- `github-readme-stats.vercel.app` — returns **503 `DEPLOYMENT_PAUSED`**; this is why the stats cards were replaced.
- `github-readme-streak-stats.herokuapp.com` — deprecated Heroku host; use `streak-stats.demolab.com`.
- `github-readme-activity-graph.vercel.app` — returns 402.
- Duplicate or filler badges (jQuery, Markdown, Trello, NPM) — removed on purpose.

Working image services (all verified 200): `capsule-render.vercel.app`, `readme-typing-svg.demolab.com`, `skillicons.dev`, `img.shields.io`, `komarev.com/ghpvc`, `github-profile-summary-cards.vercel.app` (`theme=default|github_dark`), `streak-stats.demolab.com` (`theme=default|github-dark`, `locale=es|en`).

## Verify before committing

```bash
# 1) Every embedded URL must return 200
python3 - <<'EOF' > /tmp/urls.txt
import re
urls = set()
for f in ['README.md', 'README.en.md']:
    t = open(f).read()
    urls.update(re.findall(r'(?:src|srcset|href)="(https?://[^"]+)"', t))
    urls.update(re.findall(r'\]\((https?://[^)]+)\)', t))
print('\n'.join(sorted(urls)))
EOF
while read -r u; do [ -z "$u" ] && continue; code=$(curl -s -o /dev/null -w "%{http_code}" -L --max-time 30 "$u"); [ "$code" != "200" ] && echo "$code  $u"; done < /tmp/urls.txt

# 2) Render check (GitHub's own GFM renderer)
gh api -X POST /markdown -f mode=gfm -f context=afperdomo2/afperdomo2 -f text="$(cat README.md)" > /tmp/render.html
```

- LinkedIn always returns **999** to `curl`; that is an anti-bot block, not a broken link. Don't "fix" the URL.
- The `/markdown` response is raw HTML — do **not** pipe it through `--jq`, it will fail.

## Facts about this account

- **Personal email is private.** It must never appear in the READMEs or in `mailto:` links (it is already exposed in old commit history). Contact = LinkedIn (`https://www.linkedin.com/in/afperdomo2`) + portfolio (`https://portfolio.afperdomo.cloud/`).
- Commits must use `6942422+afperdomo2@users.noreply.github.com` (already set in the local git config).
- **Private repos 404 for visitors.** Always check before linking a repo:
  `gh api repos/afperdomo2/<repo> --jq .private`
  Known private: `gymflow`, `gymflow_nuxt4`, `optijus_app`, `web-portfolio`, `parti_gasto`. Public alternatives currently featured: `vento_app_monorepo`, `NetSubastasGo`, `opencode-tray`, `dev-kit-manager` (DevStation), `devjobs`, `FitTrackProV2`.
- `gh` is authenticated as `afperdomo2` with `repo` scope but **no `user` scope**, so `gh api -X PATCH user` fails; profile bio/pinned repos must be changed in the GitHub UI.
- Aggregated language stats come from public repos; course repos (2019–2023) skew them, which is why the stack is curated manually instead of generated.

## Content conventions

- Positioning: **Fullstack Engineer · Go · Vue/React · Flutter · IA agéntica · +6 años** (the "+6 años" figure is deliberate; do not "correct" it to the 2016 start date without asking).
- Every skill mentioned must be backed by an actual repo or job experience. The owner has explicitly stated **no hexagonal architecture experience** — don't add it or re-introduce "clean architecture" as a synonym.
- About/experience/certification data lives in the private `web-portfolio` repo (`constants/*.ts`); that is the source of truth for roles, dates and issuers.
