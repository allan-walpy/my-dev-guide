# Versioning

> all versiion must follow [semathic versioning](https://semver.org/)

Versioning is bind to iteration cycle workflow; In the end of each cycle version must increase.
If there is some degradation in project, it is advised to lower stability (stable to beta or beta to
alpha etc.) and upper otherwise

> **NB!** Although, direct commits to master are forbidden, there is exception for `new version`
> commit: it must be ethier empty
> ([see `--allow-empty`](https://git-scm.com/docs/git-commit#git-commit---allow-empty))
> or contain only those changes, that changes version of projects. Title of commit should be
> `close iteration {iterationNumver}` and comment include versions of all project's instance