
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

## Dynamicly design for test cases
### Browser Setup
create a package under src/test/java and under the package(Utilites) create a class(BrowserSetup)
![folders](https://user-images.githubusercontent.com/45315685/217946279-cc28e9f0-a8f6-472c-9857-68763b119d1d.PNG)

This code contain to setup browser and manage window for generate testing in automated way

 ```ruby
package Utilities;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.edge.EdgeDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.testng.annotations.AfterSuite;
import org.testng.annotations.BeforeSuite;

import io.github.bonigarcia.wdm.WebDriverManager;


public class BrowserSetup {
	private static String browserName = System.getProperty("browser","chrome");
	private static final ThreadLocal<WebDriver> Driverlocal = new ThreadLocal<>();//to focus individual 
	public static WebDriver getDriver() {
		return Driverlocal.get();
	}
	public static void setDriver(WebDriver driver) { 
		//filename.threadlocalname.set()
		BrowserSetup.Driverlocal.set(driver);
	}
	public static WebDriver getBrowser(String browserName) {
		switch(browserName.toLowerCase()){
		case "chrome":
				WebDriverManager.chromedriver().setup();
				return new ChromeDriver();
		case "Edge":
				WebDriverManager.edgedriver().setup();
				return new EdgeDriver();
		case "firefox":
				WebDriverManager.firefoxdriver().setup();
				return new FirefoxDriver();
		default:
			throw new RuntimeException("Browser not found");
		}	
	}
	@BeforeSuite
	public static synchronized void setBrowser() {
		WebDriver webdriver = getBrowser(browserName);
		webdriver.manage().window().maximize();
		setDriver(webdriver);
	}
	@AfterSuite
	public static synchronized void quitBrowser() {
		getDriver().quit();
	}
	
}
 ``` 
 ![BrowserSetup](https://user-images.githubusercontent.com/45315685/217946859-7ee679bc-5655-49c1-88cf-78dfd6a66b87.PNG)


### Pages and testcases
#### Pages
create a package under src/test/java and under the package(Pages) create some classes which will contain page related objects and methods to generate testcases dynamicly
![foldersPages](https://user-images.githubusercontent.com/45315685/217947006-22a99141-f664-4b6c-9cc8-48ebcc45c905.PNG)

It will be easier to understand if we named the class as page name or feature we will be testing for
#### BasePage : contain methods(findElement,click,wait, write , allureScreenshot)
 ```ruby
 package pages;

import static Utilities.BrowserSetup.getDriver;

import java.io.ByteArrayInputStream;
import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import io.qameta.allure.Allure;
public class BasePage{
	public WebElement getElement(By locator) {
		return getDriver().findElement(locator);
	}
	public void clickOnElement(By locator) {
		getElement(locator).click();
	}
	public String getElementText(By locator) {
		return getElement(locator).getText();
	}
	public void clickonWaitelement(By locator) {
		WebDriverWait waitElement = new WebDriverWait(getDriver(),Duration.ofSeconds(20));
		WebElement  element= waitElement.until(ExpectedConditions.elementToBeClickable(locator));
		element.click();	
	}
	public void write(By locator, String text) {
		getElement(locator).sendKeys(text);
	}
	public void TakeScreenShot(String name) {
		Allure.addAttachment(name, new ByteArrayInputStream(((TakesScreenshot)getDriver()).getScreenshotAs(OutputType.BYTES)));
	}

}
 ``` 
 ![BasePage](https://user-images.githubusercontent.com/45315685/217947122-bdcd77dd-f911-48d3-843c-dc9ab08e3b42.PNG)


 ### Extends BasePage classes
 #### LogInPage 
login page location, login form fillup with valid or invalid credentials
![loginPage](https://user-images.githubusercontent.com/45315685/217947563-f6bcf3ee-994c-46e8-a240-8ae3e3841132.PNG)


```ruby
package pages;

import org.openqa.selenium.By;

public class LogInPage extends BasePage{
	public String LOGINPAGETITLE ="Daraz.com.bd: Online Shopping Bangladesh - Mobiles, Tablets, Home Appliances, TV, Audio &amp; More";
	public By emailfield = By.xpath("//input[@type='text']");
	public By passField =By.xpath("//input[@type='password']");
	public By loginButton =By.xpath("//button[contains(text(),'LOGIN')]");
	public void loginFormFillup(String email, String pass) {
		write(emailfield, email);
		write(passField,pass);
		TakeScreenShot("Invalid Inputs");

		clickOnElement(loginButton);
		TakeScreenShot("Invalid input outcome");
	}

}
 ``` 
>>pic
 #### DarazHomePage 
 loginbutton location 
 ![HomePage](https://user-images.githubusercontent.com/45315685/217947653-a699edbf-c625-4d7b-a7ec-b5eca9f3caf3.PNG)

```ruby
package pages;

import org.openqa.selenium.By;

public class DarazHomePage extends BasePage{
	public By LOGINSIGNUPBUTTON = By.xpath("//a[contains(text(),'Signup / Login')]");

	
	public void clickLogin(){
		clickOnElement(LOGINSIGNUPBUTTON);
		TakeScreenShot("login");
	}

}
 ``` 

#### HelpCenterPage
CustomerCare to HelpCenter page
![Helpcenter](https://user-images.githubusercontent.com/45315685/217948061-74200a15-cf5d-4ccf-b655-37612e051aa9.PNG)

```ruby
package pages;

import org.openqa.selenium.By;


public class HelpCenter extends BasePage{
	public By CustomerCareOption = By.xpath("//span[contains(text(),'CUSTOMER CARE')]");
	public By HelpCenterPage = By.xpath("//a[contains(text(),'Help Center')]");
	public String helpCenterPageTitle ="Daraz.com.bd: Online Shopping Bangladesh - Mobiles, Tablets, Home Appliances, TV, Audio &amp; More";
	public void helpCenter() {
		clickOnElement(CustomerCareOption);
		TakeScreenShot("Homepage Customer Care Option");
		clickonWaitelement(HelpCenterPage);
		TakeScreenShot("HelpcenterPage");

		
	}
	
}


 ``` 


