# Common GitHub Commands
Bunch of random Github commands I'll probably need in the future

```
brew install gh
```
```
gh auth login
```

or my preferred method:

```
gh auth login --web
```
```
gh auth status
```

That last command explicitly configures Git to use GitHub CLI as its credential helper.
```
gh auth setup-git
```
```
cd ~/Desktop
git clone https://github.com/edera-dev/docs-site.git
```

In the ```docs-site``` directory, check for a specific markdown ```.md``` file you'd like to edit
```
find content -iname '*falco*'
```
I was looking for ```content/guides/observability/falco-integration.md```
<br/><br/>
The second command is particularly useful because the file might have a name like ```quickstart.md``` while containing "```Falco```" in its title.
```
grep -Ril "Falco" content
```

## Create a branch before changing anything
Once we know the file, we'll make a branch:
```
git status
```
The ```main``` branch should stay pristine, and everything we experiment with lives on the ```new branch```.
```
git checkout -b docs/falco-quickstart
```
We can inspect it from Terminal with:
```
sed -n '1,240p' content/guides/observability/falco-integration.md
```

<img width="1167" height="635" alt="Screenshot 2026-09-10 at 11 04 04" src="https://github.com/user-attachments/assets/675d792f-b3e4-4fb1-82cd-1be76d2a03da" />


## Start Hugo locally
Need to install Hugo for Mac:
```
brew install hugo
```
Confirming it was installed correctly:
```
hugo version
```
Before changing anything, I made sure the existing site builds.
```
cd Desktop/docs-site
```
I ran the command:
```
npm run dev
```

<img width="1167" height="635" alt="Screenshot 2026-09-10 at 11 13 44" src="https://github.com/user-attachments/assets/81e5ed0b-1745-411c-9222-ec0cd4fc87da" />


I got errors. Needed to check whether the repo pins Hugo somewhere else.
```
grep -R "0.140.2" -n . --exclude-dir=.git --exclude-dir=node_modules
```

I installed the wrong Hugo version, so it had to be uninstalled via ```brew```:
```
git submodule status
```
```
ls -la themes
```
```
brew uninstall hugo
```
<img width="1167" height="635" alt="Screenshot 2026-09-10 at 11 18 24" src="https://github.com/user-attachments/assets/0c771a7a-be46-4262-9c87-5a569ca8d0c2" />

We can ask GitHub what assets actually exist for ```Hugo 0.140.2```:
```
gh release view v0.140.2 --repo gohugoio/hugo
```
<img width="1150" height="814" alt="Screenshot 2026-09-10 at 11 26 10" src="https://github.com/user-attachments/assets/f7875b60-e779-44c8-9a08-af63891cd190" />

```
rm -f /tmp/hugo.tar.gz
```
```
curl -L -o /tmp/hugo.tar.gz \
  https://github.com/gohugoio/hugo/releases/download/v0.140.2/hugo_extended_0.140.2_darwin-universal.tar.gz
```
```
mkdir -p ~/bin
tar -xzf /tmp/hugo.tar.gz -C ~/bin  
```
```
ls -l ~/bin/hugo
```
```
chmod +x ~/bin/hugo
```
```
~/bin/hugo version
```
```
which hugo
hugo version
```
```
git submodule update --init --recursive
```
```
git submodule status
```
<img width="1153" height="662" alt="Screenshot 2026-09-10 at 11 31 30" src="https://github.com/user-attachments/assets/795cfcb5-c493-4951-b798-a55e4196c18a" />

## Hugo is now finally install correctly at the right PATH
```
npm run dev
```

According to the repo's instructions, that should start Hugo with drafts enabled and serve the site at:
```
http://localhost:1313
```
<img width="1153" height="495" alt="Screenshot 2026-09-10 at 11 38 47" src="https://github.com/user-attachments/assets/b97e47be-a6d3-4b30-82c3-647a5136e60f" />

Reading a **Falco Integration** docs:
```
cat Desktop/docs-site/content/guides/observability/falco-integration.md
```

Reading the **CLI User Guide** docs:
```
cat Desktop/docs-site/content/guides/cli-user-guide.md
```

## Push to a branch and open an issue in the security repo

From ```~/Desktop/docs-site```, check what changes have been made:
```
git status
```
Before doing anything else, check the actual diff:
```
git diff -- content/guides/cli-user-guide.md
```
I want to verify that the only change is the ```<iframe>```. <br/>
If I'm currently still on ```main```, I need to create a feature branch.
```
git switch -c docs/add-webernetes-iframe
```
My existing change will remain there — creating the branch doesn't throw anything away. <br/>
First, I will need to ```stage``` the file:
```
git add content/guides/cli-user-guide.md
```
Then check what we are about to ```commit```:
```
git diff --cached
```
<img width="1152" height="663" alt="Screenshot 2026-09-10 at 15 07 14" src="https://github.com/user-attachments/assets/b7c80d11-1b59-4924-a901-1fc7c9f2c5e2" />


```
git commit -m "docs: added the Webernetes terminal to CLI guide"
```
```
git status
```
Push the branch to GitHub
```
git push -u origin docs/add-webernetes-iframe
```
Because I'm already authenticated with GitHub CLI, this should work without asking for a password. <br/>
I can open the pull request:
```
gh pr create --fill
```

<img width="1152" height="630" alt="Screenshot 2026-09-10 at 15 10 00" src="https://github.com/user-attachments/assets/10310e47-fc70-4d34-adeb-0e7e34ed3c42" />


It'll ask us for things like the ```title```/```body```.<br/>
PR title:
```
docs: add Webernetes terminal to CLI guide
```
Simple description:
```
## Summary

- Adds the interactive Webernetes terminal to the CLI user guide.
- Embeds the existing Webernetes demo using an iframe.

## Testing

- Verified locally with `npm run dev`.
- Confirmed the iframe renders correctly at localhost:1313.
```
Finding the ```security``` repo:
```
gh repo list edera-dev --limit 100
```
