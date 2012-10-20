---
layout: post
category : news
header: Credit where credit's due
author: lithium3141
---

GitHub pull requests are a marvelous thing. They've taken cross-team contribution and random patches and made a system, built on Git, that simplifies bugfixes and new features down to the push of a button.

In a pull request, though, there are two key people: the **author** and the **committer**. The author is the one actually responsible for writing the code; the committer is the one that sticks it in a repository (usually a privileged or "main" repository for release purposes). Both of these people should have their names attached to the commit, but by default in a pull request, only the author's name appears - the committer is ignored entirely.

This article lays out a series of Git commands to resolve this problem and mention both contributors on a single commit in GitHub, using a recent patch to [Multiverse-Portals](https://github.com/multiverse/Multiverse-Portals) as an example.

In this case, a friendly dev named VibroAxe added support for portal cooldowns. (What they are isn't important; the important part comes next.) This support was spread across two patches, and regrettably used tabs instead of spaces, so there was still some work to do.

Let's first lay out the existing situation:
* Central repository managed by [myself](http://www.github.com/lithium3141) and [FernFerret](http://www.github.com/fernferret)
* Forked repository for VibroAxe with two new commits
* Fix implemented but with poor tabbing practices

And what do we want to happen?
* Central repository gets **one** new commit
* New patch contains content from both VibroAxe's patches **and** fixed tabbing
* New commit mentions VibroAxe as author, myself as committer

As mentioned, we can't use GitHub's built-in merge button, since that just does a branch merge with fast-forwarding; it has no mention of the committer anywhere.
So let's start by checking out the primary repository, then adding a remote for VibroAxe's repository:

{% highlight bash %}
git clone git@github.com:Herocraft/Multiverse-Portals.git
cd Multiverse-Portals
git remote add vibroaxe https://github.com/VibroAxe/Multiverse-Portals.git
{% endhighlight %}

Now we can fetch the contents of his repository, which contains the commits we're merging:

{% highlight bash %}
git fetch vibroaxe
{% endhighlight %}

Once fetched, we need to work with the new code, so we can check out a branch. However, since we fetched without explicitly branching to track VibroAxe's repository, we need to create a branch name while checking out:

{% highlight bash %}
git checkout -b vibroaxe-cooldowns vibroaxe/master
{% endhighlight %}

Now we're on a branch with the new code that needs to be merged back. Before we combine, though, let's fix the spacing issues - we alter any code that needs to be changed and create another commit. Now we're **three commits ahead** of the upstream master branch.
From this point, getting those commits onto master involves a simple rebase:

{% highlight bash %}
git rebase -i master
{% endhighlight %}

While rebasing, we can leave the first commit as "pick", but "squash" the next two commits down onto the first - this will leave us with only one commit on master that combines the work done in VibroAxe's two commits and the spacing work done in the third.
After fixing up the messages for our new combined commit, we simply push back to master and call it a day:

{% highlight bash %}
git push origin master
{% endhighlight %}

And voila! GitHub shows VibroAxe as the committer of a **single commit** combining **all work needed** for the new cooldowns feature.

![Single Commit](/assets/uploads/rebase_commit.png)

This post was taken from Multiverse Developer, [lithium3141](http://www.lithium3141.com)'s blog. You can find the original post [here](http://lithium3141.com/2011/09/21/credit-where-credits-due/).