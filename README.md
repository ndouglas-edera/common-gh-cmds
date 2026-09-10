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
Before changing anything, I made sure the existing site builds.
```
cd Desktop/docs-site
```
I ran the command:
```
npm run dev
```
According to the repo's instructions, that should start Hugo with drafts enabled and serve the site at:
```
http://localhost:1313
```
