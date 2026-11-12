# fork-ci sandbox

End-to-end test of a proposed change to
[apache/infrastructure-actions `fork-ci`](https://github.com/apache/infrastructure-actions/issues/1517):
list fork jobs that did not succeed in the upstream check.

This repository plays the upstream. A fork runs `ci.yml`; `fork-ci-status.yml`
mirrors that run onto the PR. Pushing a `.fail` file makes some fork jobs fail.
