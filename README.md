# MPS Maven Deployer

A Gradle script to automate the deployment of JetBrains MPS libraries into a Maven repository (either local or
remote), in a format that can be used with [`mps-maven-plugin`](https://github.com/JetBrains/mps-maven-plugin).

# Usage

1. Download and install [JetBrains MPS](http://www.jetbrains.com/mps/).
2. Clone `maven-mps-deployer`.
3. In the root directory of the project run the following command:

   ```sh
   ./gradlew publishToMavenLocal -PmpsHome=<MPS installation directory>
   ```

If you want to deploy MPS artifacts to a non-local Maven repository, run `gradlew publish`:

   ```sh
   ./gradlew publish
       -PmpsHome=<MPS installation directory>
       -PrepositoryUrl=http://myrepo
       -PrepositoryUsername=joe
       -PrepositoryPassword=secret
   ```

(all on one line). The `repositoryUsername` and `repositoryPassword` parameters are optional.

# Deployed Artifacts

The deployer creates two artifacts from an MPS installation. Both artifacts have `groupId` of `org.jetbrains.mps` so
you can find them under `$HOME/.m2/repository/org/jetbrains/mps` after deployment.

1. MPS core libraries and third-party libraries installed with MPS that the plugin depends on to be able to run (from
   `$mpsHome/lib`) are all packaged together into `mps-libs.jar`, and `MPS-src.zip` is attached as the source JAR
   to the artifact.
2. MPS languages and solutions are packaged into a module with `mar` extension (for Module ARchive).

Some JARs are packaged into both artifacts since they are used by the plugin as plain Java libraries and also by MPS
as modules (solutions).
