# Undo a GIT commit not yet pushed

There are 3 ways to undo a commit that is not yet pushed:

##### Undo commit and keep files staged
`git reset --soft HEAD~`

##### Undo commit and unstage the files
`git reset HEAD~`

##### Undo commit and revert changes
`git reset --hard HEAD~`