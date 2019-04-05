# Fetch Only Remote

This my current Git workflow:
`origin` is a fork from `upstream` where I do not have push permissions, and rightfully so.
I push to `origin` from my `local` machine. The code is then merged to `upstream` via a pull request.
Of course, I want to fetch the merged code from other people from the `upstream` repository.
And to not break anything, even if I do not have push permission to `upstream` I want to make sure I cannot push there, just in case.

```
origin <------ upstream
 | ^              |
 | |              |
 V |              |
local <-----------+
```

All credit goes to: http://sushihangover.github.io/git-set-up-a-fetch-only-remote/

Fire up a prompt, and check the URLs defined for the remote repositories.
Each repo has 2: one for push, one for fetch.

It looks like this:
```shell
C:\mySuperProject>git remote -v
origin  https://github.com/{myAccount}/{superProject}.git (fetch)
origin  https://github.com/{myAccount}/{superProject}.git (push)
upstream        https://github.com/{ortherOrganisation}/{superProject}.git (fetch)
upstream        https://github.com/{ortherOrganisation}/{superProject}.git (push)
```

Now let's remove the push capability to `upstream`:

```shell
git remote set-url --push upstream DISABLE
```

You can pick `DISABLE`, `/dev/null`, or any other value that will prevent pushing to `upstream`.