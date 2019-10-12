# repository management

## branches and pull requests

see also [managing branches with git commands](https://github.com/Kunena/Kunena-Forum/wiki/Create-a-new-branch-with-git-and-manage-branches)

in avoiding direct commits to `master` branches and pull requests take place;
this document covers `git` and `vs code`;

- `git shell` can be accessed with bash;

- `vs code pallette` can be accessed with `ctrl + shift + p` combination;

### branches management

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

  > see also: [clean up branches and remotes after merge and delete in github](http://www.fizerkhan.com/blog/posts/Clean-up-your-local-branches-after-merge-and-delete-in-GitHub.html)

### pull requests management

- create pull request on website;

  > pull requests require only [`is::merge` label](#type-issue-label)

- wait until pull request is closed

  > **suggested** to delete branch after merging and closing pull request on
  > both server and local machine;

## issue and pull requests labels

> issues code presented here optimized for gitlab and their
> [scope labels](https://gitlab.com/help/user/project/labels.md#scoped-labels);

### type issue label

**single** label is **required** for **any** issues;

> scope `is`; color: `#ccc`;

| code              | remark                       |
| :---------------- | :--------------------------- |
| `is::bug`         | bug and unexpected behavior  |
| `is::feature`     | new functionality            |
| `is::hotfix`      | hotfix for released version  |
| `is::improvement` | improving code & features    |
| `is::merge`       | merge/pull request           |
| `is::question`    | question/discussion/proposal |

### priority issue label

**single** label is **required** for **opened** issues with **iteration #any**
milestone;

> scope `priority`; color: **depend on label**;

| code               | color  | remark   |
| :----------------- | :----: | :------- |
| `priority::green`  | `#4a4` | low      |
| `priority::orange` | `#f80` | medium   |
| `priority::red`    | `#f44` | high     |
| `priority::stop`   | `#000` | blocking |

### scope issue label

**at least one** label is **optional** for **any** issues;

> scope `scope`; color: `#448`; postfix: `::_`;

| code                   | remark                        |
| :--------------------- | :---------------------------- |
| `scope::code::_`       | affects actual program code   |
| `scope::code::arch::_` | affects program architecture  |
| `scope::repo::_`       | affects repository management |
| `scope::test::_`       | affects tests                 |

### stage issue label

**single** label is **required** for **opened** issues with
**iteration #current** milestone;

except: **x** label is **required** for **any** **closed** issues;

> scope `stage`; color: `#48a`, `#000` for `x` stage;

| code                | remark      |
| :------------------ | :---------- |
| `stage::holding`    | on hold     |
| `stage::processing` | in progress |
| `stage::x`          | closed      |

### cause of closure issue label

**at least one** label *including `stage::x` label* is **required**
for **closed** issues;

> scope `stage::x`; color: `#000`;

| code                       | remark                                         |
| :------------------------- | :--------------------------------------------- |
| `stage::x::done`           | merged/resolved/fixed/implemented/done         |
| `stage::x::invalid`        | not an issue                                   |
| `stage::x::invalid::clone` | duplicate                                      |
| `stage::x::rejected`       | declined/won't fix/won't merge/won't implement |

## commit conventions

commits to master branches `master`, `release` and other if any, **must** comply
with [conventional commits](https://www.conventionalcommits.org/en/);
any contradictions, elaborations are described in this section;

commits of `dev` branches **should** start with `#{issueNumber}` and not oblige
to `conventional commits`;

> conventional commit structure;
>
> ```text
> <type>[optional scope]: <description>
>
> [optional body]
>
> [optional footer]
> ```

| commit's | value         |         issue          | comment                  |
| -------: | :------------ | :--------------------: | :----------------------- |
|   `type` | `fix`         |       `is::bug`        | bugfixes;                |
|   `type` | `hotfix`      |      `is::hotfix`      | [hotfixes](#hotfixes);   |
|   `type` | `feat`        |     `is::feature`      | features;                |
|   `type` | `improvement` |   `is::improvement`    | any code improvements;   |
|   `type` | `style`       |   `is::improvement`    | code style;              |
|   `type` | `refactor`    |   `is::improvement`    | code refactors;          |
|   `type` | `chore`       |   `is::improvement`    | random changes;          |
|   `type` | `docs`        |    `scope::docs::_`    | in documentation;        |
|   `type` | `break`       |          none          | alias `BREAKING CHANGE`; |
|  `scope` | `code`        |    `scope::code::_`    | in project code;         |
|  `scope` | none          |    `scope::code::_`    | alias `code`;            |
|  `scope` | `arch`        | `scope::code::arch::_` | in code architecture;    |
|  `scope` | `repo`        |    `scope::repo::_`    | in repository;           |
|  `scope` | `test`        |    `scope::test::_`    | in test code;            |
|  `scope` | other         |     `scope::*::_`      | in other defined scopes; |

> for issue `is::merge` see affected issues in merge/pull request;
>
> changes for issues `is::question` cannot be commited, until issue changes
> type;

## iteration lifecycles

| code                 | required | comment            |
| :------------------- | :------: | :----------------- |
| `iteration #[n-1]`   |   yes    | previous if exists |
| `iteration #[n]`     |   yes    | current            |
| `iteration #[n+1]`   |    no    | next               |
| `iteration next`     |    no    | `[>n+1]`           |
| `iteration x [smth]` |    no    | thematic           |

- `iteration next` is for issues not assigned to `n`, `n+1` etc. existing
  iterations;

- `iteration x [smth]` is one and more iteration milestones with some thematic;

  > examples:
  >
  > `iteration x v1.0.0` or `iteration x release` - issues relating
  > to first stable release version;
  >
  > `iteration x hosted` - issues relating to deploying app and issues
  > relating to deployed issues;

### iteration initiation

- iteration `n-1` if exists **must** be finished and closed;

  > not finished issues/pull requests not can be transferred to next milestone;
  > code conserving these issues must not be present in result iteration code;

- new milestone for iteration `n` **must** be present;

  > new milestone creation depends on how many iteration ahead are planned
  > already;
  >
  > example 1: if you have `2, 3, 4` iterations opened,
  > on closing `iteration 2` the `iteration 5` must be created;
  >
  > example2: if you have `iteration 3` only, on closing `iteration 3`
  > the `iteration 4` must be created;

- issues **must** be assigned to created iteration, presumably reassigned from
  `iteration next` and `iteration x [smth]` if any;

- inside `iterations #[x]` milestones, issues **must** be prioritized relatively
  other issues in same milestones they are;

  > [priority labels](#priority-issue-label) assigned only for issues and pull
  > request inside `iteration #[x]` milestones;

- milestones durations for `iteration #[x]` **could** be changed at this point;

  > other milestones' duration can be changed at any time;
  >
  > except: `iteration next` must not have end date, and start date must be from
  > last existing `iteration #[x]` end date;

### iteration development

> **suggested** iteration duration is about 3 weeks (+/- a week), depending on
> size of team and importance of project(s);

- **no direct commits** to `master` are permitted - only pull request are
  acceptable;

  > exceptions:
  >
  > last commit of iteration, changing version;
  >
  > newly created repository for one/two its first iterations;

- for each issue contributor **should but not oblige to** use separate branch;

  - branch name **must follow pattern** `{username}-{smth}-dev`

    > **not recommended**: in some exceptions one contributor may have their
    > personal branch for contributing `{username}-all-dev`

    - `username` - branch contributor username (only `[A-z0-9]` allowed)

      > if several people uses branch, use prefix `group-main-`;
      >
      > also **suggested** to add branches for each contributor
      > `group-{username}` in such cases;

    - `smth` - topic of your branch; if branch dedicated to issue - use
      `issue{issueNumber}{issueTopic}`;

      > example: `issue42AnswerToQuestionOfUniverse`;

> see also: [branches and pull requests](#branches-and-pull-requests)

### iteration finalization

requirements for starting iteration finalization;

- all issues and pull requests **must** be closed or moved to next iteration;

- all test **must** be passed;

  - if no test written/present - iteration **can be finalized**, but
    **no steps** on publishing and making new version **must be** done;

  - if tests are broken - in such case two approaches;

    - **must** open issue on test in current iteration, and not closing both,
      until tests are fixed;

    - **degrade** tests to pass them all with degradation of project,
      pre-release note must be added or existing downgraded; after that
      iteration **can be closed only with pre-release**
      **version and mentioning of downgrading in changelog**;

- changelog for iteration **must** be written;

  > you **can** look up closed issues and pull request in finalizing iteration;

- all `{username}-{smth}-dev` branches **must** be deleted;

  > exception for this rule **could be** `{username}-all-dev` branches and
  > **must be** branches for pull request and issues that has been moved to next
  > iteration;

- new app version **must** be provided; [see versioning](#versioning);

  > exception: repositories with no any tests if project implies any present;

  - release of new version **must** contain;

    - changelog;

    - build/compiled files for every platform app supports

      > exceptions: `zero` versions;

    - have deployed version of app on a real server (if any required);

      > exceptions: `zero` and `alpha` versions;

## hotfixes

hotfixes are applied to already published app versions;

hotfixes **recommended** for fixing bugs breaking app normal work or its
features;

hotfixes **not recommended** for adding new functionality or improving existing;

  > exception: if new iteration dedicated to breaking api changes i.e. new major
  > version of app, for example `v1.9.20` to `v2.0.0`;

hotfix **must** have opened issue with [label `is::hotfix`](#type-issue-label)
assigned to current iteration and with
[`priority::red` or `priority::stop` label](#priority-issue-label);

hotfix **must** have created branch for mentioned issue with additional postfix
`-hotfix` before `-dev` in branch name;

  > example `walpy-issue42QuestionIsNotCorrect-hotfix-dev`

hotfix branch **must** be based on closing iteration commit,
i.e. commit with version tag;

  > it is recommended to use release branch if any present;

hotfix branch **must not** be merged with master branch;

  > if there is no branches for releases, **suggested** to create brunch
  > `hotfix-v{appVersionWithHotfix}`;

hotfix changes **must** include app version changes;

  > exception if branch for release versions exists;

hotfix pull requests **must** be treated with more attention and accuracy;

  > it is **advised** to have more than one reviewers and assignees

there are no limits on number of hotfixes per version;

## versioning

> all versions must follow [semantic versioning](https://semver.org/);

versioning is bind to iteration cycle workflow; in the end of each cycle version
may be increased and new app may be released;

> if there is some degradation in project, it is advised to lower pre-release
> postfix or add one if no present;
>
> if project gaining objective stability, it is advised to upper pre-release
> postfix or even remove;

files changes which change app version in project **must be**
in separate commit which closes iteration;

> commit with changes to version is allowed to be committed directly in master
> branch; message of commit **recommended** to follow pattern
> `close iteration {iterationNumber}` and comment include version of app and
> **suggested** version of its internal dependencies if any;

if changes in files for new version are not required,
there must be an empty commit;
[see `git commit --allow-empty`](https://git-scm.com/docs/git-commit#git-commit---allow-empty);

**suggested** following pre-release postfix;

- **zero** - app does nothing it promises and/or unusable;

- **alpha** - app has some may be working functionality and features under
  limited conditions; some test degradation is allowed;

- **beta** - app passed integration test, confirming app follow its api
  specification; no test degradation is allowed;

  > integration tests **must** cover all api specification
  > or no *beta* label allowed;

- **rc** `[short for release candidate]`- local and deployed versions of the app
  passed integration api tests;

- `[no suffix]` *implies **stable*** - same as `rc`, but must be deployed and
  used for a some period of time or/and tested with even more commits;

> current supported semanthic version is `2.0.0`;

## simplified workflow

simplified workflow can be applied for small project;

they do not need implement [iteration livecyles](#iteration-lifecycles);

every commit to `master` branch is new app version (with according changes);
