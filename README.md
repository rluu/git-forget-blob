# Completely remove a file from a git repository with git forget-blob

## Description

Completely remove a file from your git repository, including old commits, reflog and other references.

![example](/resources/blob.gif)

## Usage

    git forget-blob file-to-forget

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

# Direct the above command's output to a text file, and then 
# use that text files contents in a bash loop as arguments when 
# running the script. 
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
