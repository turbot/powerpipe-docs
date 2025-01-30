---
title: POWERPIPE_GIT_TOKEN
---


# POWERPIPE_GIT_TOKEN

Set a GitHub personal access token or application token to use when installing mods from Github.

Powerpipe uses `git` to install and update mods. When you run `powerpipe mod install` or `powerpipe mod update`, Powerpipe will first try using HTTPS and if that does not work it will try SSH.  If your SSH keys are configured properly for `git`, you should be able to pull from private repos that you have access to, as well as public ones.  Alternatively, you can authenticate with a GitHub personal access token or application token.  Set the `POWERPIPE_GIT_TOKEN` environment variable to your token and Powerpipe will use the token when installing and updating mods.


## Usage 
Set your API token:
```bash
export POWERPIPE_GIT_TOKEN=ghp_1t2ieaXluThisIsAFakeTokenG2ei14Q1PBJ
```
