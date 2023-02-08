
# Daraz-Web-Site (Automation testing with Allure Report)

### Goals

Setting up a browser that will contain the testing process

Our primary goal is to validate the login/signup option on the Daraz home page, as well as test the login page with invalid input to see if it returns a proper response.

Because the Customer Care option on the homepage takes some time (delay) to show the Help Center option, the next goal is to access the route to get to the Help Center page


## Maven file Setup
Create maven project and setup pomfile

### pom.xml

Change or add dependencies

 ```ruby
   <dependencies>     <!-- https://mvnrepository.com/artifact/org.testng/testng -->
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.6.1</version>
    <scope>test</scope>
</dependency>
<!-- https://mvnrepository.com/artifact/org.seleniumhq.selenium/selenium-java -->
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.6.0</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.github.bonigarcia/webdrivermanager -->
<dependency>
    <groupId>io.github.bonigarcia</groupId>
    <artifactId>webdrivermanager</artifactId>
    <version>5.3.0</version>
</dependency>
<dependency>
			<groupId>io.qameta.allure</groupId>
			<artifactId>allure-testng</artifactId>
			<version>2.19.0</version>
		</dependency>
	</dependencies>
 ``` 
 add plugins
 ```ruby
 <build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-surefire-plugin</artifactId>
				<version>3.0.0-M7</version>
					<configuration>
						<includes>
							<include>${testfile}</include><!--Cmd te file name dibo Terminal a likhbo[mvn test -Dtestfile="logInTest.java"]-->
						</includes>
					</configuration>
				<!-- <configuration>
					<suiteXmlFiles>
						<suiteXmlFile>${testXmlfile}</suiteXmlFile>
					</suiteXmlFiles>
				</configuration>-->
			</plugin>

		</plugins>
	</build>
 
 ``` 

 Overall pom.xml file
 ```ruby
 <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.bd</groupId>
  <artifactId>Daraz23jan</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>Daraz23jan</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
	<maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <dependencies>     <!-- https://mvnrepository.com/artifact/org.testng/testng -->
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.6.1</version>
    <scope>test</scope>
</dependency>
<!-- https://mvnrepository.com/artifact/org.seleniumhq.selenium/selenium-java -->
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.6.0</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.github.bonigarcia/webdrivermanager -->
<dependency>
    <groupId>io.github.bonigarcia</groupId>
    <artifactId>webdrivermanager</artifactId>
    <version>5.3.0</version>
</dependency>
<dependency>
			<groupId>io.qameta.allure</groupId>
			<artifactId>allure-testng</artifactId>
			<version>2.19.0</version>
		</dependency>
	</dependencies>
	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-surefire-plugin</artifactId>
				<version>3.0.0-M7</version>
					<configuration>
						<includes>
							<include>${testfile}</include><!--Cmd te file name dibo Terminal a likhbo[mvn test -Dtestfile="logInTest.java"]-->
						</includes>
					</configuration>
				<!-- <configuration>
					<suiteXmlFiles>
						<suiteXmlFile>${testXmlfile}</suiteXmlFile>
					</suiteXmlFiles>
				</configuration>-->
			</plugin>

		</plugins>
	</build>
</project>
 ``` 

