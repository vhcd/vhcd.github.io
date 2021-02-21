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

So, we're left with Git or SVN. Git notoriously doesn't work well with binary content, which we have *a lot* of. However, the [Git LFS](https://git-lfs.github.com/) extension brings better support on that front. SVN, though declining, is still liked in the game-engine world for its simplicity and out-of-the-box binary support.

In the end, we chose Git. As a team, all our existing version control is already done through Git. It's the tool that people are most familiar with. And if, like us, you need an external host, there's really not much alternative.

# Which host?

# In practice: the bad

Plugin + submodules :(
Gitlab: MR on forks :(
Plugin is sloooooow
Diffing BP is meh
Merging BP is :'(
Working with binary is :'(
LFS lock with branches/forks?

Really hope for something from Epic in the near future.
