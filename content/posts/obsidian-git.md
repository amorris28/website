---
title: "Simple guide to setting up git sync in Obsidian"
date: 2025-11-29
author: "Andrew"
description: "How to sync your Obsidian vault using git and Github."
tags: ["technical-guides"]
draft: false
---

I wanted to try out syncing my [Obsidian](https://obsidian.md/) vault via [Github](https://github.com/) and the guides that show up at the top of [DuckDuckGo](https://duckduckgo.com/) seem either out-of-date or made by folks who are primarily Obsidian users and haven't used [git](https://git-scm.com/) before. For example, many of them use personal access tokens, which is unnecessary if you have [SSH setup](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent). So, I figured out an easy way of setting up git sync and decided to document it here. This guide assumes you have a Github account and already have an SSH key for Github.

## Step 1. Create a Github repo

Should be obvious if you use Github, but for completeness: Login to Github, click on the "+" sign in the top right corner and from the drop-down choose "[New repository](https://github.com/new)." Name it whatever you want. Set it to "Private" if you don't want anyone reading your notes. I left the rest of the settings at the default. 

## Step 2. Initialize git in your vault

Create a new vault in Obsidian in a particular directory like `~/Github/vault` or `cd` into the root of an existing vault. Initialize the local git repo and connect it to the Github repo you created. Replace `yourusername` and `your-repo` with your username and repo name, respectively:

```bash
cd ~/Github/vault/
git init
git remote add origin git@github.com:yourusername/your-repo.git
```

## Step 3. Create a .gitignore

Create a `.gitignore` file in your vault root and add the following lines:

```bash
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.trash/
```

This will prevent syncing device-specific settings and trash while syncing your plugins, themes, and settings.

## Step 4. Make your first commit

```bash
git add .
git commit -m "Initial vault setup"
git branch -M main
git push -u origin main
```

## Step 5. Configure the [Obsidian Git Community Plugin](https://github.com/Vinzent03/obsidian-git)

In Obsidian with the vault open, go to Options > Community plugins > Browse and search for "git." Should be the first option. Install and enable it. Then go to the git plugin settings page under Community plugins.

- Set "Auto commit-and-sync interval (minutes)" to 10-15 minutes
- Enable "Auto commit-and-sync after stopping file edits"
- Set "Auto pull interval (minutes)" to 10-15 minutes (same as backup interval)
- Set "Merge strategy" to Merge.
- Enable "Pull on startup"
- Enable "Push on commit-and-sync"
- Add your name and email to "Author name for commit" and "Author email for commit"

Everything else I leave at the default.

If you are the only one commiting to the repo, it is unlikely you will run into merge conflicts unless you fail to pull on startup or push after editing, so I recommend enabling those. You can bump the auto commit-and-sync and auto pull interval to a shorter time interval if you switch devices frequently.

## Conclusion

That is all and syncing should work seamlessly. Simply start editing a note and when you stop editing and the commit-end-sync interval ends, it will automatically pull, commit, and push to Github. Just note that if you make edits and close Obsidian before the commit-and-sync interval is up, it will not commit and push changes to Github. If you then open the same vault on another device and commit-and-sync, you may run into merge conflicts. Therefore, to be safe, you might want to do a manual commit-and-sync before closing Obsidian. 

