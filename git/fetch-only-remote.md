# Fetch Only Remote

http://sushihangover.github.io/git-set-up-a-fetch-only-remote/

This my current Git workflow.
`origin` is a fork from `upstream` where I do not have push permissions, and rightfully so.
I push (always) and pull (never) to `origin` from my `local` machine.
Even if I do not have push permission to `upstream` I want to make sure I cannot push there, just in case.

origin <------ upstream
 | ^              |
 | |              |
 V |              |
local <-----------+
 