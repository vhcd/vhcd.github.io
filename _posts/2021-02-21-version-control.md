---
published: false
title: Version control
---
As a software engineer, [version control](https://en.wikipedia.org/wiki/Version_control) is mandatory for any project I work with. A driving simulation platform, or experiment, is no exception. However, such projects have quite a few differences from traditional software. In this post, we'll explain the challenges we faced regarding source control, and how we answered them (or failed to).

# Requirements

In what ways are our project different than classic software? Well first, we'll have to explain our workflow.

Currently, the platform is an Unreal Engine project, which includes assets, plugins, source code, configuration files, etc. For each experiment using the platform, we create a [fork](https://en.wikipedia.org/wiki/Fork_(software_development)) of the project, where we add all project-specific development. That way, the main project isn't polluted with features that are too specific, and can continue adding on common features. If the fork wants to get new features, or if the main project decides to actually integrate a project-specific features, this can be done using our source control tool. When the fork reaches "production" state, i.e. the experiment is running, it is frozen to ensure that we'll always have an exact copy of the simulator that was used in the experiment.

This in itself isn't a really novel use of source control. However, the content of the project is. We have assets, and a lot of them. Meshes, materials, textures, sounds, etc. All in binary format. Currently, we have over 150GB of them, coming from over 150 assets packs from the [Marketplace](https://www.unrealengine.com/marketplace/en-US/store).

Do we need to version those? In my opinion, yes. We tend to modify a lot of assets we bought, to tailor them for our needs. Sometimes we fix bugs, or expend features. This requires some versioning.

On top of that, we need version control to be simple to use. Sadly, most our researchers aren't too familiar with version control systems, so we need our version control to be intuitive, and not just writting dozens of `git` command in a shell.

# Which tool?

What tools are there for us? Well, you have the common ones, such as [Git](https://en.wikipedia.org/wiki/Git) or [SVN](https://en.wikipedia.org/wiki/Apache_Subversion). And then you have some that are more focused on using game engines, such as [Plastic SCM](https://en.wikipedia.org/wiki/Plastic_SCM) or [Perforce](https://en.wikipedia.org/wiki/Perforce). How to choose?

Well, we don't have that much money lying around, so even though Perforce probably is the gold standard for Unreal Engine development, we can't really use it. They have a free tier, but their 5-user limit is a no-go for us. Same for Plastic SCM, which is also more oriented towards Unity, which actually bought the company, in what I think is a very good move on their part.

So, we're left with Git or SVN. Git notoriously doesn't work well with binary content, which we have *a lot* of. However, the [Git LFS](https://git-lfs.github.com/) extension brings better support on that front. SVN, though declining, is still liked in the game-engine world for its simplicity, native binary and file-locking support.

In the end, we chose Git. As a team, all our existing version control is already done through Git. It's the tool that people are most familiar with. And if, like us, you need an external host, there's really not much alternative.

# Which host?

When you have a 150GB project that you want to host on Git, where do you do it?

The best solution would probably be a self-hosted [GitLab](https://gitlab.com/) instance. However, our IT service has no plan of hosting a GitLab, and if I wanted to do that, I'd be left more or less alone. It's doable, but I don't really want to maintain that.

What's out there? [GitHub](https://github.com/), cloud-based GitLab or [Azure Repos](https://azure.microsoft.com/en-us/services/devops/repos/). Each have their limitation

* GitHub has a 1GB of storage and 1GB of bandwith limit for LFS, which you can extend by buying [data packs](https://docs.github.com/en/github/setting-up-and-managing-billing-and-payments-on-github/upgrading-git-large-file-storage#purchasing-additional-storage-and-bandwidth-for-an-organization)
* GitLab has a 10GB per repo size limit
* Azure has a 5-user limit

Not ideal in any case. We chose GitLab, as its limitations are the easiest ones to work with.

# In practice

We have a 150GB project, of mostly binary assets, which we want to version using Git LFS on GitLab, have a fork for every experimentation, and be accessible for users that don't know what Git is. How do we manage that?

## Setup

Our main repository is what we call a [superproject](https://en.wikibooks.org/wiki/Git/Submodules_and_Superprojects), as it's mostly a container of [submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules). Since we have well over a hundred assets from the Marketplace, we put each one in a submodule. Thankfully, nearly all of them are under 10GB in size, so they fit within GitLab's limits. We also have submodules for each plugins we use (around a dozen), and for each [scene](/making-a-scene) we create. Not only does it solve the 10GB issues, it's also cleaner to have each of those in its own repository, as it makes for a better and clearer version log on each. Instead of having 10 commits that modify wildly different assets on a single repo, we have 10 repos, each having a single commit.

One the client side, we recommend researchers to use [TortoiseGit](https://tortoisegit.org/), which has a nice Windows shell integration, making it easy to use. We also wrote some shell scripts that can do some of the more complex tasks (e.g. merging upstream from a fork).

## The bad

Now for the bad. Being used to version control for C++ or Python based projects, I have very disappointed by how this particular subject is handled in Unreal Engine. Compared to everything else in the engine, version control is so painful to work with on a day to day basis.

There's no official Git support from Unreal Engine, but there's an [official plugin](https://github.com/SRombauts/UE4GitPlugin) that's used by most Git users. If you read through the features, it looks great and seem like it would fit most needs. But the reality is different.

The plugin doesn't handle submodules. So right off the bat, it's pretty much useless for us, as we have a superproject that's just submodules. But just by having the plugin enabled, most in-editor actions (e.g. file save, move) take a significant amount of time longer, so much that I now only enable the plugin when  I really need it (e.g. [Collections](https://docs.unrealengine.com/en-US/Basics/ContentBrowser/UserGuide/Collections/index.html)).

Since every file in Unreal Engine is in binary, diffing/merging is a huge pain. There's a [diffing tool](https://www.unrealengine.com/en-US/blog/diffing-blueprints?sessionInvalidated=true) for Blueprints, which *mostly* works, but will sometimes tell you that there's no difference between two revisions, leaving you wondering what could have possibly changed. And it's just for Blueprints, so you can't diff levels, materials, UI, or anything that you'd really like to be able to diff.

And beyond diffing, there's no merging tool! If you're used to some variant of [Gitflow worklow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow), you better hope that there's no conflict when merging branches, otherwise you're in for a headache. Having dealt with a few, I now try my best at avoiding them.

People often mentions [file locking](https://github.com/git-lfs/git-lfs/wiki/File-Locking) as a (well, *the*) solution to avoid conflicts, which is also why people in this world like SVN. I haven't tried it yet, but from what I've read, file locking in LFS isn't that great, mostly due to the decentralized nature of Git. And for our cases, I'm really not sure that file-locking would help across branches or even forks.

Gitlab: MR on forks :(
Diffing BP is meh
Merging BP is :'(
LFS on forks :/
LFS on forks with submodules
LFS lock with branches/forks?
Forking submodules?

Hoping GitLab doesn't change their offer

Really hope for something from Epic in the near future. Why no text?
