---
layout: post
title: "Git tips and tricks"
date: 2014-02-13 10:45:01 -0600
categories: git
---

## How do I change the author of my commits?

Sometimes it happens that you commit from different computers with different [git author and email configurations](https://help.github.com/articles/setting-your-commit-email-address-in-git/).
If you prefer consistency, follow the following steps:

### Reset the git commits author

{% highlight shell %}
git filter-branch --commit-filter \
'export GIT_AUTHOR_NAME="Author Name"; \
export GIT_AUTHOR_EMAIL=authoremail; \
export GIT_COMMITTER_NAME="Committer Name"; \
export GIT_COMMITTER_EMAIL=committeremail; \
git commit-tree "$@"'
{% endhighlight %}

### Push the changes to remote

{% highlight shell %}
git push -u -f origin --all
{% endhighlight %}

*   **-u** Most of commits will be out of sync after that change, so this is useful to refresh the association between local branches and remote
*   **-f** Force push
*   **--all** Push all branches

### Keep your clone in-sync

{% highlight shell %}
git fetch origin master
git reset --hard origin/master
{% endhighlight %}

E voilà!

_Source: [could I change my name and surname in all previous commits?](http://stackoverflow.com/questions/4493936/git-could-i-change-my-name-and-surname-in-all-previous-commits)_

* * *

## How many lines of code do I have?

Reading "[How many lines of code is Facebook?](http://www.quora.com/Facebook-Engineering/How-many-lines-of-code-is-Facebook)" question, I found this git gem:

{% highlight shell %}
git ls-files | xargs cat | wc -l
{% endhighlight %}
