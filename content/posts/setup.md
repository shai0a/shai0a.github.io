---
title: "GitHub Page Setup"
date: 2022-05-28T12:04:56-05:00
draft: false
---
## Install Git and Hugo
1. On Arch Linux
```sh
sudo pacman -S git hugo
```

## Hugo new site quickstart and test
1. Create a new site in the specified directory
```sh
hugo new site <dir>
cd <dir>
```

2. Create Git repository
```sh
git init
```

3. Add a theme ([Hugo Themes](https://themes.gohugo.io/))
- Find the theme's repository using the Download link on the theme's page
```sh
git submodule add <theme repository> themes/<theme>
echo "theme = '<theme>'" >> config.toml
```
:warning: The theme repository should be a public and read-only repository therefore it will probably begin with *https://*

4. Create some content
```sh
hugo new posts/<post-name>.md
```

5. In **config.toml**, replace
```yaml
draft: true
```
with
```yaml
draft: false
```

6. Test it out
```sh
hugo server -D
```

7. Open in browser
```sh
xdg-open http://localhost:1313
```

## Configuration for GitHub
**:bell: Replace instances of `<user>` with GitHub user name**
1.  Edit **config.toml**
 - Change
```yaml
baseURL = 'http://example.org/'
```

to

```yaml
baseURL = 'https://\<user\>.github.io/'
```
 :warning: Note the change from ***'http'*** to ***'https'***
 - Add
```yaml
publishDir = 'docs'
```
2. Update
```sh
hugo -D
```

## Setup GitHub and repository
### Create GitHub account
[GitHub Signup](https://github.com/signup)

### CLI Setup
```sh
ssh-keygen -t ed25519
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

#### Install GitHub CLI (Optional)
```sh
pacman -S github-cli
```

##### Login
```sh
gh auth login
```

You will be prompted for the SSH public key you generated earlier if you select SSH as the preferred protocol.

##### Or, if github-cli is already installed and logged in

```sh
gh ssh-key add ~/.ssh/id_ed25519.pub
```


#### Alternatively, setup without Github CLI

```sh
xclip -sel c ~/.ssh/id_ed25519.pub
```

Paste and save at *https://github.com/settings/keys -> New SSH key*

### Create new repository on GitHub (Using GitHub CLI)
```sh
cd <dir>
gh repo create <user>.github.io --public --source=. --remote=upstream
```

### Commit and push to GitHub
1. Tell GitHub to bypass Jekyll processing
```sh
touch docs/.nojekyll
```
2. Commit and push to GitHub
```sh
git add .
git commit -m "first commit"
git remote add origin git@github.com:<user>/<user>.github.io
git push origin master
```


## Assuming your repository is on GitHub and is named \<user\>.github.io
Go to the repository on GitHub.com

1. In *Settings -> Code and automation: Pages*

Change ***Source*** from **'/ (root)'** to ***'/docs'*** and save


2. Try browsing [https://\<user\>.github.io/](https://\<user\>.github.io/)
:warning: Building may take a few minutes. Check under *repository -> Actions -> Workflows -> 
pages-build-deployment*

