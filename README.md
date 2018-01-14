# Completely remove a file from a git repository with git forget-blob

## Description

Completely remove a file from your git repository, including old commits, reflog and other references.

![example](/resources/blob.gif)

## Usage

    git forget-blob file-to-forget

## Caveats

Script `git-forget-blob.sh` assumes that GNU xargs exists on the system and is in the system PATH.
However, Mac OS X ships with a BSD xargs by default, which does not support the `--no-run-if-empty` option.
The workaround is to use Homebrew to install and use the `findutils` package, which contains GNU xargs.
Then either update the `git-forget-blob.sh` to use this different version of xargs, or ensure that 
the GNU xargs is used by default by being listed earlier in the user PATH environment variable.

## Other Information

From https://stackoverflow.com/a/20609719

```
cd ${TOP_LEVEL_GIT_REPO_DIRECTORY}

# Remove stale junk.
git gc

# Print the 10 largest files in the repository.  
git rev-list --objects --all | \
    grep "$(git verify-pack -v .git/objects/pack/*.idx | \
    sort -k 3 -n | \
    tail -10 | \
    awk '{print$1}')"

# Direct the above command's output to a text file.
git rev-list --objects --all | \
    grep "$(git verify-pack -v .git/objects/pack/*.idx | \
    sort -k 3 -n | \
    tail -10 | \
    awk '{print$1}')" > hash_and_blobs.txt

# Remove the hash, leaving the blob name on each line.
awk '{$1=""}1' hash_and_blobs.txt | awk '{$1=$1}1' > blobs.txt

# Remove each blob in blobs.txt in a loop.
for f in `cat blobs.txt`; do ./git-forget-blob.sh $f; done;
```


See more details at

https://ownyourbits.com/2017/01/18/completely-remove-a-file-from-a-git-repository-with-git-forget-blob/
