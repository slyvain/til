# Tar-GZ a Directory

TAR man page: https://linux.die.net/man/1/tar

The command `tar` will archive the fullpath of a directory. What if we want to archive (and compress) only the content of a directory?

The option `-C`, short for `--directory`, will set the `tar` execution in the given directory.

We then need to indicate what to archive, we can do that by using a `.` or another path.

```shell
tar -zcvf my-archives.tar.gz -C "fullpath-directory-to-archive" .
```
> Note the `.` at the end of the line. We then exclude all parent directories and archive only the content.

## Explanation

Say we're in `/home/user1`, and we want to archive the files in `/home/user1/archives/very-important-files/`.

```shell
tar -zcvf my-archives.tar.gz ~/archives/very-important-files/
```
This will include the fullpath: `/home/user1/archives/very-important-files/`.

Were we to extract the archive in `/home/user1/archives-copy/`, we would find our files in:

`/home/user1/archives-copy/home/user1/archives/very-important-files/`.

To archive only the content of `very-important-files` directory, we can execute:
```shell
tar -zcvf my-archives.tar.gz -C ~/archives/very-important-files/ .
```

We can also select a sub-directory, which will archive the content of the sub-directory:
```shell
tar -zcvf my-archives.tar.gz -C ~/archives/very-important-files/ /even-more-important-files/
```
(we could just use -C with this sub-directory, but hey, that works too!)