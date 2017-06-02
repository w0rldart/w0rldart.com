---
layout: post
title:  "Git tips and tricks"
date:   2014-02-13 10:45:01 -0600
categories: git
---

## How do I change the author of my commits?

Whilst I was going through the log of my Git commits, I had noticed some inconsistencies with the author value of some commits. They were all from me, but with different values, so I decided to try and fix that by setting the author and the email values to universal values. To do so, I used the following command:

<pre class="EnlighterJSRAW" data-enlighter-language="shell">git filter-branch --commit-filter \
'export GIT_AUTHOR_NAME="Author Name"; \
export GIT_AUTHOR_EMAIL=authoremail; \
export GIT_COMMITTER_NAME="Committer Name"; \
export GIT_COMMITTER_EMAIL=committeremail; \
git commit-tree "$@"'</pre>

Which I found whilst reading [could I change my name and surname in all previous commits?](http://stackoverflow.com/questions/4493936/git-could-i-change-my-name-and-surname-in-all-previous-commits) question on StackOverflow. **Next, push changes:**

<pre class="EnlighterJSRAW" data-enlighter-language="shell">git push -u -f origin --all</pre>

*   **-u **I used it because I had almost all commits out of sync after that change, and this is useful to refresh the association between local branches and remote
*   **-f **Force push
*   **--all **Push all branches

**Merge onto local**

<pre class="EnlighterJSRAW" data-enlighter-language="shell">git fetch origin master
git reset --hard origin/master</pre>

And voilà!  

* * *

## How many lines of code do I have?

Reading the most upvoted answer for the [How many lines of code is Facebook?](http://www.quora.com/Facebook-Engineering/How-many-lines-of-code-is-Facebook) question, I found this git gem.

<pre class="EnlighterJSRAW" data-enlighter-language="shell">git ls-files | xargs cat | wc -l</pre>

<span style="line-height: 1.75em;">Obviously the command can be extended and adjusted to custom needs</span>
