# iteration lifecycles

| code                 | required | comment            |
| :------------------- | :------: | :----------------- |
| `iteration #[n-1]`   |   yes    | previous if exists |
| `iteration #[n]`     |   yes    | current            |
| `iteration #[n+1]`   |    no    | next               |
| `iteration next`     |    no    | `[>n+1]`           |
| `iteration x [smth]` |    no    | thematic           |

- `iteration next` is for issues not assigned to `n`, `n+1` etc. existing iterations;

- `iteration x [smth]` is one and more iteration milestones with some thematic

  > examples:
  >
  > `iteration x v1.0.0` or `iteration x release` - issues relating
  > to first stable release version;
  >
  > `iteration x hosted` - issues relating to deploying app and issues
  >relating to deployed issues;

## iteration initiation

- iteration `n-1` if exists **must** be finished and closed;

  > not finished issues/pull requests not can be transferred to next milestone -
  > code conserving these issues must not be present in result iteration code;

- new milestone for iteration `n` **must** be present;

  > new milestone creation depends on how many iteration ahead are planned already;
  >
  > example 1: if you have [2, 3, 4] opened, on closing `iteration 2`
  > the `iteration 5` must be created;
  >
  > example2: if you have `iteration 3` only, on closing `iteration 3`
  >the `iteration 4` must be created;

- issues **must** be assigned to created iteration,
  presumably reassigned from `iteration next` and `iteration x [smth]` if any;

- inside `iterations #[x]` milestones, issues **must** be prioritized
  relatively other issues in same milestones they are;

  > [priority labels](./issue_labels.md#priority) assigned only
  > for issues and pull request inside `iteration #[x]` milestones;

- milestones durations for `iteration #[x]` **could** be changed at this point;

  > other milestones' duration can be changed at any time;
  >
  > except: `iteration next` must not have end date, and start date
  > must be from last existing `iteration #[x]` end date;

## iteration development

> **suggested** iteration duration is about 3 weeks (+/- a week),
> depending on size of team and importance of project(s);

- **no direct commits** to `master` are permited - only pull request are acceptable;

  > exceptions:
  >
  > - last commit of iteration, changing version;
  >
  > - newly created repository for one/two its first iterations;

- for each issue contributor **should but not oblige to** use separate branch;

  - branch name **must follow pattern** `{username}-{smth}-dev`

    > **not recommended**: in some exceptions one contributor may have
    > their personal branch for contributing `{username}-all-dev`

    - `username` - branch contributor username (only `[A-z0-9]` allowed)
      > if several people uses branch, use prefix `group-main-`;
      >
      > also **suggesting** to add branches for each contributor
      > `group-{username}` in such cases;

    - `smth` - topic of your branch; if branch dedicated to issue - use `issue{issueNumber}{issueTopic}`;

      > example: `issue42AnswerToQuestionOfUniverse`;

> see also: [branches and pull requests](./branches.md)

## iteration finalization

requirements for starting iteration finalization;

- all issues and pull requests **must** be closed or moved to next iteration;

- all test **must** be passed;

  - if no test written/present - iteration **can be finalized**, but
    **no steps** on publishing and making new version **must be** done;

  - if tests are broken - in such case two approaches;

    - **must** open issue on test in current iteration,
      and not closing both, until tests are fixed;

    - **degrade** tests to pass them all with degradation of project,
      pre-release note must be added or existing downgraded;
      after that iteration **can be closed only with pre-release**
      **version and mentioning of downgrading in changelog**;

- changelog for iteration **must** be written;

  > you **can** look up closed issues and pull request in finalizing iteration;

- all `{username}-{smth}-dev` branches **must** be deleted;

  > exception for this rule **could be** `{username}-all-dev` branches
  > and **must be** branches for pull request and issues
  > that has been moved to next iteration;

- new app version **must** be provided; [see versioning](./versioning.md);

  > exception: repositories with no any tests if project implies any present;

  - release of new version **must** contain;

    - changelog;

    - build/compiled files for every platform app supports

      > exceptions: `zero` versions;

    - have deployed version of app on a real server (if any required);

      > exceptions: `zero` and `alpha` versions;

## hotfixes

[see hotfixes](./hotfix.md);
