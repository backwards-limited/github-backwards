# Setup

Apologies I shall only cover **Mac** - One day I may include Linux and Windows.

Install [Homebrew](https://brew.sh) for easy package management on Mac:

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Install essentials:

```shell
brew install git
brew install --cask gitkraken
```

Other useful installations:

```shell
brew cask install virtualbox
brew cask install docker
brew install httpie
brew install thefuck
```

With Git installed we now configure.
You don't need to register for an account of anything, but you will need to provide:
- Your name
- Your email

To configure the name that Git will associate with your work:
```shell
git config --global user.name "David Ainslie"
```

which you can check:
```shell
git config user.name
David Ainslie
```

Do the same for your email.
When using GitHub, you'll want your Git email address to match your GitHub account:
```shell
git config --global user.email dainslie@gmail.com
```

and check it:
```shell
git config user.email
dainslie@gmail.com
```

With a new project (e.g. when I just started this one and pushed to GitHub):
```shell
git --version
git version 2.31.1

git log --oneline
acf93d8 (HEAD -> main, origin/main) Simplify table of contents
03fbe05 Initial
```

Just to finish off, it is recommended to run the following so that all new projects have an initial branch named `main`:
```shell
git config --global init.defaultBranch main
```