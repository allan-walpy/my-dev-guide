# Iteration lifecycles

## Iteration initiation

> Iteration **N** - current iteration;
>
> Iteration **N-1** - previous iteration;
>
> Iteration **N+1** - next iteration;
>
> Iteration **Next** - means milestone for issues that hasn't got into milestones, but planned for
> the future;

- Iteration `N-1` **must** be finished;

- New milestone for iteration `N+1` **must** be created;

- Issues **must** be rearrange between iterations `N`, `N+1` and `Next`;

- Issue iterations `N` and `N+1` **must** be prioritized inside/relatively their iterations;

  > Priority labels valid only for issues and pull request that is open and belong to `N` or `N+1`
  > iteration

- Milestone durations for `N` and `N+1` iterations **could** be changed at this point;

## Iteration development

> **NB!** Recommended iteration duration is about 3 weeks (+/- week)

- **No direct commits** to `master` are permited - only pull request are acceptable;

  > Exception **could** be last commit of iteration, changing version

- For each issue contributor **should but not oblige to** use separate branch;

- Every development branch **nust** follow perticilar pattern `{username}-{smth}-dev`:

  - `username` - username (only letters and digits allowed) of whom this branch is

    > if branch uses several people, the one incharge username uses or key word `group`

  - `smth` - topic of your branch; if branch dedicated to issue - use `issue#`, for example
    `issue42`

    > if you prefer only one persistant branch for youself use key word `all`;

### Fetching main branch

> see also
> [pulling from master to dev branch](https://stackoverflow.com/questions/20101994/git-pull-from-master-into-the-development-branch)

- make sure you are on your branch

  > git shell
  >
  > ```shell
  > git checkout {your_branch_name}
  > ```

- load last `master` commits

  > git shell
  >
  > ```shell
  > git fetch origin
  > ```

- merge you branch with `master`

  > git shell
  >
  > ```shell
  >  git merge origin/master
  >  ```

## Iteration finalization

> **NB!** You may face a problem, then local branches won't deletes with remotes one. Use this:
>
> ```git
> # list of bbranches and remotes;
> git branch -a
>
> # see non actual remotes;
> git remote prune origin --dry-run
>
> # delete all non actual remotes;
> git remote prune origin
> ```
>
> also: [clean up branches and remotes after merge and delete in github](http://www.fizerkhan.com/blog/posts/Clean-up-your-local-branches-after-merge-and-delete-in-GitHub.html)

To initiate finalization *(sic!)*:

- all issues and pull request **must** be closed

- all test **must** be passed (except on code style)

  > if you think test are broken - you **must** open issue on current milestone
  > to fix them or reject them from project *(sic!)*; after all no new version
  > can be provided if tests failing;

- change log **must** be written;

  > you **can** look up closed commits and pull request branches in finalizing milestone

- All `{username}-{smth}-dev` **must** be deleted;

  > Exception for this rule **could** be `{username}-all-dev` branches;

- New app version **must** be provided;

  - on assining new version **must** use [semantic versioning](https://semver.org)

    Then deciding about pre-release suffix, **try** to follow this:

    - **zero** - app does nothing;

    - **alpha** - app has some coded and may be working features;

    - **beta** - app passed integrational test, comfirming app follow its api specification;

      > **NB!** integrational tests **must** cover all api specification, or you stick with alpha;

    - **rc** `[short for release candidate]`- local and deployed (if any) versions of the app past
      100% covered integrational api tests

    - **stable** `[or no suffix]` - same as `rc`, but must be deployed and used for a someperiod
      of time or/and tested with even more commits;

  - change log **can** give a clue about how huge changes were and that parts of projects they are

- Release of new version **must**:

  - include changelog;
  - provided published self-contained versions of app for every platform that app supports (exeptions for `zero` versions);
  - have deployed version of app on a real server (if any required);

## Hotfixes

Information about hotfixes [here](./hotfix.md);