# issue and pull requests labels

> issues code presented here optimized for gitlab and their
> [scope labels](https://gitlab.com/help/user/project/labels.md#scoped-labels);

## type

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

## priority

**single** label is **required** for **opened** issues with **iteration #any** milestone;

> scope `priority`; color: **depend on label**;

| code               | color  | remark   |
| :----------------- | :----: | :------- |
| `priority::green`  | `#4a4` | low      |
| `priority::orange` | `#f80` | medium   |
| `priority::red`    | `#f44` | high     |
| `priority::stop`    | `#000` | blocking |

## scope

**at least one** label is **optional** for **any** issues;

> scope `scope`; color: `#448`; postfix: `::_`;

| code                   | remark                        |
| :--------------------- | :---------------------------- |
| `scope::code::_`       | affects actual program code   |
| `scope::code::arch::_` | affects program architecture  |
| `scope::repo::_`       | affects repository management |
| `scope::test::_`       | affects tests                 |

## stage

**single** label is **required** for **opened** issues with
**iteration #current** milestone;

except: **x** label is **required** for **any** **closed** issues;

> scope `stage`; color: `#48a`, `#000` for `x` stage;

| code                | remark      |
| :------------------ | :---------- |
| `stage::holding`    | on hold     |
| `stage::processing` | in progress |
| `stage::x`          | closed      |

## cause of closure

**at least one** label *including `stage::x` label* is **required**
for **closed** issues;

> scope `stage::x`; color: `#000`;

| code                       | remark                                         |
| :------------------------- | :--------------------------------------------- |
| `stage::x::done`           | merged/resolved/fixed/implemented/done         |
| `stage::x::invalid`        | not an issue                                   |
| `stage::x::invalid::clone` | duplicate                                      |
| `stage::x::rejected`       | declined/won't fix/won't merge/won't implement |
