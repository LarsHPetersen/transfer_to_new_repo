# Transfer to new repo

First, we have to fetch all of the remote branches and tags from the existing repository to our local index:

```bash
git fetch origin
```

But even if all branches and tags have been fetched and exist in a local index, we still won’t have a copy of them that is physically local. And a local copy is required to migrate the repository.

We can check for any missing branches that we need to create a local copy of. Let’s list all existing branches (local and remote) to see whether we are missing any copies locally:

```bash
git branch -a

* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/dev
  remotes/origin/main
```

We can easily tell from this output whether we have local copies of all remote branches: The remote ones are prefixed with the remotes/origin/ path, and the local ones aren’t. So, only our main branch is local, whereas remotes/origin/dev are not. That’s OK — let’s just create local copies:

```bash
git checkout -b develop origin/dev
```

After creating local copies of everything, we can verify once again whether all branches with the remotes/origin/ prefix have corresponding local copies (shown without the prefix):

```bash
git branch -a

  dev
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/dev
  remotes/origin/main
```

Now we know for sure that all branches in our repository are stored locally, and we are ready to move the repository to a new host.

At this point, let’s assume we have an SSH-cloned URL of our new repository. If not, create a new repository, and then you will have its SSH-cloned URL.

Let’s use the SSH-cloned URL of our new repository to create a new remote in our existing local repository by executing the following command:

```bash
git remote add new-origin git@github.com:user/repo.git
```

This will give us two remotes for the existing repository: 
the original one (named origin, connected to the existing remote host) and a new one (named new-origin, connected to the new host).

(Git has this concept of “remotes,” which are simply URLs to other copies of your repository. 
The name origin refers to the original remote repository; by convention, it is considered the primary centralized repository.)

Now we are ready to push all local branches and tags to the new remote named new-origin. 
We could push each local branch separately, but pushing all branches at once would be much faster by executing the following Git command:

```bash
git push --all new-origin
```

Let’s also push all of the tags to new-origin with this Git command:

```bash
git push --tags new-origin
```

We’re almost done. Let’s make new-origin the default remote, which will direct all future commits to the new repository that we have just pushed to. To do this, remove the old repository remote:

```bash
git remote rm origin
```

And rename new-origin to just origin, so that it becomes the default remote:

```bash
git remote rename new-origin origin
```
