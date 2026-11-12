# Results, 2026-11-12

Bridge: [`chihsuan/infrastructure-actions/fork-ci@e0627a9`](https://github.com/chihsuan/infrastructure-actions/tree/e0627a9a0decef769f41b730cba478344d874435/fork-ci)
(branch `fork-ci-job-details`, for apache/infrastructure-actions#1517).
Upstream: this repository. Fork: [chihsuan/fork-ci-sandbox](https://github.com/chihsuan/fork-ci-sandbox).
[`ci.yml`](.github/workflows/ci.yml) has a passing job, a matrix with one failing cell that emits `::error`,
a job named ``weird `name` | @example``, a reusable-workflow job that times out, and a skipped job.

## Check on [PR #1](https://github.com/chihsuan-sandbox/fork-ci-sandbox/pull/1)

Fork run [43229743470](https://github.com/chihsuan/fork-ci-sandbox/actions/runs/43229743470) (6 jobs: 2 success, 1 skipped, 2 failure, 1 cancelled).
Check [131221768203](https://github.com/chihsuan-sandbox/fork-ci-sandbox/runs/131221768203):

- The PR's check list shows the title next to the status: `Fork run failure: unit (two) and 2 more`.
- The summary lists the three jobs that did not succeed, with links built from job IDs:

| Job | Result | Failed step | Error |
|---|---|---|---|
| `unit (two)` | `failure` | `Run tests` | `TestExample.testWrite failed: expected 3 but was 2` |
| `nested / inner` | `cancelled` | | `The job has exceeded the maximum execution time of 1m0s` |
| ``weird 'name' \| @example`` | `failure` | `Check` | `Process completed with exit code 1.` |

- The passing and skipped jobs are not listed. The reusable-workflow job is listed by its nested name.
- The backtick in the job name became `'`, `|` is escaped, and `@example` stays inside a code span.

## Found and fixed during the test

With `8b7dcb6`, `unit (two)` showed the runner's generic `Process completed with exit code 1.`
instead of the job's `::error` ([check 131220628863](https://github.com/chihsuan-sandbox/fork-ci-sandbox/runs/131220628863)).
`e0627a9` prefers the job's own failure annotation and ignores warning and notice annotations.

## Bridge runs

| Run | Trigger | Result |
|---|---|---|
| [43229342953](https://github.com/chihsuan-sandbox/fork-ci-sandbox/actions/runs/43229342953) | manual, `8b7dcb6` | 1 written |
| [43229780438](https://github.com/chihsuan-sandbox/fork-ci-sandbox/actions/runs/43229780438) | `workflow_run`, fork run still active | 1 written |
| [43230001996](https://github.com/chihsuan-sandbox/fork-ci-sandbox/actions/runs/43230001996) | manual, run completed | 1 written, jobs listed |
| [43230050018](https://github.com/chihsuan-sandbox/fork-ci-sandbox/actions/runs/43230050018) | manual, nothing changed | 1 written, same content |

The last run still writes because of apache/infrastructure-actions#1512: GitHub stores the check page as
`details_url`, so `publish()` never sees an unchanged check. With #1514 it should write nothing.
Whether that run skipped the jobs API is not visible in the log; unit tests cover it.

The first `workflow_run` bridge run ([43229074809](https://github.com/chihsuan-sandbox/fork-ci-sandbox/actions/runs/43229074809))
failed before any job started, minutes after this organization was created. Later runs of the same file passed.
