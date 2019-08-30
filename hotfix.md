# Hotfixes

Sometimes versions has flows/bugs, which make app almost or completely broken. So, instead of
waiting when iteration ends and fixes the issue, `hotfix` can be applied

- **Must** be open issue with [label `is::hotfix`](./issue_labels.md#Label-Type), assigned to
current iteration and with
[`priority::red` or `priority::critical` label](./issue_labels.md#Label-Priority);

- **Must** be created branch for this issue with name
`{username}-issue{HotfixIsssueNumber}-hotfix-dev`

  > **NB!** instead of normal behavior - branching from `master` (original branch), branch for
  > hotfix based on release version branch `release` - which retain last release version of app

- When hotfix done, you **must** create pull request directly to release version `release` branch

  > **NB!** As you intent to commit in release version only branch, with issue fix, you pull
  > request must contain change in version patch number

- Hotfix pull request **must** be treated with more attention and accuracity

  > It is **advised** to have more than one reviewers and assignees

- There are no limits about how many hotfixes there could be