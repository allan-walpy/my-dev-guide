# branches and pull requests

see also [managing branches with git commands](https://github.com/Kunena/Kunena-Forum/wiki/Create-a-new-branch-with-git-and-manage-branches)

in avoiding direct commits to `master` branches and pull requests take place;
this document covers `git` and `vs code`;

- `git shell` can be accessed with bash;

- `vs code pallette` can be accessed with `ctrl + shift + p` combination;

## branches management

- create local branch;

  > `Git: Create Branch` in vs code pallette;

  ```shell
  git checkout -b {branchName}
  ```

- push to server/publish branch;

  > `Git: Publish Branch` in pallette;

  ```shell
  git push origin {branchName}
  ```

- change current branch;

  > `ctrl + shift + g` in vs code;

  ```shell
  git checkout {branchName}
  ```

- commit to current branch

  > `ctrl + shift + g` in vs code;

  ```shell
  git commit -S -m "#{issueNumber} {commitMessage}"
  ```

- delete local branch

    > `Git: Delete Branch...` in vs code pallette;

    ```shell
    git branch -d {branchName}
    ```

- delete remote deleted from server branches;

  > `ctrl + shift + ~` in vs code; use `git` commands;

  ```shell
  git remote prune origin
  ```

  git branch -a

  > see also: [clean up branches and remotes after merge and delete in github](http://www.fizerkhan.com/blog/posts/Clean-up-your-local-branches-after-merge-and-delete-in-GitHub.html)

## pull requests management

- create pull request on website;

  > pull requests require only [`is::merge` label](./issue_labels.md#type)

- wait until pull request is closed

  > **suggesting** to delete branch after merging and closing pull request
  > on both server and local machine;
