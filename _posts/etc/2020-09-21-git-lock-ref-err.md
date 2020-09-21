---
layout: post
title: Git Cannot Lock Ref 에러 해결
excerpt: "git"
tags: [programming, git]
permalink: /os/:year/:month/:day/:title/
category : Etc
---

git pull을 하려고 했는데 아래와 같은 에러가 발생.  
error: cannot lock ref 'refs/remotes/origin/{branch_name}': 'refs/remotes/origin/{branch_prefix}' exists; cannot create 'refs/remotes/origin/{branch_name}'

git gc와 git remote 명령어를 통해 해결하였다.  

<pre class="prettyprint">
$ git gc --prune=now
$ git remote prune origin
</pre>

Git GC와 Git Remote의 Manual
```
GIT-GC(1)                                                                                               Git Manual                                                                                              GIT-GC(1)

NAME
       git-gc - Cleanup unnecessary files and optimize the local repository

SYNOPSIS
       git gc [--aggressive] [--auto] [--quiet] [--prune=<date> | --no-prune] [--force]

DESCRIPTION
       Runs a number of housekeeping tasks within the current repository, such as compressing file revisions (to reduce disk space and increase performance) and removing unreachable objects which may have been created
       from prior invocations of git add.

       Users are encouraged to run this task on a regular basis within each repository to maintain good disk space utilization and good operating performance.

       Some git commands may automatically run git gc; see the --auto flag below for details. If you know what you’re doing and all you want is to disable this behavior permanently without further considerations, just
       do:

           $ git config --global gc.auto 0

OPTIONS
       --aggressive
           Usually git gc runs very quickly while providing good disk space utilization and performance. This option will cause git gc to more aggressively optimize the repository at the expense of taking much more
           time. The effects of this optimization are persistent, so this option only needs to be used occasionally; every few hundred changesets or so.

       --auto
           With this option, git gc checks whether any housekeeping is required; if not, it exits without performing any work. Some git commands run git gc --auto after performing operations that could create many
           loose objects.

           Housekeeping is required if there are too many loose objects or too many packs in the repository. If the number of loose objects exceeds the value of the gc.auto configuration variable, then all loose
           objects are combined into a single pack using git repack -d -l. Setting the value of gc.auto to 0 disables automatic packing of loose objects.

           If the number of packs exceeds the value of gc.autoPackLimit, then existing packs (except those marked with a .keep file) are consolidated into a single pack by using the -A option of git repack. Setting
           gc.autoPackLimit to 0 disables automatic consolidation of packs.

       --prune=<date>
           Prune loose objects older than date (default is 2 weeks ago, overridable by the config variable gc.pruneExpire). --prune=all prunes loose objects regardless of their age and increases the risk of corruption
           if another process is writing to the repository concurrently; see "NOTES" below. --prune is on by default.

       --no-prune
           Do not prune any loose objects.

       --quiet
           Suppress all progress reports.

       --force
           Force git gc to run even if there may be another git gc instance running on this repository.

CONFIGURATION
       The optional configuration variable gc.reflogExpire can be set to indicate how long historical entries within each branch’s reflog should remain available in this repository. The setting is expressed as a
       length of time, for example 90 days or 3 months. It defaults to 90 days.

       The optional configuration variable gc.reflogExpireUnreachable can be set to indicate how long historical reflog entries which are not part of the current branch should remain available in this repository.
       These types of entries are generally created as a result of using git commit --amend or git rebase and are the commits prior to the amend or rebase occurring. Since these changes are not part of the current
       project most users will want to expire them sooner. This option defaults to 30 days.

       The above two configuration variables can be given to a pattern. For example, this sets non-default expiry values only to remote-tracking branches:

           [gc "refs/remotes/*"]
                   reflogExpire = never
                   reflogExpireUnreachable = 3 days

       The optional configuration variable gc.rerereResolved indicates how long records of conflicted merge you resolved earlier are kept. This defaults to 60 days.

       The optional configuration variable gc.rerereUnresolved indicates how long records of conflicted merge you have not resolved are kept. This defaults to 15 days.

       The optional configuration variable gc.packRefs determines if git gc runs git pack-refs. This can be set to "notbare" to enable it within all non-bare repos or it can be set to a boolean value. This defaults to
       true.

       The optional configuration variable ‘gc.aggressiveWindow` controls how much time is spent optimizing the delta compression of the objects in the repository when the --aggressive option is specified. The larger
       the value, the more time is spent optimizing the delta compression. See the documentation for the --window’ option in git-repack(1) for more details. This defaults to 250.

       Similarly, the optional configuration variable gc.aggressiveDepth controls --depth option in git-repack(1). This defaults to 50.

       The optional configuration variable gc.pruneExpire controls how old the unreferenced loose objects have to be before they are pruned. The default is "2 weeks ago".

