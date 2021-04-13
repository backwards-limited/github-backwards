# Committing

Some useful resources for our learning:
- [Faker](https://github.com/marak/Faker.js/)
- [TensorFlow](https://github.com/tensorflow/tensorflow)
- [Signal](https://github.com/signalapp/Signal-Android)
- [Git Ignore](https://www.toptal.com/developers/gitignore) and [GitHub Ignore](https://docs.github.com/en/github/getting-started-with-github/ignoring-files)

## Atomic Commits

When possible, a commit should encompass a single feature, change of fix.
In other words, try to `keep each commit focused on a single thing`.

This make it much easier to undo or rollback changes later on.
It also makes your code or project easier to review.

In our basics repository, create a folder and add some images:
```shell
mkdir mood-board

cd mood-board

wget https://allthatsinteresting.com/wordpress/wp-content/uploads/2019/03/josephine-baker-ostrich.jpg

wget https://allthatsinteresting.com/wordpress/wp-content/uploads/2019/03/1920s-women-convertibles-furs.jpg

wget https://allthatsinteresting.com/wordpress/wp-content/uploads/2019/03/charlie-chaplin-tramp.jpg
```

```shell
cd ..

git status
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        mood-board/
```

Let's change across our text files "Gatsby" with "Catsby":
```shell
find . -name '*.txt' -print0 | xargs -0 sed -i "" "s/Gatsby/Catsby/g"
```

Now we have:
```shell
git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   chapter2.txt
        modified:   characters.txt
        modified:   outline.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        mood-board/
```

If we were to add all as one single commit, this wouldn't really follow the best practice of `atomic commits`.

Let's split the commits:
- A commit for changing `Gatsby` to `Catsby`
- Another commit for adding new images

```shell
git add chapter2.txt characters.txt outline.txt

git commit -m "Rename Gatsby to Catsby"
```

```shell
git add mood-board/

git commit -m "New images"
```

## Commit Messages - Present or Past Tense

It is recommended to use `present tense imperative style` messages.

e.g.
- GOOD: "Make xyz do frotz"
- BAD: "Makes xyz do fotz" or "I changed xyz to do frotz"