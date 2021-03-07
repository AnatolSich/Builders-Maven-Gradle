# Build the project with Maven

**The POM**
This project's core POM is:

 ```sh
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>builders</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <artifactId>junit</artifactId>
            <groupId>junit</groupId>
            <scope>test</scope>
            <version>4.12</version>
        </dependency>
    </dependencies>

    <properties>
        <java.version>1.11</java.version>
        <maven.compiler.version>3.8.1</maven.compiler.version>
    </properties>

    <modules>
    <module>admin</module>
    <module>services</module>
    <module>utils</module>
    <module>web</module>
    </modules>

    <packaging>pom</packaging>

    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.5.1</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin><plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.0</version>
            </plugin>
            </plugins>
        </pluginManagement>
    </build>


</project>
 ```  
 
**Build the Project**
 ```sh
mvn package
 ```
 The command line will print out various actions, and end with the following:
 ```sh
 ...
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  9.990 s
[INFO] Finished at: 2021-03-07T18:06:42+02:00
[INFO] ------------------------------------------------------------------------

 ```
 
# Running Tests

You can use mvn test to run unit test in Maven. Few examples :

```sh
# Run all the unit test classes.
$ mvn test

# Run a single test class.
$ mvn -Dtest=TestApp1 -DfailIfNoTests=false test

# Run multiple test classes.
$ mvn -Dtest=TestApp1,TestApp2 -DfailIfNoTests=false test

# Run a single test method from a test class.
$ mvn -Dtest=TestApp1#methodname test

# Run all test methods that match pattern 'testHello*' from a test class.
$ mvn -Dtest=TestApp1#testHello* test

# Run all test methods match pattern 'testHello*' and 'testMagic*' from a test class.
$ mvn -Dtest=TestApp1#testHello*+testMagic* test
 ```


# Build the project with Gradle

**Review settings.gradle**
```sh
rootProject.name = 'builders'
include 'admin'
include 'services'
include 'utils'
include 'web'
 ``` 

Our build contains 4 subprojects that represents the application. It is configured in the build.gradle file:

```sh

apply plugin: 'java'
apply plugin: 'idea'
apply plugin: 'war'


repositories {
    mavenLocal()
    mavenCentral()
}

    dependencies {
        testCompile 'junit:junit:4.12'
        compile group: 'javax.servlet', name: 'javax.servlet-api', version: '3.0.1'
    }

    dependencies {
        compile project(':admin')
        compile project(':web')
    }

jar {
    baseName = 'admin-jar'
    version =  '0.0.1'
}
war {
    from 'web'
    webInf { from 'webapp/WEB-INF' }
    classpath fileTree('libs')
    webXml = file('/web/src/main/resources/web_config.properties')
}

test {
  useJUnit()
  useJUnitPlatform()

  minHeapSize = "128m"
  maxHeapSize = "512m"


  jvmArgs '-XX:MaxPermSize=256m'


  beforeTest { descriptor ->
     logger.lifecycle("Running test: " + descriptor)
  }

  failFast = true

  onOutput { descriptor, event ->
     logger.lifecycle("Test: " + descriptor + " produced standard out/err: " + event.message )
  }
}

```

**Build the project**
 Run:
 ```sh
 gradle build
 ``` 
 Also you can run each module separately using 
  ```sh
 gradle :<module_name>:build
 ``` 
 
  # Running Tests
 
 Fully-qualified name pattern
Prior to 4.7 or if the pattern doesn’t start with an uppercase letter, Gradle treats the pattern as fully-qualified. So if you want to use the test class name irrespective of its package, you would use --tests *.SomeTestClass. Here are some more examples:
 ```sh
# specific class
gradle test --tests org.gradle.SomeTestClass

# specific class and method
gradle test --tests org.gradle.SomeTestClass.someSpecificMethod

# method name containing spaces
gradle test --tests "org.gradle.SomeTestClass.some method containing spaces"

# all classes at specific package (recursively)
gradle test --tests 'all.in.specific.package*'

# specific method at specific package (recursively)
gradle test --tests 'all.in.specific.package*.someSpecificMethod'

gradle test --tests '*IntegTest'

gradle test --tests '*IntegTest*ui*'

gradle test --tests '*ParameterizedTest.foo*'

# the second iteration of a parameterized test
gradle test --tests '*ParameterizedTest.*[2]'
Note that the wildcard '*' has no special understanding of the '.' package separator. It’s purely text based. So --tests *.SomeTestClass will match any package, regardless of its 'depth'.

 ```
You can also combine filters defined at the command line with continuous build to re-execute a subset of tests immediately after every change to a production or test source file. The following executes all tests in the 'com.mypackage.foo' package or subpackages whenever a change triggers the tests to run:
```sh
gradle test --continuous --tests "com.mypackage.foo.*"
 ```
