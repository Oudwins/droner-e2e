# Droner GitHub E2E sandbox — base-update-rebase-live-20260912

A small, independent repository for testing Droner's integration with real
GitHub branches, pull requests, and CI checks.

## Fixture

`fixture.txt` must contain exactly `pass` followed by a newline. The `Fixture`
workflow runs on pull requests and pushes to `main`:

- Set the fixture to `fail` to produce a failing check.
- Restore it to `pass` to produce a successful check.
- Change this README when you need a passing PR with a real commit.

Run the same check locally with Bash:

```bash
diff -u <(printf 'pass\n') fixture.txt
```

## Manual testing

1. Authenticate with `gh auth login` and open this clone as a project in Droner.
2. Create a session on a unique branch, such as `e2e/<run-id>/merge`.
3. Make a change, commit it, and push the session branch.
4. Open a PR with `gh pr create --base main`.
5. Wait for Droner to show the PR and CI status before making the next change.
6. Test closing, reopening, or merging the PR and observe Droner's state.
7. Close any remaining test PRs and delete the branches created by the run.

Keep `main` passing: restore the fixture before merging a test PR. Use separate
branches for scenarios that close without merging or delete a tracked branch.

## Automated testing

The test runner and assertions belong in the Droner repository. Each run should
clone this sandbox into a temporary directory, use uniquely named branches,
start Droner with an isolated data directory and port, and clean up its PRs and
branches. Wait for observed state changes with deadlines instead of fixed sleeps.

Use `git@github.com:Oudwins/droner-e2e.git` as the remote. Locally, Droner can use
the token from `gh auth token`. Cross-repository CI needs a token with access to
this sandbox; the Droner repository's built-in Actions token is not sufficient.
