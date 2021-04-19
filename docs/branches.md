# Branches

![Commits](images/commits.jpg)

The `main` branch doesn't do anything special.
Upon creating a repository, you have to start on a branch and so one is automatically created and this will be the `main` branch by default, but it doesn't have to stay that way.

## HEAD

```shell
git log --oneline
f27630c (HEAD -> main) Introduce .gitignore
9b4a486 Only commit 1 file instead of 2
a4b7a7e Add titanic images
...
```
or
```shell
git log
commit f27630c3d905634a3e3827f2993d2e2511297963 (HEAD -> main)
Author: David Ainslie <dainslie@gmail.com>
Date:   Sat Apr 17 16:32:31 2021 +0100

    Introduce .gitignore
...
```

`HEAD` is simply a pointer that refers to the current `location` in your repository.
It points to a particular branch reference.

When working on a repository, the HEAD point somewhere at any one time.
When we clone we will be on `main` and that is where HEAD points.
Then we might switch (get checkout) to a branch and that is where HEAD points:

![On main](images/head-on-main.jpg)

and we switch to a branch:

![On branch](images/on-a-branch.jpg)

## Creating and Switching Branches

![Create new branch](images/create-new-branch.jpg)

As an example, we'll work in a new repository:

```shell
mkdir road-trip-play-list

cd road-trip-play-list 

git init
```

We'll create a new file using a `Here document`: 

```shell
> cat > playlist.txt <<EOF
> SONG - ARTIST
> =============
> EOF
```

```shell
git add playlist.txt 

git commit -m "Add new playlist header"
```

We'll add some songs:
```shell
> cat >> playlist.txt <<EOF
> SOS - ABBA
> One of Us - ABBA
> EOF
```

```shell
git add playlist.txt 

git commit -m "Add 2 ABBA songs"
```

Create a new branch (switching to it at the same time, and we'll see HEAD has moved but still at the same commit):
```shell
git checkout -b oldies
Switched to a new branch 'oldies'

git log --oneline
bf88e1b (HEAD -> oldies, main) Add 2 ABBA songs
9287a04 Add new playlist header
```

However, `checkout` is being deprecated in favour of `switch` e.g.
```shell
git switch main
Switched to branch 'main'

git switch oldies
Switched to branch 'oldies'
```

Ok, let's add songs on this new branch:
```shell
> cat >> playlist.txt <<EOF
> He Stopped Loving Her Today - George Jones
> The Race is on - George Jones
> EOF
```

```shell
git add playlist.txt 

git commit -m "Add two songs by George Jones"
```

Now we'll see `main` in left behind as `HEAD` tracks the latest commit:
```shell
git log --oneline
97f3a1d (HEAD -> oldies) Add two songs by George Jones
bf88e1b (main) Add 2 ABBA songs
9287a04 Add new playlist header
```