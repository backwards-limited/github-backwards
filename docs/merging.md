# Merging

- We merge branches, not specific commits
- We always merge to the current `HEAD` branch

Perform a `merge` with these two basic steps:
- Switch to or checkout the branch you want to merge the changes into (the receiving branch)
- Use the `git merge` command to merge changes from a specific branch into the current branch

## Fast Forward Merge

As an example, to merge the bugfix branch into main:
```shell
git switch main

git merge bugfix
```

i.e. we want to merge the following:

![Merging](images/merging.jpg)

So perform the first step i.e.
```shell
git switch main
```

![Switch](images/git-switch.jpg)

Then we:
```shell
git merge bugfix
```

![Merge](images/git-merge.jpg)

The above is called a **Fast Forward Merge**, because main simply caught up on the commits from bugfix.

So let's actually perform a merge:
```shell
➜ git branch -v
  harry 67dcd21 Add harry's stag patronus
* lily  8530ca3 Add lily's doe patronus
  main  11feb7c Add empty patronus file

➜ git switch main
Switched to branch 'main'

➜ git merge lily
Updating 11feb7c..8530ca3
Fast-forward
 patronus.txt | 16 ++++++++++++++++
 1 file changed, 16 insertions(+)
```

## What if we add a commit to main while on a branch?

![Add commit to main while on branch](images/commit-added-to-main.jpg)

![Merge commit](images/merge-commit.jpg)

The `new commit` now has two parents, unlike most others such as a fast-forward commit.

Example:
```shell
mkdir playlist

cd playlist

git init

touch songs.txt

git add songs.txt

git commit -m "Add songs file"
```

Let's add 2 songs to our file:
```shell
vim songs.txt
```

```text
SOS - ABBA
One Of Us - ABBA
```

```shell
git add songs.txt
git commit -m "Add 2 ABBA songs"
```

Create new branch and add more songs:
```shell
git checkout -b ABBA

echo "Mamma Mia - ABBA" >> songs.txt
git add .
git commit -m "Add Mamma Mia"

echo "Dancing Queen - ABBA" >> songs.txt
git add .
git commit -m "Add Dancing Queen"
```

At this point, if we merge the `ABBA` branch into `main` it would just be a fast forward.

Instead, let's go back to `main` and do some more work.
At first we'll keep things simple and avoid conflicts by adding a new file:
```shell
git switch main

touch podcasts.txt
git add podcasts.txt
git commit -m "Add podcasts file"

cat <<EOF > podcasts.txt
The Lead
RadioLab
This American Life
EOF

git add podcasts.txt
git commit -m "Add 3 podcasts"
```

So now `main` has a commits that branch `ABBA` does not know about:

![Commit on main](images/main-with-commit.jpg)

Let's merge the `ABBA` branch into `main` (and we'll see a merge commit occur):
```shell
git merge ABBA
```
our default editor will pop up - a commit (in this case a merge commit) needs a message, but as we did not provide one with `-m` the editor is presented e.g.
```text
Merge branch 'ABBA'
# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch
#
# Lines starting with '#' will be ignored, and an empty message aborts the commit
```
As we can see, git has suggested a commit message for us - "Merge branch 'ABBA'".
Upon "quitting" or "write and quitting" our new commit will take place:
```shell
Merge made by the 'recursive' strategy.
 songs.txt | 2 ++
 1 file changed, 2 insertions(+)
```

![Merge commit](images/merge-commit-occurred.jpg)

We can also see our new commit on the command line:
```shell
git log --oneline
b5118f1 (HEAD -> main) Merge branch 'ABBA'
9f586c3 Add 3 podcasts
aced795 Add podcasts file
6f466b3 (ABBA) Add Dancing Queen
19c4d9f Add Mamma Mia
41cfb09 Add 2 ABBA songs
b9d521b Add songs file
```

## Merge Conflict

When you encounter a merge conflict, Git warns you in the console that it could not automatically merge.
**It also changes the contents of your files to indicate the conflicts that it wants you to resolve.**

Within a file we could see something like the following, where we are on say `main` and we are currently at `HEAD` of the `main` branch and what is being merged in is a branch named `bug-fix` and so do we keep what we currently have at the top, or take what is being merged in at the bottome, or a combination?
```text
<<<<<<< HEAD
I have 2 cats
I also have chickens
=======
I used to have a dog
>>>>>>> bug-fix
```

So, the contents from the branch you are trying to merge from is displayed between the `=======` and `>>>>>>>` symbols.

Resolving conflicts steps:
- Open the file(s) with merge conflicts
- Edit the file(s) to remove the conflicts; decide which branch's contents you want to keep in each conflict; or keep the content from both
- Remove the conflict "markers" in the document
- Add your changes, and then make a commit

## Merge Conflict Example

We'll carry on from the previous `playlist` repository, though first just get rid of the `ABBA` branch that was merged in, and we'll work on 2 branches to show a merge conflict.

```shell
git branch -d ABBA

git switch -c serena

git switch main  

git switch -c bjorn
```

Current state of play is:
![Creating merge conflict](images/creating-merge-conflict.jpg)

First Bjorn changes file `songs.txt`:
```shell
vim songs.txt
```

```text
Dancing Queen - ABBA 
Minimum Brain Size - King Gizzard & The Lizard Wizard
```

```shell
git add songs.txt
git commit -m "Add KGLW song"
```

Bjorn adds 2 more songs on another commit:
```shell
vim songs.txt
```

```text
Dancing Queen - ABBA 
Minimum Brain Size - King Gizzard & The Lizard Wizard 
The Adults Are Talking - The Strokes 
Last Nite - The Strokes
```

```shell
git add songs.txt 
git commit -m "Add 2 Strokes songs"
```

Meanwhile, Serena adds another ABBA song:
```shell
git switch serena
```

```shell
vim songs.txt
```

```text
SOS - ABBA 
One Of Us - ABBA 
Mamma Mia - ABBA 
Dancing Queen - ABBA 
Gimme Gimme - ABBA
```

```shell
git add songs.txt 
git commit -m "Add 1 ABBA song"
```

Let's just add one more song for Serena:
```shell
vim songs.txt
```

```text
SOS - ABBA 
One Of Us - ABBA 
Mamma Mia - ABBA 
Dancing Queen - ABBA 
Gimme Gimme - ABBA 
Here You Come Again - Dolly Parton
```

```shell
git add songs.txt 
git commit -m "Add 1 Dolly Parton song"
```

Current status is:
![Current status](images/creating-merge-conflict-status.jpg)

GitKraken can show up the whole picture, though we can see most of the picture from a branch e.g.
```shell
git log --oneline --graph
* 1a6ba30 (HEAD -> serena) Add 1 Dolly Parton song
* 3410200 Add 1 ABBA song
*   b5118f1 (main) Merge branch 'ABBA'
|\  
| * 6f466b3 Add Dancing Queen
| * 19c4d9f Add Mamma Mia
* | 9f586c3 Add 3 podcasts
* | aced795 Add podcasts file
|/  
* 41cfb09 Add 2 ABBA songs
* b9d521b Add songs file
```

Let's switch to Bjorn's branch and from there create a new branch named `combo` and merge in Serena's branch into `combo`:
```shell
git switch bjorn

git switch -c combo

git merge serena
Auto-merging songs.txt
CONFLICT (content): Merge conflict in songs.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Aha! Let's fix.

```shell
code songs.txt
```

This time I use Visual Studio Code as it helps out with resolving merge conflicts:

![Resolve](images/resolve-merge-conflict.jpg)