# Issue and Pull Requests Labels

Issue labes managed by repository onwer

## Label Type

scope `is`;

**single** label is **requeired** for **any** issues;

- `is::bug` - issue about bug;
- `is::feature` - issue about new features;
- `is::improvement` - issue about improving app code etc;
- `is::merge` - issue about merge/pull request;
- `is::hotfix` - issue about hotfix;
- `is::question` - issue for questions and disscussions;

## Label Priority

scope `priority`;

**single** label is **required** for **opened** issues with **iteration`#`** milestone assigned;

- `priority::green` - minor priority;
- `priority::yellow` - above average priority;
- `priority::red` - critical priority;
- `priority::critical` - super critical priority (typicaly used for hotfixes of important things);

## Label Scope

scope `scope`;

label is **optional**

- `scope::common` - issue with not determined scope or with multiply scopes;
- `scope::project` - issue affects repository managment;
- `scope::core` - issue affects project code;
- `scope::test` - issue affects tests; NB! affected only incorrect/buggy tests, correct failed tests not affected;

## Label Stage

scope `stage`;

**single** label is **optional** for **opened** issues

- `stage::on_hold` - issue soon be addressed, or hold until some other issues resolved;
- `stage::in_process` - issue is being resolving;
- `stage::on_closing` - issue resolve is on review;
- `stage::closed` - **required** for any **closed** issues;

## Label Cause of Closure

scope `stage::closed`;

**single** label is **optional** for **closed** issues

- `stage::closed::done` - issue resolved/fixed/done;
- `stage::closed::invalid` - issue not aproved, not an issue;
- `stage::closed::invalid::duplicate` - issue already exists;
- `stage::closed::rejected` - declined pull requests, not inplemented issues, never fixing bugs etc;
