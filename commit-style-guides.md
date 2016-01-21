#Commit Style
h/t @markprokoudine for first introducing us to this habit.

Our commit style is based off Angular style, and looks lie this:

```
category(object): message
```

Object is the code you worked on, and category can be:

- refactor;
- fix;
- feat;
- chore;
- hotfix;

Some good examples of commit messages are:

```
fix(user): fullName computed property was broken
feat(login-page): user can now register
refactor(contacts.index): now conforms to es6
```
