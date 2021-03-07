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
