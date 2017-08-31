# Scala IDE profiles

This pom project provides profiles to be able to compile a Scala IDE plugin. After setting this project as the `parent`, enabling the right profiles configures your plugin project to be compatible with Scala IDE.

**Pay special attention to the Scala IDE versions, all profiles are not compatible with all versions**

* profiles for Scala versions
  * scala-2.12.x
* profiles for Scala IDE releases
  * scala-ide-stable (stable releases)
  * scala-ide-dev (milestones)
  * scala-ide-nightly (nightly builds)
* profiles for Eclipse releases
  * eclipse-oxygen

In addition, you can *sign* your builds and you get a uniform version qualifier `('${version.tag}-${version.suffix}-'yyyyMMddHHmm'-${buildNumber}')`.

# Usage

For example, this `pom.xml` is all that's needed to get a build that works against any Scala version, Scala IDE and Eclipse combination:

    <?xml version="1.0" encoding="UTF-8"?>
    <project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <modelVersion>4.0.0</modelVersion>

      <parent>
        <groupId>org.scala-ide</groupId>
        <artifactId>plugin-profiles</artifactId>
        <version>1.0.14</version>
      </parent>

      <groupId>org.scala-ide</groupId>
      <artifactId>org.scala-ide.play2.build</artifactId>
      <version>0.2.0-SNAPSHOT</version>
      <name>Play 2 Support for Scala IDE</name>
      <packaging>pom</packaging>

      <modules>
        <module>org.scala-ide.play2</module>
        <module>org.scala-ide.play2.tests</module>
        <module>org.scala-ide.play2.feature</module>
        <module>org.scala-ide.play2.source.feature</module>
        <module>org.scala-ide.play2.update-site</module>
      </modules>

    </project>

Then you can build your project by running:

```
mvn -Peclipse-oxygen,scala-ide-stable,scala-2.12.x clean package
```

If you want to sign your build, just add the right jarsigner properties:

```
mvn -Dtycho.localArtifacts=ignore \
    -Peclipse-oxygen,scala-ide-stable,scala-2.12.x \
    -Dversion.tag='v' \
    -Djarsigner.storepass=*** \
    -Djarsigner.keypass=*** \
    -Djarsigner.keystore=/path/to/your.keystore \
    clean package
```
## Create a build to be integrated in the Scala IDE ecosystem repositories

To make a build compatible with the tools creating the Scala IDE ecosystem repositories, an additionnal step is required before the actual build step. Run the following command to correctly set the versions of the Scala and Scala IDE dependencies:

```
mvn -Peclipse-oxygen,scala-ide-stable,scala-2.12.x -Pset-versions -Dtycho.style=maven --non-recursive exec:java

```

# Release a new version (of plugin-profiles)

If you have fixes to this pom, you should publish a new version to
Sonatype. Follow the
[OSS guide](http://central.sonatype.org/pages/ossrh-guide.html). If
everything is configured correctly, releasing should be entirely
managed by maven:

```
$ mvn release:clean
$ mvn release:prepare
$ mvn release:perform
```

There's an additional manual step:

- go to [Sonatype](http://central.sonatype.org/pages/releasing-the-deployment.html) and inspect and release the staging repository.
