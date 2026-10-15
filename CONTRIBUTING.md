# Contributing

Thanks for helping out. These rules follow the ones used by
[stable-diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp/blob/master/CONTRIBUTING.md),
which this project wraps.

## Before you start

- Search open PRs and issues first so work isn't duplicated.
- For large refactors or changes that affect many parts of the server or the
  WebUI, open an issue to discuss the approach before writing the PR.
- Security problems: do not open a public issue or PR. See [SECURITY.md](SECURITY.md).

## Pull requests

**One clear change per PR.** Do not mix a bug fix with a new feature, or two
unrelated fixes in one PR. Small, focused PRs get reviewed and merged quickly;
large mixed ones are hard to verify and may not be merged. Several commits are
fine when they belong to the same change.

When contributing from a fork, use a personal fork and enable
**Allow edits from maintainers** so small follow-up fixes can be made directly
on your branch.

Every PR description should include:

- What changed and why (short problem / solution summary).
- How you verified it: build command, the requests or steps you ran, and the
  key output. Screenshots for WebUI changes.
- Related issues or PRs, if any.

Do not commit test scripts, experiments, generated output, model weights,
secrets, or local absolute paths.

## Commits

Use Conventional Commit style subjects, optionally scoped, imperative mood:
`fix(queue): ...`, `feat(webui): ...`, `docs: ...`, `refactor: ...`,
`chore: ...`, `perf: ...`.

## AI-assisted contributions

AI tools are welcome; much of this project is written with them. The rules:

- **You are the author and you are responsible.** Review every changed line
  before submitting, and make sure you understand what it does and why.
  You should be able to answer review questions about your PR yourself.
- **Do not list AI tools as co-authors.** Remove `Co-authored-by:` trailers
  for AI assistants from your commits.
- Do not submit code you have not run, or unrelated generated changes that
  came along with the ones you meant to make.

## Building and testing

GPU builds need CUDA (or another sd.cpp backend). See the README and
`docs/DEPENDENCIES-*.md` for platform setup.

```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -DSD_CUDA=ON
cmake --build build -j$(nproc)

# WebUI only (type-check + bundle)
cd webui && npm install && npm run build
```

Run the server with `./build/bin/sdcpp-restapi --config config.json` and
check the parts you changed (REST endpoints, WebUI views, MCP tools).
`npm run build` must pass (`vue-tsc` type check) for WebUI changes.

If you change the download catalog in `data/model_architectures.json`, run
`scripts/check_download_links.py` and include the result.

## Project conventions

`CLAUDE.md` describes the code layout and the step-by-step checklists for
common changes (new endpoint, new load option, new MCP tool, new
architecture). Follow them; new endpoints must be registered through
`ApiRegistry` so they appear in `/openapi.json`, and user-facing changes
should update `docs/API.md`.

## Dependencies

Do not bump the pinned stable-diffusion.cpp commit or other third-party
dependencies in a feature PR. If an update is needed, open an issue first;
dependency upgrades are validated and done by the maintainer.
