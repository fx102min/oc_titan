# Upstream updates for this fork

`Sync upstream` checks `anomalyco/opencode:dev` every day at approximately
09:17 Asia/Seoul (00:17 UTC), after pushes to this fork's `dev`, and when run
manually from GitHub Actions. GitHub can delay scheduled runs.

When upstream has commits that are not in this fork, the workflow opens or reuses
one PR from `anomalyco:dev` into `fx102min/oc_titan:dev`. The upstream branch itself
is the PR source, so new upstream commits appear in the same PR automatically.
There is no bot branch to overwrite, and no personal access token is required.

The workflow checks an exact snapshot of both branches by merging them locally
on Linux and Windows runners. It runs `bun run check` on Linux and affected
package unit tests on both platforms. Browser E2E tests and release builds are
not included. Conflicts fail verification without changing either branch.
The `upstream-sync/checks` commit status links to the results. If either branch
changes while checks are running, the result asks for another run.

Review the changes and a passing status on the current upstream commit, then use
**Create a merge commit**. Squashing or rebasing removes the upstream ancestry
that the next update uses to detect already integrated commits. The automation
never approves, enables auto-merge, or merges a PR, and has no contents-write
permission.

`Fork checks` also checks PRs from branches in this fork. Upstream PRs use the
explicit `Sync upstream` run so checks do not depend on approval of workflows
triggered by a bot-created PR. To check a newer upstream commit immediately, open
**Actions → Sync upstream → Run workflow**, selecting `dev`.

## One-time setup

1. Merge the automation setup PR into this fork's default branch, `dev`.
2. Enable **Sync upstream** and **Fork checks** in this fork's Actions settings.
   Keep the inherited upstream workflows disabled: they use upstream-specific
   runners, publishing secrets, and PR maintenance policies.
   The upstream issue/PR cleanup workflows also have repository guards so they
   cannot close this fork's issues or update PRs if activated by a schedule.
3. Under **Settings → Actions → General → Workflow permissions**, enable
   **Allow GitHub Actions to create and approve pull requests**. GitHub combines
   these capabilities in one setting; this workflow only creates PRs.
   The default token permission can remain read-only.
4. Run **Sync upstream** once and inspect its summary and resulting PR, if any.

Schedules only run once the workflow exists on the default branch. GitHub can
disable scheduled workflows after 60 days of repository inactivity; re-enable
the workflow in Actions if that happens.
