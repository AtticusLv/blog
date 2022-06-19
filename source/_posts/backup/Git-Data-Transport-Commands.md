---
title: Git Data Transport Commands
date: 2016-09-02 16:25:05
tags:
- git
categories:
- 技术资料
---
Below picture describes the whole commands during git handling.<br>
{% asset_img git.png Git-Data-Transport-Commands %}

# How to resolve conflicts when merging?
eg. We want to merge both issue2 and issue3, 2 branches to master.
First we merge issue2:
>$ git checkout master
>
>Switched to branch 'master'
>$ git merge issue2
>
>Updating b2b23c4..8f7aa27
>Fast-forward
>myfile.txt |    2 ++
>1 files changed, 2 insertions(+), 0 deletions(-)

{% asset_img issue2.png issue2 %}

 Then we merge issue3:
>$ git merge issue3
>Auto-merging myfile.txt
>CONFLICT (content): Merge conflict in myfile.txt
>Automatic merge failed; fix conflicts and then commit the result.

We can find it shows us merge failure message, cause there is a conflict in myfile.txt
The conflicts part should be modified, then commit again.
>$ git add
>$ git commit -m "message"
>$ git pull origin master
>modify the file manually

In below picture, yellow process shows the modified part.
{% asset_img issue3.png issue3 %}

Sometimes, we'll meet belowing situation:
```
# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
~                                                                               
~                                                                               
~                                                                            
~   
```
we can follow these steps to commit our changes from the master:
1. Press {% kbd i %} to enter **insert** mode
2. Now you can type your message, as if you were in a normal text editor
3. Press {% kbd esc %} to go back to **command** mode
4. Then type `:w` followed by {% kbd enter %} to save
5. Finally `:q` followed by {% kbd enter %} to quit

# Git Basic Commands
* show branch
> git branch
* create or checkout branch
> git checkout -b <newbranch>
* add a remote branch
> git push origin master:<new_branch_name>
* update everything
> git pull
* list existing remotes
> git remote -v
* change remote's url
> git remote set-url origin <git_url>
* show all commits
> git log
* temporarily swith to a different commits
> git checkout <commit_code>
* revert to a previous commit, ignoring any changes
> git reset --hard HEAD
* revert to a commit that's older than the most recent commit
> git reset <commit_code>
> -- Moves pointer back to previous HEAD
> git reset --soft HEAD@{1}
> git commit -m "Revert to <commit_code>"
> -- Updates working copy to reflect the new commit
> git reset --hard
