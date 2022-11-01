# VIVIDUS Build System
VIVIDUS Build System is a set of [Gradle](https://gradle.org/) scripts and configuration files that are used across different VIVIDUS-based projects and allow to avoid copy-pasting of boilerplate scripts.

## Usage
### Option 1: Internal (Recommened)
The build system can be added as [Git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules) to the root of the project with tests. In this case no extra configuration is needed.

#### Update from the VIVIDUS Build System Remote
To update the internal build system run the following command from the test project root:
```sh
git submodule update --remote vividus-build-system
```
Then the changes can be staged, committed and pushed to a remote repository, for example:

```sh
git add vividus-build-system
git commit -m "Update VIVIDUS build system to the latest version"
git push [<repository> [<refspec>…]]
```

#### Pull the updated version of the VIVIDUS Build System
In case if someone followed the above instructions: updated the internal build system and pushed the changes, the other team members may see the following status after test project pulling:

<pre>
<b>(main) » git pull</b>
Updating bb45687..825283f
Fast-forward
 README.md            | 81 +++------------------------------------------------------------------------------
 build.gradle         |  2 +-
 vividus-build-system |  2 +-
 3 files changed, 5 insertions(+), 80 deletions(-)

<b>(main*) » git status</b>
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        <b>modified:   vividus-build-system (new commits)</b>

no changes added to commit (use "git add" and/or "git commit -a")
</pre>

By default, the `git pull` command recursively fetches submodules changes, as we can see in the output of the first command above. However, it does not update the submodules. This is shown by the output of the `git status` command, which shows the submodule is “modified”, and has “new commits”. To finalize the update, you need to run `git submodule update`:

<pre>
<b>(main*) » git submodule update</b>
Submodule path 'vividus-build-system': checked out '1914c3ec0d14cb771d01245e5b0d66cd58d4e5a8'

<b>(main) » git status</b>
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
</pre>

More details of how to automate this process can be found in the official [Git guide](https://git-scm.com/book/en/v2/Git-Tools-Submodules#_pulling_upstream_changes_from_the_project_remote).

### Option 2: External
The build system can be cloned as a regular Git repository to any location. After that new environment variable `VIVIDUS_BUILD_SYSTEM_HOME` should be created with an absoulute path of just cloned repository root.

### Conflicts Resolution
:information_source: When both internal and external build systems are available, the external one will be used.
