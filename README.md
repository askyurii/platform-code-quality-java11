# Platform code quality for java11

## Contents
### Includes the following source code analyzer configs
1. [checkstyle](https://checkstyle.sourceforge.io) a source code analyzer config
2. [spotbugs](https://spotbugs.github.io)
3. [PMD](https://pmd.github.io) a source code analyzer config

### Other configs
1. Apache [versions](https://www.mojohaus.org/versions-maven-plugin) plugin config to auto update dependencies

## Usage

## Checkstyle

#### How to include?

    <plugin>
        <artifactId>maven-checkstyle-plugin</artifactId>
        <version>${maven-checkstyle-plugin.version}</version>
        <configuration>
            <encoding>${project.build.sourceEncoding}</encoding>
            <consoleOutput>true</consoleOutput>
            <failsOnError>true</failsOnError>
            <failOnViolation>${maven-checkstyle-plugin.failOnViolation}</failOnViolation>
            <logViolationsToConsole>${maven-checkstyle-plugin.logViolationsToConsole}</logViolationsToConsole>
            <violationSeverity>warning</violationSeverity>
            <linkXRef>true</linkXRef>
            <skip>${maven-checkstyle-plugin.skip}</skip>
        </configuration>
        <executions>
            <execution>
                <id>checkstyle-validate</id>
                <phase>validate</phase>
                <goals>
                    <goal>check</goal>
                </goals>
                <configuration>
                    <sourceDirectories>${project.build.sourceDirectory}</sourceDirectories>
                    <includeTestSourceDirectory>false</includeTestSourceDirectory>
                    <configLocation>/checkstyle/checkstyle.xml</configLocation>
                    <outputFile>${project.build.directory}/checkstyle/checkstyle-result.xml</outputFile>
                    <cacheFile>${project.build.directory}/checkstyle/checkstyle-cache</cacheFile>
                </configuration>
            </execution>
            <execution>
                <id>checkstyle-validate-test</id>
                <phase>validate</phase>
                <goals>
                    <goal>check</goal>
                </goals>
                <configuration>
                    <sourceDirectories/>
                    <testSourceDirectories>${project.build.testSourceDirectory}</testSourceDirectories>
                    <includeTestSourceDirectory>true</includeTestSourceDirectory>
                    <configLocation>/checkstyle/checkstyle-test.xml</configLocation>
                    <outputFile>${project.build.directory}/checkstyle/checkstyle-result-test.xml</outputFile>
                    <cacheFile>${project.build.directory}/checkstyle/checkstyle-cache-test</cacheFile>
                </configuration>
            </execution>
        </executions>
        <dependencies>
            <dependency>
                <groupId>com.puppycrawl.tools</groupId>
                <artifactId>checkstyle</artifactId>
                <version>${maven-checkstyle-plugin.checkstyle.rules.version}</version>
            </dependency>
            <dependency>
                <groupId>com.bondarenko.platform</groupId>
                <artifactId>platform-code-quality-java11</artifactId>
                <version>${platform-code-quality-java11.version}</version>
            </dependency>
        </dependencies>
    </plugin>

#### How to skip?
Add to maven properties

    <maven-checkstyle-plugin.skip>true<maven-checkstyle-plugin.skip>

#### Which rules to use?
The latest checked rules

    <maven-checkstyle-plugin.checkstyle.rules.version>8.43</maven-checkstyle-plugin.checkstyle.rules.version>

#### How to suppress warnings?
1. You can use a ```java.lang.SuppressWarnings```


    @SuppressWarnings({"EmptyLineSeparator", "SingleSpaceSeparator"})

2. You can ignore all checkstyle rules for a block of code with comment filter


    //CHECKSTYLE:OFF SingleSpaceSeparator
    ignored code here
    //CHECKSTYLE:ON SingleSpaceSeparator

#### Where can I find the results of the scan?
Your can check the following path for production code: ```<project>/target/checkstyle/checkstyle-result.xml```

And for test code: ```<project>/target/checkstyle/checkstyle-test.xml``` 

## SpotBugs



## PMD
