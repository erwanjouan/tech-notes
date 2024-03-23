![type:video](https://www.youtube.com/embed/oQNpMSyge84)

Goal : Reconstruct all the questions that people face

1. Make sure you scope as many thing as you can.
<scope>test<scope>
so that the dependency is not transitive to the end user.
default is compile

Symptoms for dependency issues:
- NoSuchMethodError
- NoSuchFieldError
- NoClassDefFoundError
These errors are not recoverable.

First thing to narrow down issue.
```
mvn dependency:tree
```
this is the resolved tree, when Maven has resolved all the conflicts.
```
mvn dependency:tree -Dverbose
```
Maven gives the ability to isolate a perticular group (e.g: org.springframework.boot)
```
mvn dependency:tree -Dverbose -Dincludes=org.springframework.boot
```
There can be plenty of versions but we need to select only one.
Because the classpath is a list of jar that are loaded, and only the first instance of a class is loaded.
Maven says that the nearest depency wins.
If you mention  the dependency in pom file:
- direct dependency (level 1)
    - dependency (level 2)

Maven goes down to the whole tree and find the nearest. It does not pick the most recent one, because it can break the backwark compatibility.
You need to understand what Maven does to pick the right version.

**Maven enforcer plugin**

Add the plugin in the plugins
```
mvn verify
```
will list all the dependencies

How to get rid of unwanted dependencies ?
- Use the term <exclusions> in <dependency> to remove transitive dependency.
- Use dependency management, which is a lookup table where you can define some global definition. Dependency in <dependencyManagement> are not yet used by the project, it does not pull the artifact in your project, whereas  <dependencies> does. In case of conflicts, Maven will pickup the version in <dependencyManagement>. It is clear when doing -Dverbose. BTW never add SNAPSHOT in dependency management.

Check [Google Best Practices for Java Libraries](https://jlbp.dev/)
JLBP-7 = backward compatibilty 
JLBP-10

What to do if upper version break lower version ?
- Maven shade plugin with relocation property, you relocate a given package to another package.
- Classloaders. All maven plugin have their own classloader.
- OSGI 

If you make breaking change, change your group.
org.apache.common.lang, org.apache.common.lang3

If a library has changed its artifactid, Maven may add both artifactid in the classpath and collision will occur.
We don't want to have duplicated class.

Avoid having a package split in different modules.
