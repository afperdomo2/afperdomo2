# AGENTS.md

Repo de **perfil de GitHub** (`afperdomo2/afperdomo2`). Los únicos archivos versionados son `README.md` y `README.en.md`; `README.md` es lo que se renderiza en https://github.com/afperdomo2. No hay build, tests, lint ni codegen — "verificar" significa comprobar que el Markdown renderiza y que cada URL embebida funciona.

## Commits

- **No crear commits.** Al terminar los cambios, deja los archivos modificados en el working tree y reporta qué cambió; el dueño del repo revisa y hace el commit. No ejecutes `git commit`, `git push`, `git commit --amend` ni `git rebase` salvo que el dueño lo pida explícitamente en ese momento.

## Reglas de edición

- **Mantén ambos archivos sincronizados.** `README.md` está en español, `README.en.md` en inglés. Mismas secciones, mismo orden, mismas imágenes. Actualiza el selector de idioma al inicio de cada archivo cuando agregues o quites contenido.
- Todo el contenido es Markdown + HTML inline; GitHub lo sanitiza. Soportado y en uso: `<picture>`, `<details>`, `<div align="center">`, tablas y `<sub>`.
- **El modo claro/oscuro se hace con `<picture>`**, nunca con un `<img>` suelto:
  ```html
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="...theme=github_dark">
    <img src="...theme=default" alt="...">
  </picture>
  ```
- Sin badges de tecnología duplicados. El stack es una fila de `skillicons.dev` + siete categorías en `<details>`; agrega la tecnología a la lista de su categoría en vez de añadir otro badge.

## No reintroducir

- `github-readme-stats.vercel.app` — devuelve **503 `DEPLOYMENT_PAUSED`**; por eso se reemplazaron las tarjetas de estadísticas.
- `github-readme-streak-stats.herokuapp.com` — host de Heroku deprecado; usa `streak-stats.demolab.com`.
- `github-readme-activity-graph.vercel.app` — devuelve 402.
- Badges duplicados o de relleno (jQuery, Markdown, Trello, NPM) — se quitaron a propósito.

Servicios de imágenes que funcionan (los 7 verificados con 200): `capsule-render.vercel.app`, `readme-typing-svg.demolab.com`, `skillicons.dev`, `img.shields.io`, `komarev.com/ghpvc`, `github-profile-summary-cards.vercel.app` (`theme=default|github_dark`), `streak-stats.demolab.com` (`theme=default|github-dark`, `locale=es|en`).

## Verificar antes de entregar

```bash
# 1) Cada URL embebida debe responder 200
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

# 2) Verificar el render (con el renderizador GFM de GitHub)
gh api -X POST /markdown -f mode=gfm -f context=afperdomo2/afperdomo2 -f text="$(cat README.md)" > /tmp/render.html
```

- LinkedIn siempre devuelve **999** a `curl`; es un bloqueo anti-bot, no un enlace roto. No "arregles" la URL.
- La respuesta de `/markdown` es HTML crudo — **no** la pases por `--jq`, va a fallar.

## Datos de la cuenta

- **El email personal es privado.** No debe aparecer en los READMEs ni en enlaces `mailto:` (ya está expuesto en el historial antiguo de commits). Contacto = LinkedIn (`https://www.linkedin.com/in/afperdomo2`) + portafolio (`https://portfolio.afperdomo.cloud/`).
- Los commits usan `6942422+afperdomo2@users.noreply.github.com` (ya configurado en el git local); no lo cambies a un correo personal.
- **Los repos privados dan 404 a los visitantes.** Verifica siempre antes de enlazar un repo:
  `gh api repos/afperdomo2/<repo> --jq .private`
  Privados conocidos: `gymflow`, `gymflow_nuxt4`, `optijus_app`, `web-portfolio`, `parti_gasto`. Alternativas públicas que están destacadas: `vento_app_monorepo`, `NetSubastasGo`, `opencode-tray`, `dev-kit-manager` (DevStation), `devjobs`, `FitTrackProV2`.
- `gh` está autenticado como `afperdomo2` con scope `repo` pero **sin scope `user`**, así que `gh api -X PATCH user` falla; la bio y los repos fijados se cambian en la UI de GitHub.
- Las estadísticas de lenguajes se agregan desde los repos públicos; los repos de cursos (2019–2023) las distorsionan, por eso el stack se cura a mano en lugar de generarse.

## Convenciones de contenido

- Posicionamiento: **Fullstack Engineer · Go · Vue/React · Flutter · IA agéntica · +6 años** (la cifra "+6 años" es deliberada; no la "corrijas" a la fecha de inicio de 2016 sin preguntar).
- Cada habilidad mencionada debe estar respaldada por un repo real o por experiencia laboral. El dueño declaró explícitamente que **no tiene experiencia en arquitectura hexagonal** — no la agregues ni reintroduzcas "clean architecture" como sinónimo.
- Los datos de about/experiencia/certificaciones viven en el repo privado `web-portfolio` (`constants/*.ts`); esa es la fuente de verdad para roles, fechas y emisores.
