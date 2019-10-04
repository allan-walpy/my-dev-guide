# Branches and Pull Requests

> see also:
>
> - [managing branches with git commands](https://github.com/Kunena/Kunena-Forum/wiki/Create-a-new-branch-with-git-and-manage-branches)
> - [managing branches in visual studio](https://docs.microsoft.com/ru-ru/azure/devops/repos/git/branches?view=vsts&tabs=visual-studio)

It is forbidden for anyone to directly commit to `master` branch, so if you want commit, you need
to create a pull request

- create localy branch `{username}-issue{issueNumber}-dev`

  > **NB!** if there isn't any issue conserning you branch or you cover several issues:
  > you may put any words in place of `issue{issueNumber}` reflecting type of your branch

  - git shell

    ```shell
    git checkout -b {username}-issue{issueNumber}-dev
    ```

  - vs code: open Comman Palette `Ctrl + Shift + p`, find `Git: Create Branch`

- commit to that branch

  - git shell

    ```shell
    git commit -m "#{issueNumber} doing staff"
    ```

  - vs code: go to Source Control `Ctrl + Shift + G`

- publish branch (at any point of development)

  - git shell

    ```shell
    git push origin {username}-issue{issueNumber}-dev
    ```

  - vs code: open Comman Palette `Ctrl + Shift + p`, find `Git: Publish Branch`

- create pull request on website

  - merge as special issue requred only [`a merge` label](./issue_labels.md#Label-Type) and any of
    [scope types labels](./issue_labels.md#Label-Scope)

- wait until pull request is closed

  Pull request can't be merged if failing one or more pull request checks and requiments.
  Although, commitier is not oblige to fix that - it can be or reviewer or assignies

  > Please, **do** delete branch after merge closure on local and remote server