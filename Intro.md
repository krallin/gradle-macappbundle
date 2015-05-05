# Introduction #

The gradle-macappbundle plugin creates Mac OSX .app applications from within Gradle. The runSetFile and createDmg tasks will probably only run on under OSX as they use the SetFile and hdiutil applications.

# Oracle versus Apple Java #

The way java in installed on the Mac has changed significantly over the past couple of OSX releases. Apple is no longer maintaining the OSX version of java, with 1.6 being the last JVM directly from Apple. All future releases, starting with java 1.7 for the Mac will come from Oracle. In addition, Oracle Java wants a slightly different layout in the .app. One major change is that now a .app can bundle the JRE inside the .app, eliminating the need for the user to separately install java, although at the significant increase in the size of the .app. The macappbundle plugin version is now 2.0 since this is a pretty major change to the internals of the .app. Oracle style .apps are now the default, but Apple style can still be created. It is anticipated that the Apple style will be removed at a future date.

# Tasks #

There are 13 tasks, but most users will only use createApp, createDmg or CreateAppZip:
  * configMacApp - configures defaults
  * generatePlist - creates the Info.plist file
  * generatePkgInfo - create the PkgInfo file
  * copyToResourcesJava - copies the jars into the .app
  * copyStub - copies the stub used to start Java to the .app
  * copyIcon - copies the .icon file to the .app
  * runSetFile - runs SetFile to toggle the magic bit (not run by default)
  * bundleJRE - bundles the Oracle JRE in the .app (optional)
  * createApp - empty task that depends on the above
  * codeSign - digital signature using codesign
  * copyBackgroundImage - copies a background image to the .dmg
  * createDmg - creates a .dmg disk image containing the app
  * createAppZip - create a .zip file containing the .app for users that cannot create a .dmg because they are not using OSX

An example configuration within your build.gradle for Gradle 2.1 or later might look like:

```

plugins {
  id "edu.sc.seis.macAppBundle" version "2.1.0"
}

macAppBundle {
    mainClassName = "com.example.myApp.Start"
    icon = "myIcon.icns"
    bundleJRE = true
    javaProperties.put("apple.laf.useScreenMenuBar", "true")
    backgroundImage = "doc/macbackground.png"
}

```

# Configuration #

The configuration for the tasks is done via the Extension mechanism in Gradle. The [MacAppBundlePluginExtension](http://code.google.com/p/gradle-macappbundle/source/browse/src/main/groovy/edu/sc/seis/gradle/macAppBundle/MacAppBundlePluginExtension.groovy) source code shows the list of items that can be configured. Most map in a straightforward way into the values used in the Mac OSX Application bundle. See https://java.net/projects/appbundler for Oracle style apps and [Apple's documentation](https://developer.apple.com/library/mac/#documentation/Java/Conceptual/Java14Development/03-JavaDeployment/JavaDeployment.html#//apple_ref/doc/uid/TP40001885-SW1) for more information on the Apple .app java directory structure.