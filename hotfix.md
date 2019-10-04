# hotfixes

hotfixes are applied to already published app versions;

hotfixes **recommended** for fixing bugs breaking app normal work or its features;

hotfixes **not recommended** for adding new functionality or improving existing;

  > exception: if new iteration dedicated to breaking api changes i.e. new major version of app, for example `v1.9.20` to `v2.0.0`;

hotfix **must** have opened issue with [label `is::hotfix`](./issue_labels.md#type) assigned to
current iteration and with
[`priority::red` or `priority::stop` label](./issue_labels.md#Label-Priority);

hotfix **must** have created branch for mentioned issue with additional postfix `-hotfix` before `-dev` in branch name;

  > example `walpy-issue42QuestionIsNotCorrect-hotfix-dev`

hotfix branch **must** be based on closing iteration commit, i.e. commit with version tag;

  > it is recommended to use release branch if any present;

hotfix branch **must not** be merged with master branch;

  > if there is no branches for releases, **suggested** to create brunch `hotfix-v{appVersionWithHotfix}`;

hotfix changes **must** include app version changes;

  > exception if branch for release versions exists;

hotfix pull requests **must** be treated with more attention and accuracy

  > it is **advised** to have more than one reviewers and assignees

there are no limits on number of hotfixes per version;
