# MPS Maven Deployer

A Gradle script to automate the deployment of JetBrains MPS into a Maven repository (either local or remote).

# Usage

1. Clone `maven-mps-deployer`.
2. In the root directory of the project run the following command:

   ```sh
   ./gradlew publishToMavenLocal -PmpsVersion=<MPS version>
   ```

   where `<MPS version>` is a two- or three-component version number, such as `3.4` or `3.4.1`.
If you want to deploy MPS artifacts to a non-local Maven repository, run `gradlew publish`:

   ```sh
   ./gradlew publish
       -PmpsVersion=<MPS version>
       -PrepositoryUrl=http://myrepo
       -PrepositoryUsername=joe
       -PrepositoryPassword=secret
   ```

(all on one line). The `repositoryUsername` and `repositoryPassword` parameters are optional.

# Deployed Artifacts

The deployer downloads the official MPS distribution in `zip` format from JetBrains and repackages it into two
artifacts. Both artifacts have `groupId` of `org.jetbrains.mps` so you can find them under
`$HOME/.m2/repository/org/jetbrains/mps` in the local repository after deployment.

1. `org.jetbrains.mps:mps`: the whole MPS archive is repackaged to include the contents of its top-level directory
   directly into the archive.

   For example, the `MPS-3.4.1.zip` as downloaded from JetBrains contains a top-level directory named `MPS 3.4` which
   contains MPS files and directories (such as `lib`, `plugins`, or `languages`). Having the top-level directory with
   a version-dependent name is impractical for automation, so a new archive is created to include the `MPS 3.4`
   directory contents (the above mentioned `lib`, `plugins`, `languages`, etc.) directly.

   The structure of this artifact is stable and should not change unless MPS packaging changes drastically.
   
2. `org.jetbrains.mps:mps-libs`: certain MPS libraries are packaged together into `mps-libs.jar`. This artifact is
   intended for internal use by `mps-maven-plugin` only. Specifically, it provides MPS API for (a part of)
   `mps-maven-plugin`. This artifact is not required to _run_ the plugin since at runtime the plugin uses the JARs from
   `org.jetbrains.mps:mps`. As such, the artifact's internal structure may change and it may even be removed at some
   point.