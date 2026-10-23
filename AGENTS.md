# romanbediner-preview - agent notes

<!-- ci-standard:begin -->
## CI/CD standard (Roman, 2026-10-23). Binding.

Read `CI-STANDARD.md` at the repository root before you add or change anything under `.github/workflows/`.

- Workflow changes go through a pull request. Never `gh pr merge --admin`, and never push a workflow change straight to the default branch.
- The required check `ci-standard` lints the workflow files your pull request changes. Its name is frozen; do not rename it.
- Before you push, run `python3 .github/ci-standard/ci-cost-lint.py --changed`. Exit 1 means fix it first.
- The six rules that matter most: push triggers only on the default or a release branch with a `paths:` filter (CI001); a
  `concurrency:` group on every push and pull_request workflow (CI002); a job-level `if:` on comment, issue, workflow_run and
  repository_dispatch jobs (CI003); `timeout-minutes` on every job (CI004); no schedule more often than hourly (CI007); no second
  run of the same check on the merge push (CI008).
- Push once when ready, batch merges, test only what you touch, and put a cost line (Actions minutes, API calls, model) in every report.
- `CI-STANDARD.md` and the files under `.github/ci-standard/` (except `config.json`) are byte copies from `rbediner/automation-estate`.
  Do not edit them here; change the canonical copy there and the installer opens the pull requests.
<!-- ci-standard:end -->