NOTES
       git gc tries very hard not to delete objects that are referenced anywhere in your repository. In particular, it will keep not only objects referenced by your current set of branches and tags, but also objects
       referenced by the index, remote-tracking branches, refs saved by git filter-branch in refs/original/, or reflogs (which may reference commits in branches that were later amended or rewound). If you are
       expecting some objects to be deleted and they aren’t, check all of those locations and decide whether it makes sense in your case to remove those references.

       On the other hand, when git gc runs concurrently with another process, there is a risk of it deleting an object that the other process is using but hasn’t created a reference to. This may just cause the other
       process to fail or may corrupt the repository if the other process later adds a reference to the deleted object. Git has two features that significantly mitigate this problem:

        1. Any object with modification time newer than the --prune date is kept, along with everything reachable from it.

        2. Most operations that add an object to the database update the modification time of the object if it is already present so that #1 applies.

       However, these features fall short of a complete solution, so users who run commands concurrently have to live with some risk of corruption (which seems to be low in practice) unless they turn off automatic
       garbage collection with git config gc.auto 0.

HOOKS
       The git gc --auto command will run the pre-auto-gc hook. See githooks(5) for more information.

SEE ALSO
       git-prune(1) git-reflog(1) git-repack(1) git-rerere(1)

GIT
       Part of the git(1) suite

Git 2.17.1  
```


```
GIT-REMOTE(1)                                                                                           Git Manual                                                                                          GIT-REMOTE(1)

NAME
       git-remote - Manage set of tracked repositories

SYNOPSIS
       git remote [-v | --verbose]
       git remote add [-t <branch>] [-m <master>] [-f] [--[no-]tags] [--mirror=<fetch|push>] <name> <url>
       git remote rename <old> <new>
       git remote remove <name>
       git remote set-head <name> (-a | --auto | -d | --delete | <branch>)
       git remote set-branches [--add] <name> <branch>...
       git remote get-url [--push] [--all] <name>
       git remote set-url [--push] <name> <newurl> [<oldurl>]
       git remote set-url --add [--push] <name> <newurl>
       git remote set-url --delete [--push] <name> <url>
       git remote [-v | --verbose] show [-n] <name>...
       git remote prune [-n | --dry-run] <name>...
       git remote [-v | --verbose] update [-p | --prune] [(<group> | <remote>)...]

DESCRIPTION
       Manage the set of repositories ("remotes") whose branches you track.

OPTIONS
       -v, --verbose
           Be a little more verbose and show remote url after name. NOTE: This must be placed between remote and subcommand.

