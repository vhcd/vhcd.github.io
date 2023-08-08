---
published: true
title: Version control
---
As a software engineer, [version control](https://en.wikipedia.org/wiki/Version_control) is mandatory for any project I work on. A driving simulation platform, or experiment, is no exception. However, such projects have quite a few differences from traditional software. In this post, we'll explain the challenges we faced regarding source control, and how we answered them (or failed to).

# Requirements

In what ways are our projects different from classic software? Well first, we'll have to explain our workflow.

Currently, the platform is an Unreal Engine project, which includes assets, plugins, source code, configuration files, etc. For each experiment using the platform, we create a [fork](https://en.wikipedia.org/wiki/Fork_(software_development)) of the project, where we add all project-specific development. That way, the main project isn't polluted with features that are too specific, and can continue adding on common features. If the fork wants to get new features, or if the main project decides to actually integrate a project-specific feature, this can be done using our source control tool. When the fork reaches "production" state, i.e. the experiment is running, it is frozen to ensure that we'll always have an exact copy of the platform that was used for the experiment.

This in itself isn't a really novel use of source control. However, the content of the project is. We have assets, and lots of them. Meshes, materials, textures, sounds, etc. All in binary format. Currently, we have over 150GB of them, coming from over 150 assets packs from the [Marketplace](https://www.unrealengine.com/marketplace/en-US/store).

Do we need to version those? In my opinion, yes. We tend to modify a lot of assets we bought, to tailor them to our needs. Sometimes we fix bugs, or expend features. This requires versioning.

On top of that, we need version control to be simple to use. Most our users aren't too familiar with version control systems, so we need our version control to be intuitive, and not just writing dozens of `git` commands in a terminal.

# Which tool?

What tools are there for us? Well, you have the common ones, such as [Git](https://en.wikipedia.org/wiki/Git) or [SVN](https://en.wikipedia.org/wiki/Apache_Subversion). And then you have some that are more focused on using game engines, such as [Plastic SCM](https://en.wikipedia.org/wiki/Plastic_SCM) or [Perforce](https://en.wikipedia.org/wiki/Perforce). How to choose?

Well, we don't have that much money lying around, so even though Perforce probably is the gold standard for Unreal Engine development, we can't really use it. They have a free tier, but their 5-user limit is a no-go for us. Same for Plastic SCM, which is also more oriented towards Unity, which actually bought the company, in what I think is a very smart move on their part.

So, we're left with Git or SVN. Git notoriously doesn't work well with binary content, which we have *a lot* of. However, the [Git LFS](https://git-lfs.github.com/) extension brings better support on that front. SVN, though declining overall, is still liked in the game-engine world for its simplicity, native binary and file-locking support.

In the end, we chose Git. Our existing software projects (e.g. data acquisition/wrangling) are already using Git. It's the tool that people are most familiar with. And if, like us, you need an external host, there's really not much alternative.

# Which host?

When you have a 150GB project that you want to host on Git, where do you do it?

The best solution would probably be a self-hosted [GitLab](https://gitlab.com/) instance. However, our IT service has no plan of hosting a GitLab, and if I wanted to do that, I'd be left more or less alone. It's doable, but I don't really want to maintain that.

What's out there? [GitHub](https://github.com/), cloud-based GitLab or [Azure Repos](https://azure.microsoft.com/en-us/services/devops/repos/). Each have their limitation

* GitHub has a 1GB of storage and 1GB of bandwidth limit for LFS, which you can extend by buying [data packs](https://docs.github.com/en/github/setting-up-and-managing-billing-and-payments-on-github/upgrading-git-large-file-storage#purchasing-additional-storage-and-bandwidth-for-an-organization)
* GitLab has a 10GB per repo size limit
* Azure has a 5-user limit

Not ideal in any case. We chose GitLab, as its limitations are the easiest ones to work with.

# In practice

We have a 150GB project, of mostly binary assets, which we want to version using Git LFS on GitLab, have a fork for every experimentation, and be accessible for users that don't know what Git is. How do we manage that?

## Setup

Our main repository is what we call a [superproject](https://en.wikibooks.org/wiki/Git/Submodules_and_Superprojects), as it's mostly a container of [submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules). Since we have well over a hundred asset packs from the Marketplace, we put each one in a submodule. Thankfully, nearly all of them are under 10GB in size, so they fit within GitLab's limits. We also have submodules for each plugin we use (around two dozens), and for each [scene](/making-a-scene) we create. Not only does it solve the 10GB issue, it's also cleaner to have each of those in its own repository, as it makes for a better and clearer version log on each. Instead of having 10 commits that modify wildly different assets on a single repo, we have 10 repos, each having a single commit. Cleaner, but a bit more difficult to work with.

On the client side, we recommend our users to try [TortoiseGit](https://tortoisegit.org/), which has a nice Windows shell integration, making it easy to work with. We also wrote some shell scripts that can do some of the more complex tasks (e.g. merging upstream from a fork).

## The bad

Now for the bad. Being used to version control for C++ or Python based software projects, I have been very disappointed by how this particular subject is handled in Unreal Engine. Compared to everything else in the engine, version control can be quite painful to work with on a day-to-day basis.

There's no official Git support from Unreal Engine, but there's an [unofficial plugin](https://github.com/SRombauts/UE4GitPlugin) that's used by most Git users. If you read through the features, it looks great and seems like it would fit most needs. But the reality is different.

The plugin doesn't handle submodules. So right off the bat, it's pretty much useless for us, as we have a superproject that's just submodules. But just by having the plugin enabled, most in-editor actions (e.g. file save, move) take a significant amount of time longer, so much that I now only enable the plugin when  I really need it (e.g. [Collections](https://docs.unrealengine.com/en-US/Basics/ContentBrowser/UserGuide/Collections/index.html)).

Since every file in Unreal Engine is in binary, diffing/merging is a huge pain. There's a [diffing tool](https://www.unrealengine.com/en-US/blog/diffing-blueprints?sessionInvalidated=true) for Blueprints, which *mostly* works, but will sometimes tell you that there's no difference between two revisions, leaving you wondering what could have possibly changed. And it's just for Blueprints, so you can't diff levels, materials, UI, or anything that you'd really like to be able to diff. And obviously, it doesn't work with submodules, so if you want to diff, you need to have the two files on your local drive.

And beyond diffing, there's no merging tool. If you're used to some variant of [Gitflow workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow), you'd better hope that there's no conflict when merging branches, otherwise you're in for a headache. Having dealt with a few, I now try my best at avoiding them.

People often mentions [file locking](https://github.com/git-lfs/git-lfs/wiki/File-Locking) as a (well, *the*) solution to avoid conflicts, which is also why people in this world like SVN. I haven't tried it yet, but from what I've read, file locking in LFS isn't that great, mostly due to the decentralized nature of Git. And for our cases, I'm really not sure that file-locking would help across branches or even forks.

The forking workflow also is source of headaches when confronted to our superproject configuration. When forking the superproject, you're not actually forking the submodules. Which can be good or bad, depending on the usage. But if we really wanted to have a clean fork of *all* submodules, we'd be in for a wild ride setting this up.

LFS also doesn't really like forking that much, as it doesn't actually know which remote Git is talking to. So if you switch your local repo from a fork to upstream, you also have to run `git config lfs.url git@gitlab.com:my_new_remote` if you don't want LFS to shout all kinds of non-obvious errors on checkout/push. You can make some aliases and functions to make this easier, but still.

And when you combine the two last issues, and you need to change remote on all submodules? Well, now you start to like `git submodule foreach` a great deal all the while hating how painful this whole thing has become.

I could go on and on about all the weird errors that I saw popped on my git terminal, and that cost me hours to debug. No matter how infuriating this is for our users, developers or myself, I still think that given the circumstances, we've chosen the right solution. Git LFS, hosted on GitLab as a superproject, it works. It's not easy every day, but it works.

# Hoping for a better tomorrow

When the news came out that [Unity bought Plastic SCM](https://blogs.unity3d.com/2020/08/13/codice-software-joins-unity-technologies-to-bring-version-control-to-real-time-3d-workflows/), I really hoped that this would trigger some sort of reaction from Epic Games. I still do, because version control is so lacking in the Unreal Engine world. This is even more infuriating when you realize that most assets (e.g. blueprints, levels) actually are mostly text, and could probably be versioned, diff'd and merged as such. Given the amount of money Epic Games have, I wish they'd make a deal with Perforce or [Assembla](https://www.assembla.com/perforce) to solve this issue once and for all.

Because beyond the Git and Unreal Engine interaction, there's our huge reliance on GitLab's free tier. If they decided to change anything on that (e.g. max users, bandwidth, repo size, repo amount), we'd be stuck with really nowhere to go (well, we'd maintain our own GitLab instance). So I'm thankful everyday for GitLab's free tier, and really hope it won't change anytime soon.

In simple words, version control is, by far, the worst aspect of our whole Unreal Engine adventure.
