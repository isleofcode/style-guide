# SCM (git) Guides

We prefer using git in our projects to track state & changes, when possible. Our
branching & commit message guidelines help developers communicate state for
readability by unfamiliar devs, or when revisiting code you wrote 6 months ago.


## Cheat Sheet:
- Use git-flow's branch naming conventions
- Use AngularJS' commit message conventions
- Commit atomically
- Rebase from upstream, merge from downstream
- Do not squash commits
- When in doubt, branch


## Branching
We adhere to a mostly [git-flow](http://nvie.com/posts/a-successful-git-branching-model/)
branching model.

#### Main branches
- `master` is master. Code should be cleaned, tested, and release-worthy.
  Permission to merge to master is restricted to the project lead, and anyone
  they may nominate;
- `develop` is the group sandbox. Code should be feature-complete and not be
  breaking, but may be gappy.

#### Supporting branches
Three branch types stem from `develop`:
- `release/${semver}` are temporary sandboxes, meant to bring `develop` to
  `master`-worthy state;
- `feat/${foo-bar}` are feature branches, where we spend most of our time; and
- `fix/${baz}` are where we fix bugs.

One branch type stems from `master`:
- `hotfix/${abc}`, where we fix a bug that's been promoted to master.

#### Working with branches
When reviewing work, it can be helpful to step through state, both mentally & in
execution. To achieve this, we:
- Commit atomically (more on this in the next section);
- Rebase from upstream branches, e.g.
  `git checkout feat/user-registration; git rebase develop`, to keep the HEAD
  branch's work at the tip; and
- Merge from downstream branches with PRs on GitHub, e.g.
  https://github.com/isleofcode/style-guide/pull/2 (how meta!)


## Committing
We use a consistent style of commit messages, as we find it helps us write
clear, concise notes that are also easily parsable by others & our future
selves. It has a nice side effect of encouraging an atomic & detailed commit
style. h/t @markprokoudine for first introducing us to this habit.

Our commit style is based off [AngularJS' guidelines](https://github.com/angular/angular.js/blob/master/CONTRIBUTING.md#commit).

#### Header
The header is the most important element of the commit message, and is the only
one considered mandatory. It is the first line of a commit message (max 100
chars, <80 preferred) and ought to look like:

```
<type>(<scope>): <subject>
```
Where:
- `<type>` categorizes the work performed in the commit, and must be one of:
    - feat;
    - fix;
    - hotfix;
    - refactor;
    - docs;
    - style;
    - perf;
    - test; or
    - chore;
- `<scope>` should indicate the primary app concept affected by the commit, e.g.
  `user`, `login-page`, `contacts.index`; and
- `<subject>` starts with an imperative verb and describes the work performed,
  e.g. `compute user.fullName from firstName and lastName`, `add user
  registration flow`, `conform route to es6`. Lowercase is preferred, w/no
  trailing dot.

Though `<scope>` is technically an optional parameter, please omit it only for
insignificant commits like `style: add newlines`.

#### Body
The body is an optional part of your commit message. Use it to describe the
motivation for the changes, defend a strategy you've selected, or explain noted
bugs to future readers.

Please use the present imperative tense, i.e. "change" vs. "changed" /
"changes".

#### Footer
The footer is an optional part of your commit message, and is the place to
identify breaking changes & any GitHub issues the commit closes.

#### Atomic commits
This commit style can feel uncomfortable at first, as it forces you to think of
a group of changes as a sequential series of steps. Doing so benefits everyone,
as the developer reflects on the recipe for this successful implementation, and
the reader can follow along or rollback as necessary.

If you are struggling to come up with an accurate commit message that satisfies
the guides above, consider adding / rm'ing work from the planned commit (a GUI
or perverse familiarity with `git diff` will be helpful here).

Compiling the examples from above, you can almost visualize what code was
changed and how:
- `fix(user): compute user.fullName from firstName and lastName`
- `feat(login-page): add user registration flow`
- `refactor(contacts.index): conform route to es6`
- `style: add newlines`
