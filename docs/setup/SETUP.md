# First-time setup

The docs in this folder were written in claude.ai. The steps below are for a Claude Code session started in this folder (`~/Projects/goals-to-actions`). Tick them off as you go.

## 1. Main repo

- [x] `git init`, first commit of everything here ("Add product docs and ADRs").
- [ ] Create the GitHub repo `goals-to-actions` (private is fine) and push: `gh repo create goals-to-actions --private --source=. --push`

## 2. Front-end repo (`web-frontend/`)

- [x] `npm create vite@latest web-frontend -- --template react-ts`
- [x] Add PWA support: `npm i -D vite-plugin-pwa`, configure manifest + service worker in `vite.config.ts`.
- [x] Move `docs/setup/web-CLAUDE.md` to `web-frontend/CLAUDE.md`; create `src/config.ts` with `APP_NAME = "Goals to Actions"` and the folder structure it describes.
- [x] Add Vitest; add a short `web-frontend/README.md` (install, dev, build, test).
- [ ] Inside `web-frontend/`: `git init`, first commit, `gh repo create goals-to-actions-web-frontend --private --source=. --push`
- [x] Confirm `git status` in the main folder doesn't show `web-frontend/`.

## 3. Work tracking

- [ ] Create a GitHub Projects board "Goals to Actions" (Todo / In progress / Done) and link both repos.
- [x] Copy `.github/ISSUE_TEMPLATE/` into `web-frontend/` too, so issues there use the same template.
- [ ] Add the first click-through issues (one per screen: Today, Week, Goals, Goal detail, Add, Settings, plus mock data and PWA install).

## 4. Later in stage 4

- [ ] `api/` (`goals-to-actions-api`): FastAPI + Postgres, own CLAUDE.md, OpenAPI type generation into `web-frontend/` (tracker task t410).