COMMANDS
       With no arguments, shows a list of existing remotes. Several subcommands are available to perform operations on the remotes.

       add
           Adds a remote named <name> for the repository at <url>. The command git fetch <name> can then be used to create and update remote-tracking branches <name>/<branch>.

           With -f option, git fetch <name> is run immediately after the remote information is set up.

           With --tags option, git fetch <name> imports every tag from the remote repository.

           With --no-tags option, git fetch <name> does not import tags from the remote repository.

           By default, only tags on fetched branches are imported (see git-fetch(1)).

           With -t <branch> option, instead of the default glob refspec for the remote to track all branches under the refs/remotes/<name>/ namespace, a refspec to track only <branch> is created. You can give more
           than one -t <branch> to track multiple branches without grabbing all branches.

           With -m <master> option, a symbolic-ref refs/remotes/<name>/HEAD is set up to point at remote’s <master> branch. See also the set-head command.

           When a fetch mirror is created with --mirror=fetch, the refs will not be stored in the refs/remotes/ namespace, but rather everything in refs/ on the remote will be directly mirrored into refs/ in the local
           repository. This option only makes sense in bare repositories, because a fetch would overwrite any local commits.

           When a push mirror is created with --mirror=push, then git push will always behave as if --mirror was passed.

       rename
           Rename the remote named <old> to <new>. All remote-tracking branches and configuration settings for the remote are updated.

           In case <old> and <new> are the same, and <old> is a file under $GIT_DIR/remotes or $GIT_DIR/branches, the remote is converted to the configuration file format.

       remove, rm
           Remove the remote named <name>. All remote-tracking branches and configuration settings for the remote are removed.

       set-head
           Sets or deletes the default branch (i.e. the target of the symbolic-ref refs/remotes/<name>/HEAD) for the named remote. Having a default branch for a remote is not required, but allows the name of the
           remote to be specified in lieu of a specific branch. For example, if the default branch for origin is set to master, then origin may be specified wherever you would normally specify origin/master.

           With -d or --delete, the symbolic ref refs/remotes/<name>/HEAD is deleted.

           With -a or --auto, the remote is queried to determine its HEAD, then the symbolic-ref refs/remotes/<name>/HEAD is set to the same branch. e.g., if the remote HEAD is pointed at next, "git remote set-head
           origin -a" will set the symbolic-ref refs/remotes/origin/HEAD to refs/remotes/origin/next. This will only work if refs/remotes/origin/next already exists; if not it must be fetched first.

           Use <branch> to set the symbolic-ref refs/remotes/<name>/HEAD explicitly. e.g., "git remote set-head origin master" will set the symbolic-ref refs/remotes/origin/HEAD to refs/remotes/origin/master. This
           will only work if refs/remotes/origin/master already exists; if not it must be fetched first.

       set-branches
           Changes the list of branches tracked by the named remote. This can be used to track a subset of the available remote branches after the initial setup for a remote.

           The named branches will be interpreted as if specified with the -t option on the git remote add command line.

           With --add, instead of replacing the list of currently tracked branches, adds to that list.

       get-url
           Retrieves the URLs for a remote. Configurations for insteadOf and pushInsteadOf are expanded here. By default, only the first URL is listed.

           With --push, push URLs are queried rather than fetch URLs.

           With --all, all URLs for the remote will be listed.

       set-url
           Changes URLs for the remote. Sets first URL for remote <name> that matches regex <oldurl> (first URL if no <oldurl> is given) to <newurl>. If <oldurl> doesn’t match any URL, an error occurs and nothing is
           changed.

           With --push, push URLs are manipulated instead of fetch URLs.

           With --add, instead of changing existing URLs, new URL is added.

           With --delete, instead of changing existing URLs, all URLs matching regex <url> are deleted for remote <name>. Trying to delete all non-push URLs is an error.

           Note that the push URL and the fetch URL, even though they can be set differently, must still refer to the same place. What you pushed to the push URL should be what you would see if you immediately fetched
           from the fetch URL. If you are trying to fetch from one place (e.g. your upstream) and push to another (e.g. your publishing repository), use two separate remotes.

       show
           Gives some information about the remote <name>.

           With -n option, the remote heads are not queried first with git ls-remote <name>; cached information is used instead.

       prune
           Deletes stale references associated with <name>. By default, stale remote-tracking branches under <name> are deleted, but depending on global configuration and the configuration of the remote we might even
           prune local tags that haven’t been pushed there. Equivalent to git fetch --prune <name>, except that no new references will be fetched.

           See the PRUNING section of git-fetch(1) for what it’ll prune depending on various configuration.

           With --dry-run option, report what branches will be pruned, but do not actually prune them.

       update
           Fetch updates for a named set of remotes in the repository as defined by remotes.<group>. If a named group is not specified on the command line, the configuration parameter remotes.default will be used; if
           remotes.default is not defined, all remotes which do not have the configuration parameter remote.<name>.skipDefaultUpdate set to true will be updated. (See git-config(1)).

           With --prune option, run pruning against all the remotes that are updated.

DISCUSSION
       The remote configuration is achieved using the remote.origin.url and remote.origin.fetch configuration variables. (See git-config(1)).

EXAMPLES
       ·   Add a new remote, fetch, and check out a branch from it

               $ git remote
               origin
               $ git branch -r
                 origin/HEAD -> origin/master
                 origin/master
               $ git remote add staging git://git.kernel.org/.../gregkh/staging.git
               $ git remote
               origin
               staging
               $ git fetch staging
               ...
               From git://git.kernel.org/pub/scm/linux/kernel/git/gregkh/staging
                * [new branch]      master     -> staging/master
                * [new branch]      staging-linus -> staging/staging-linus
                * [new branch]      staging-next -> staging/staging-next
               $ git branch -r
                 origin/HEAD -> origin/master
                 origin/master
                 staging/master
                 staging/staging-linus
                 staging/staging-next
               $ git checkout -b staging staging/master
               ...

       ·   Imitate git clone but track only selected branches

               $ mkdir project.git
               $ cd project.git
               $ git init
               $ git remote add -f -t master -m master origin git://example.com/git.git/
               $ git merge origin

SEE ALSO
       git-fetch(1) git-branch(1) git-config(1)

GIT
       Part of the git(1) suite

Git 2.17.1                        
```




