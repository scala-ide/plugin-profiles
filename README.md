# Scala IDE profiles

This pom project provides profiles to be able to compile a Scala IDE plugin. After setting this project as the `parent`, enabling the right profiles configures your plugin project to be compatible with Scala IDE.

**Pay special attention to the Scala IDE versions, all profiles are not compatible with all versions**

* profiles for Scala versions
    * scala-2.12.x
    * scala-2.11.x (Scala IDE version 3.0.4 -> ..)
    * scala-2.10.x (Scala IDE version 3.0.0 -> 4.0.0-m2)
    * scala-2.9.x (Scala IDE version 3.0.0)
* profiles for Scala IDE releases
  * scala-ide-stable (stable releases)
  * scala-ide-dev (milestones)
  * scala-ide-nightly (nightly builds)
* profiles for Eclipse releases
  * eclipse-indigo (Scala IDE versions 2.0.0 -> 3.0.1)
  * eclipse-juno (Scala IDE versions 3.0.0 -> 4.0.0-m2)
  * eclipse-kepler (Scala IDE versions 4.0.0-m3 -> ..)
  * eclipse-luna (Scala IDE versions 4.0.0-m2 -> ..)

In addition, you can *sign* your builds and you get a uniform version qualifier `('${version.tag}-${version.suffix}-'yyyyMMddHHmm'-${buildNumber}')`.

# Usage

For example, this `pom.xml` is all that's needed to get a build that works against any Scala version, Scala IDE and Eclipse combination:

    <?xml version="1.0" encoding="UTF-8"?>
    <project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <modelVersion>4.0.0</modelVersion>
      <prerequisites>
        <maven>3.0</maven>
      </prerequisites>

      <parent>
        <groupId>org.scala-ide</groupId>
        <artifactId>plugin-profiles</artifactId>
        <version>1.0.0</version>
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

Now you can build your project by running

```
mvn -Peclipse-indigo,scala-ide-stable,scala-2.10.x clean package
```

If you want to sign your build, just add the right jarsigner properties:

```
mvn -Dtycho.localArtifacts=ignore \
    -P eclipse-indigo,scala-ide-stable,scala-2.10.x \
    -Dversion.tag='v' \
    -Djarsigner.storepass=*** \
    -Djarsigner.keypass=*** \
    -Djarsigner.keystore=/path/to/your.keystore \
    clean package
```

## Release a new version

If you have fixes to this pom, you should publish a new version to
Sonatype. Follow the
[OSS guide](https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide#SonatypeOSSMavenRepositoryUsageGuide-7e.DeployandStagewithSBT). If
everything is configured correctly, releasing should be entirely
managed by maven:

```
$ mvn release:clean
$ mvn release:prepare
$ mvn release:perform
```
