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

### Option 2: External
The build system can be cloned as a regular Git repository to any location. After that new environment variable `VIVIDUS_BUILD_SYSTEM_HOME` should be created with an absoulute path of just cloned repository root.

### Conflicts Resolution
:information_source: When both internal and external build systems are available, the external one will be used.
