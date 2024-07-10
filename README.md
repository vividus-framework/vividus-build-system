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

<details>
  <summary><h2>Migrating from <code>1.0</code> to <code>2.0</code></h2></summary>

  Build System 1.0 is not maintained anymore and is going to be retired. All users are recommended to migrate to Build System 2.0.

  ### How to switch to Build System 2.0
  1. Sync Build System:
      - [Internal](#option-1-internal-recommened): follow the [guide above](#update-from-the-vividus-build-system-remote).
      - [External](#option-2-external): just pull the latest updates with Git client of your choice.
  1. Open `gradle.properties` located in the root directory of the project with tests.
  1. Update the property to point `2.0`:
      ```properties
      buildSystemVersion=2.0
      ```
  1. Build the project.

  ### List of breaking changes
  This is the list of breaking changes between major versions `1.0` and `2.0`. It should help to successfully migrate
  existing automated tests and infrastructre for them to use Build System 2.0.

  1. Java 17+ is required: make sure to update all environments (local, CI, etc.) where tests are developed and run to use
  [Java 17](https://adoptium.net/temurin/releases/?version=17) or higher.<br/>Also it's needed to check the IDE used to develop tests supports Java 17 or higher.
  1. [VIVIDUS Artifactory](https://vividuscentral.jfrog.io/artifactory/releases) is removed from the list of repositories
  where dependencies are downloaded from.

      Most likely this change won't affect any users, because VIVIDUS Artifactory was used as a backup storage of VIVIDUS
      releases up to `0.4.5` version. The primary storage is GitHub Packages.

  1. Correcness of the file with known issues configuration (`known-issues.json`) is checked at test project build phase.
  1. There is no longer a requirement to define the extra property `ext.buildSystemDir`. The mentioned property can be
  safely removed from the `build.gradle` file located in the test project root directory.

  <details>
    <summary>The following list conatins breaking changes that may affect people who develop own VIVIDUS plugins and modules.</summary>

    1. [SonarQube Gradle plugin](https://plugins.gradle.org/plugin/org.sonarqube) is not added by default anymore.

        SonarQube is a tool which is used to track code quality, it doesn't have VIVIDUS support (yet :)), so there is no
        reason to apply it to all projects. If you use SonarQube for your modules, you should manage such integrations on
        your side. You can find example of simple migration [here](https://github.com/vividus-framework/vividus/commit/b216a5801ac181bfa59794e87ebfa909fe191da3).
    1. [VIVIDUS Artifactory](https://vividuscentral.jfrog.io/artifactory/releases) is removed from the list of repositories
    where dependencies are downloaded from.

        If you use VIVIDUS Artifactory as a caching proxy to download dependencies, then you should configure repositories
        storing the required dependencies on your side.

    1. `**/*.min.js` files are removed from exclusions of static code check ensuring that `https://` is used for everything.

        Fine-tuned exclusions for custom files should be done at the project level.
  </details>
</details>

## Migrating from `2.0` to `3.0`

### How to switch to Build System 3.0
1. Sync Build System:
    - [Internal](#option-1-internal-recommened): follow the [guide above](#update-from-the-vividus-build-system-remote).
    - [External](#option-2-external): just pull the latest updates with Git client of your choice.
1. Open `gradle.properties` located in the root directory of the project with tests.
1. Update the property to point `3.0`:
    ```properties
    buildSystemVersion=3.0
    ```
1. Build the project.

### List of breaking changes

This is the list of breaking changes between major versions `2.0` and `3.0`. It should help to successfully migrate
existing automated tests and infrastructre for them to use Build System 3.0.

1. Java 21+ is required: make sure to update all environments (local, CI, etc.) where tests are developed and run to use
[Java 21](https://adoptium.net/temurin/releases/?version=21) or higher.
1. The ability to specify custom prefix for the version of the built artifact is removed. Previously it was possible to
set custom prefix using project property `versionPrefix`, but this feature was not customizable and was not popular.
1. The task `validateKnownIssues` is not included in the project build phase anymore. The correcness of the file with
known issues configuration (`known-issues.json`) is checked in scope of the task `testVividusInitialization`.
