# MPS Maven Deployer
A Gradle script to automate the deployment of JetBrains MPS libraries into a Maven repository (either local or
remote), in a format that can be used with [`mps-maven-plugin`](https://github.com/JetBrains/mps-maven-plugin).

# Usage

1. Download and install [JetBrains MPS](http://www.jetbrains.com/mps/).
2. Clone `maven-mps-deployer`.
3. In the root directory of the project run the following command:
   ```sh
   ./gradlew publishToMavenLocal -DmpsHome=<MPS installation directory>
   ```

Additionally, if you want to deploy MPS artifacts to a non-local Maven repository, follow these steps:
1. Edit `build.gradle`, uncomment the `repositories` section inside the `publishing` block and fill in your repository
   details there.
2. Run `./gradlew publish`.

# Deployed Artifacts
The deployer creates several artifacts from an MPS installation. All artifacts have `groupId` of `org.jetbrains.mps` so
you can find them under `$HOME/.m2/repository/org/jetbrains/mps` after deployment.

1. Third-party libraries installed with MPS. These are libraries like GNU Trove, dom4j or ASM. They, too, are packaged
   under `org.jetbrains.mps` group because there may be modifications made to them.
2. MPS core libraries (from `MPS_HOME/lib`) - these are packaged similarly to the third-party libraries, but
   additionally `MPS-src.zip` is attached as a source JAR to each artifact.
3. MPS language "groups" (currently `languageDesign`, `baseLanguage`, and `make`). To simplify dependency management
   the languages belonging to one "group" are packaged together into one JAR of JARs, with `.lar` extension (for
   "Language ARchive").
