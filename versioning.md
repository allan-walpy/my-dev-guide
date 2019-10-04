# versioning

> all versions must follow [semathic versioning](https://semver.org/);

versioning is bind to iteration cycle workflow; in the end of each cycle version may be increased and new app may be released;

> if there is some degradation in project, it is advised to lower pre-release postfix or add one if no present;
>
> if project gaining objective stability, it is advised to upper pre-release postfix or even remove;

files changes which change app version in project **must be** in separate commit which closes iteration;

> commit with changes to version is allowed to be committed directly in master branch; message of commit **recommended** to follow pattern `close iteration {iterationNumber}` and comment include version of app and **suggested** version of its internal dependencies if any;

if changes in files for new version are not required, there must be an empty commit; [see `git commit --allow-empty`](https://git-scm.com/docs/git-commit#git-commit---allow-empty);

**suggesting** following pre-release postfix;

- **zero** - app does nothing it promises and/or unusable;

- **alpha** - app has some may be working functionality and features under limited conditions; some test degradation is allowed;

- **beta** - app passed integration test, confirming app follow its api specification; no test degradation is allowed;

  > integration tests **must** cover all api specification or no *beta* label allowed;

- **rc** `[short for release candidate]`- local and deployed versions of the app passed integration api tests;

- `[no suffix]` *implies **stable*** - same as `rc`, but must be deployed and used for a some period of time or/and tested with even more commits;
