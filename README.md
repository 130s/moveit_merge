# MoveIt! Merge - Migration Notes

All of the git history and the current three supported branches (indigo|jade|kinetic) will be migrated to the single repo [https://github.com/ros-planning/moveit](https://github.com/ros-planning/moveit) for the following repos:

 - https://github.com/ros-planning/moveit_core
 - https://github.com/ros-planning/moveit_ros
 - https://github.com/ros-planning/moveit_planners
 - https://github.com/ros-planning/moveit_plugins
 - https://github.com/ros-planning/moveit_setup_assistant
 - https://github.com/ros-planning/moveit_commander
 - https://github.com/ros-planning/moveit_ikfast
 - https://github.com/ros-planning/moveit_resources
 - https://github.com/ros-planning/moveit_experimental

As of **7am Pacific, Friday August 5th** (subject to be pushed back if necessary) these repos will be fully deprecated and all code changes / PRs should be created or moved to the single *moveit* repo.

The rational for this migration is further discussed on [Discourse](http://discourse.ros.org/t/migration-to-one-github-repo-for-moveit/266).

## Developers: HOW YOU CAN HELP US

1. Sync / rebase your individual forks now to the current (soon to be deprecated) repos. This way you will have fewer potential conflicts the day we switch over
2. Change over to the unified repo the same day/week that we do to reduce future conflicts
3. Close out all open pull requests in the old repo now. Avoid opening new complex PRs that might take a while to get merged in.
4. Review these migration notes and leave feedback on [Discourse](http://discourse.ros.org/t/migration-to-one-github-repo-for-moveit/266).

## Merge Method

> Note: the following section is subject to change. See [alternate method](http://discourse.ros.org/t/migration-to-one-github-repo-for-moveit/266/22)

We used a common ``git merge`` method that was loosely inspired by [this blog](https://saintgimp.org/2013/01/22/merging-two-git-repositories-into-one-repository-without-losing-file-history/) to preserve the git history of all repos. The automated merge script we used can be found [here](https://github.com/davetcoleman/moveit_merge/blob/master/git_merge_moveit.sh).

Unfortunately on Github we will not be able to see the git history for files - this is a limitation of Github and has been discussed online. In fact it is also a limitation of git - ``git`` has no actual file moving capabailites but instead detects changes automatically similar to refactoring a single file. Locally you can still see the history, however, using the ``--follow`` command e.g.:

    git log --follow ./moveit/moveit_core/robot_state/src/robot_state.cpp

## Migrating Forks

Many MoveIt! users likely have their own forks with modifications living in their code bases - we want to encourage those users to make the migration with us to a unified repo. To make this easy for developers, we've maintained the old repo folder / package structure so that you can in theory just copy a folder over into the new git repo and do a ``commit``. Before making the copy though we recommend you ``pull`` \ ``rebase`` your old repo to the latest changes on the parent deprecated repository. This way there will the a minimum number of potential conflicts you have to sort through when syncing with the new merged ``moveit`` repo.

In the future we are interested in flattening out some of the subfolders - i.e. "moveit/moveit_ros/planning" could become "moveit/moveit_ros_planning" since this better represents the package name. But this is not part of the initial migration.

## Migrating Issues

We would like to start opening all new issues on the new repo, and old ones should be copied there as well. This will be one of the objectives of World MoveIt! Day.

## Releasing Debians

Future releasing of indgo|jade|kinetic will be from this new unified repo.

## Tags/Releases

We will not be migrating the old git tags / releases to the unified repo due to the difficulty of doing so. See also "Old Repositories"

## Old Repositories

The old repos will remain in existence at least until support for the Indigo LTS ends May 2021. Prominent notes will be made in their READMEs that they are deprecated.

## Large Dependency Chains

For systems that, for example, do not need visualization components we recommend disabling various packages from being built on your platform. There are several ways to do this with catkin:

### Method 1

    touch moveit/moveit_ros/visualization/CATKIN_IGNORE

etc for whatever packages you need to ignore

### Method 2

If you use catkin_tools:

    catkin config --blacklist moveit_ros_visualization

### Method 3

    rm -rf moveit/moveit_ros/visualization

And add to your .gitignore the notice to ignore the deletion.
