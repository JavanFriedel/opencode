# Open Code Neo — Fork Guide

This fork keeps upstream contribution work and Neo customizations separate.

## Branches

- `dev`: clean mirror of `upstream/dev`. Do not commit here.
- `neo`: main Neo development branch. All customizations live here.
- `feature/<slug>`: Neo-specific topic branches based on `neo`.
- `pr/<description>`: upstream-ready topic branches based on `dev`.

## Workflow

1. Sync `dev` from `upstream/dev`.
2. Merge upstream changes into `neo`: `git checkout neo && git merge dev`.
3. Start Neo features from `feature/<slug>` branched off `neo`.
4. Start upstream-capable work from `pr/<description>` branched off `dev`.
5. Never branch upstream PR work from `neo`.

## Remotes

- `upstream`: `git@github.com:anomalyco/opencode.git`
- `origin`: `git@github.com:JavanFriedel/opencode-neo.git`

## Common Commands

Sync the clean upstream mirror:

```bash
git switch dev
git fetch --all --prune
git merge --ff-only upstream/dev
git push origin dev
```

Merge upstream into Neo:

```bash
git switch neo
git merge dev
git push origin neo
```

Start an upstream contribution:

```bash
git switch dev
git switch -c pr/<description>
```

Start a Neo feature:

```bash
git switch neo
git switch -c feature/<slug>
```

## Repo Map

- `packages/opencode`: core CLI, server, TUI, and most application logic
- `packages/app`: shared web UI
- `packages/desktop`: Electron desktop wrapper around the app
- `packages/llm`: model/provider integration code
- `packages/docs`: docs site sources
- `packages/sdk`: generated SDKs and related build scripts
- `packages/console`: console app assets and UI pieces
- `sdks/vscode`: VS Code extension
- `script`: repo-level scripts and generators

## Where To Look

- CLI and runtime entry points: `packages/opencode/src/index.ts`, `packages/opencode/src/cli/`
- TUI: `packages/opencode/src/cli/cmd/tui/`
- Server routes and APIs: `packages/opencode/src/server/`
- Shared app UI: `packages/app/src/`
- Desktop app: `packages/desktop/`
- LLM providers and request shaping: `packages/llm/`
- Config and project bootstrapping: `packages/opencode/src/config/`, `packages/opencode/src/project/`

## Change Impact Guide

- If you change API or server shapes under `packages/opencode/src/server/`, run `./script/generate.ts`.
- If you change the JavaScript SDK, regenerate it with `./packages/sdk/js/script/build.ts`.
- Run tests and type checks from package directories, not from the repo root.
- Run `bun typecheck` from the affected package directory.

## Upstream Contribution Guardrails

- Read `CONTRIBUTING.md` before opening a PR.
- Upstream requires an issue before the PR.
- Keep PRs small and focused.
- Use conventional commit style for PR titles and commits.
- For UI changes, include screenshots or video.
- For logic changes, record how you verified the behavior.

## Style And Local Guidance

- Follow the root `AGENTS.md` style guidance.
- Check package-level `AGENTS.md` files before changing code in that package.
- `packages/opencode/AGENTS.md` contains important rules for Effect code, module layout, migrations, and service patterns.

## Personal Policy

- Put fork-only documentation and local workflow notes under `docs/personal/`.
- If upstream rejects a change you still want, move it to a `feature/<slug>` branch.
