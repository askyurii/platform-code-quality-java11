# Platform code quality for java11

## Contents

### Includes the following source code analyzer configs

1. [checkstyle](https://checkstyle.sourceforge.io) a source code analyzer config
2. [spotbugs](https://spotbugs.github.io)
3. [PMD](https://pmd.github.io) a source code analyzer config

### Other configs

1. Apache [versions](https://www.mojohaus.org/versions-maven-plugin) plugin config to auto update
	 dependencies

## Usage

### Checkstyle

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

#### Which versions to use?

	<maven-checkstyle-plugin.version>3.1.2</maven-checkstyle-plugin.version>
	<maven-checkstyle-plugin.checkstyle.rules.version>8.43</maven-checkstyle-plugin.checkstyle.rules.version>

#### What are required properties

	<maven-checkstyle-plugin.version/>
	<maven-checkstyle-plugin.skip/>
	<maven-checkstyle-plugin.failOnViolation/>
	<maven-checkstyle-plugin.logViolationsToConsole/>
	<maven-checkstyle-plugin.checkstyle.rules.version/>
	<platform-code-quality-java11.version/>

#### How to skip?

Add to maven properties

	<maven-checkstyle-plugin.skip>true<maven-checkstyle-plugin.skip>

#### How to suppress warnings?

1. You can use ```java.lang.SuppressWarnings``` annotation


	@SuppressWarnings({"EmptyLineSeparator", "SingleSpaceSeparator"})

2. You can ignore all checkstyle rules for a block of code with comment filter


	//CHECKSTYLE:OFF SingleSpaceSeparator
	ignored code here 
	//CHECKSTYLE:ON SingleSpaceSeparator

#### Where can I find the results of the scan?

Your can check the following path for production
code: ```<project>/target/checkstyle/checkstyle-result.xml```

And for test code: ```<project>/target/checkstyle/checkstyle-test.xml```

### SpotBugs

#### How to include?

	<plugin>
		<groupId>com.github.spotbugs</groupId>
		<artifactId>spotbugs-maven-plugin</artifactId>
		<version>${spotbugs-maven-plugin.version}</version>
		<configuration>
			<skip>${spotbugs-maven-plugin.skip}</skip>
			<effort>Max</effort>
			<threshold>Low</threshold>
			<failOnError>true</failOnError>
			<spotbugsXmlOutput>true</spotbugsXmlOutput>
			<spotbugsXmlOutputDirectory>${project.build.directory}/spotbugs</spotbugsXmlOutputDirectory>
			<excludeFilterFile>/spotbug/exclude-find-bugs-filter.xml</excludeFilterFile>
			<spotbugsXmlOutputFilename>source-check.xml</spotbugsXmlOutputFilename>
			<includeTests>true</includeTests>
			<plugins>
				<plugin>
					<groupId>com.h3xstream.findsecbugs</groupId>
					<artifactId>findsecbugs-plugin</artifactId>
					<version>${spotbugs-maven-plugin.spotbugs.findsecbugs-plugin.version}</version>
				</plugin>
			</plugins>
		</configuration>
		<executions>
			<execution>
				<id>spotbugs-check</id>
				<phase>pre-integration-test</phase>
				<goals>
					<goal>check</goal>
				</goals>
			</execution>
		</executions>
		<dependencies>
			<dependency>
				<groupId>com.bondarenko.platform</groupId>
				<artifactId>platform-code-quality-java11</artifactId>
				<version>${platform-code-quality-java11.version}</version>
			</dependency>
			<dependency>
				<groupId>com.github.spotbugs</groupId>
				<artifactId>spotbugs</artifactId>
				<version>${spotbugs-maven-plugin.spotbugs.version}</version>
			</dependency>
		</dependencies>
	</plugin>

#### Which versions to use?

	<spotbugs-maven-plugin.spotbugs.version>4.2.3</spotbugs-maven-plugin.spotbugs.version>
	<spotbugs-maven-plugin.spotbugs.findsecbugs-plugin.version>1.10.1</spotbugs-maven-plugin.spotbugs.findsecbugs-plugin.version>

#### What are required properties

	<spotbugs-maven-plugin.version/>
	<spotbugs-maven-plugin.skip/>
	<spotbugs-maven-plugin.spotbugs.findsecbugs-plugin.version/>
	<spotbugs-maven-plugin.spotbugs.version/>
	<platform-code-quality-java11.version/>

#### How to skip?

	<spotbugs-maven-plugin.skip>true</spotbugs-maven-plugin.skip>

#### How to suppress warnings?

1. You can use ```edu.umd.cs.findbugs.annotations.SuppressFBWarnings``` annotation


	@SuppressFBWarnings("CRLF_INJECTION_LOGS")
	@PostMapping("/logout")
	void logout(@UserIdentifier UserId userId) { 
		log.info("logout '{}'", userId);
	}

Can be imported from

	<dependency>
		<groupId>com.github.spotbugs</groupId>
		<artifactId>spotbugs-annotations</artifactId>
		<scope>provided</scope>
	</dependency>

#### Where can I find the results of the scan?

Your can check the following path for production code and
tests: ```<project>/target/spotbugs/source-check.xml```

### PMD

#### How to include?

	<plugin>
		<artifactId>maven-pmd-plugin</artifactId>
		<version>${maven-pmd-plugin.version}</version>
		<configuration>
			<skip>${maven-pmd-plugin.skip}</skip>
			<linkXRef>true</linkXRef>
			<sourceEncoding>${project.build.sourceEncoding}</sourceEncoding>
			<minimumTokens>60</minimumTokens>
			<targetJdk>${project.java.version}</targetJdk>
			<printFailingErrors>${maven-pmd-plugin.printFailingErrors}</printFailingErrors>
			<failOnViolation>${maven-pmd-plugin.failOnViolation}</failOnViolation>
			<outputDirectory>${project.build.directory}/pmd</outputDirectory>
			<targetDirectory>${project.build.directory}/pmd</targetDirectory>
			<includeTests>true</includeTests>
			<rulesets>
				<ruleset>/pmd/ruleset.xml</ruleset>
			</rulesets>
		</configuration>
		<executions>
			<execution>
				<phase>pre-integration-test</phase>
				<goals>
					<goal>check</goal>
				<goal>cpd-check</goal>
				</goals>
			</execution>
		</executions>
		<dependencies>
			<dependency>
				<groupId>net.sourceforge.pmd</groupId>
				<artifactId>pmd-core</artifactId>
				<version>${maven-pmd-plugin.pmd-rules.version}</version>
			</dependency>
			<dependency>
				<groupId>net.sourceforge.pmd</groupId>
				<artifactId>pmd-java</artifactId>
				<version>${maven-pmd-plugin.pmd-rules.version}</version>
			</dependency>
			<dependency>
				<groupId>net.sourceforge.pmd</groupId>
				<artifactId>pmd-xml</artifactId>
				<version>${maven-pmd-plugin.pmd-rules.version}</version>
			</dependency>
			<dependency>
				<groupId>com.bondarenko.platform</groupId>
				<artifactId>platform-code-quality-java11</artifactId>
				<version>${platform-code-quality-java11.version}</version>
			</dependency>
		</dependencies>
	</plugin>

#### Which versions to use?

	<maven-pmd-plugin.version>3.13.0</maven-pmd-plugin.version>
	<maven-pmd-plugin.failOnViolation>true</maven-pmd-plugin.failOnViolation>
	<maven-pmd-plugin.printFailingErrors>true</maven-pmd-plugin.printFailingErrors>
	<maven-pmd-plugin.pmd-rules.version>6.35.0</maven-pmd-plugin.pmd-rules.version>

#### What are required properties

	<maven-pmd-plugin.version/>
	<maven-pmd-plugin.skip/>
	<maven-pmd-plugin.printFailingErrors/>
	<maven-pmd-plugin.failOnViolation/>
	<maven-pmd-plugin.pmd-rules.version/>
	<platform-code-quality-java11.version/>

#### How to skip?

	<maven-pmd-plugin.skip>true</maven-pmd-plugin.skip>

#### How to suppress warnings?

You can use ```java.lang.SuppressWarnings``` annotation

	@SuppressWarnings("PMD.AvoidFileStream")
	void login(@UserIdentifier UserId userId) throws FileNotFoundException {
		new FileInputStream(userId.getUsername());
		log.info("login '{}'", userId);
	}

#### Where can I find the results of the scan?

Your can check the following path for production code and tests: ```<project>/target/pmd/pmd.html```

To check duplicates report: ```<project>/target/pmd/cpd.html```
