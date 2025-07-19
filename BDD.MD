# Selenium Automation Testing Mastery Guide

_Complete technical deep-dive into Selenium WebDriver, TestNG, Cucumber BDD with advanced XPath, locator, and wait strategies_

## Table of Contents

- [Selenium WebDriver Fundamentals](#selenium-webdriver-fundamentals)
- [TestNG Integration & Advanced Features](#testng-integration--advanced-features)
- [Cucumber BDD Implementation](#cucumber-bdd-implementation)
- [Complex XPath Strategies](#complex-xpath-strategies)
- [Driver & Locator Strategies](#driver--locator-strategies)
- [Element Wait Strategies](#element-wait-strategies)
- [Data-Driven Testing with Data Providers](#data-driven-testing-with-data-providers)
- [Keyword-Driven Testing Framework](#keyword-driven-testing-framework)

## Selenium WebDriver Fundamentals

### WebDriver Architecture

**Client-Server Architecture:**

- **WebDriver Client**: Your test scripts
- **WebDriver Protocol**: JSON Wire Protocol/W3C WebDriver Protocol
- **Browser Driver**: ChromeDriver, GeckoDriver, EdgeDriver
- **Browser**: Chrome, Firefox, Edge, Safari

### Driver Initialization Best Practices

**WebDriverManager Integration:**

```java
@BeforeMethod
public void setUp() {
    WebDriverManager.chromedriver().setup();
    ChromeOptions options = new ChromeOptions();
    options.addArguments("--disable-notifications");
    options.addArguments("--disable-geolocation");
    options.addArguments("--disable-extensions");
    driver = new ChromeDriver(options);
    driver.manage().window().maximize();
    driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
}
```

**Cross-Browser Compatibility:**

```java
public WebDriver initializeDriver(String browserName) {
    switch (browserName.toLowerCase()) {
        case "chrome":
            WebDriverManager.chromedriver().setup();
            ChromeOptions chromeOptions = new ChromeOptions();
            chromeOptions.addArguments("--headless");
            return new ChromeDriver(chromeOptions);

        case "firefox":
            WebDriverManager.firefoxdriver().setup();
            FirefoxOptions firefoxOptions = new FirefoxOptions();
            firefoxOptions.addArguments("--headless");
            return new FirefoxDriver(firefoxOptions);

        case "edge":
            WebDriverManager.edgedriver().setup();
            return new EdgeDriver();

        default:
            throw new IllegalArgumentException("Browser not supported: " + browserName);
    }
}
```

## TestNG Integration & Advanced Features

### TestNG Annotations Hierarchy

**Execution Order:**

1. `@BeforeSuite`
2. `@BeforeTest`
3. `@BeforeClass`
4. `@BeforeMethod`
5. `@Test`
6. `@AfterMethod`
7. `@AfterClass`
8. `@AfterTest`
9. `@AfterSuite`

### Advanced TestNG Features

**Parallel Execution Configuration:**

```xml

















```

**Test Groups and Dependencies:**

```java
@Test(groups = {"smoke", "regression"})
public void testLogin() {
    // Login test implementation
}

@Test(dependsOnMethods = {"testLogin"}, groups = {"regression"})
public void testDashboard() {
    // Dashboard test - runs only after successful login
}

@Test(groups = {"smoke"}, priority = 1)
public void testHighPriorityFeature() {
    // High priority test
}
```

**Custom Listeners:**

```java
public class CustomTestListener implements ITestListener {

    @Override
    public void onTestFailure(ITestResult result) {
        String testName = result.getMethod().getMethodName();
        WebDriver driver = ((BaseTest) result.getInstance()).getDriver();

        // Screenshot on failure
        takeScreenshot(driver, testName);

        // Log failure details
        System.out.println("Test Failed: " + testName);
        System.out.println("Failure Reason: " + result.getThrowable().getMessage());
    }

    @Override
    public void onTestSkipped(ITestResult result) {
        System.out.println("Test Skipped: " + result.getMethod().getMethodName());
    }
}
```

## Cucumber BDD Implementation

### Feature File Structure

**Best Practices for Feature Files:**

```gherkin
@smoke @login
Feature: User Authentication
  As a registered user
  I want to login to the application
  So that I can access my account features

  Background:
    Given I am on the login page

  @positive
  Scenario: Successful login with valid credentials
    When I enter username "validuser@test.com"
    And I enter password "ValidPass123"
    And I click the login button
    Then I should be redirected to the dashboard
    And I should see welcome message "Welcome back!"

  @negative
  Scenario Outline: Login failure with invalid credentials
    When I enter username ""
    And I enter password ""
    And I click the login button
    Then I should see error message ""

    Examples:
      | username          | password     | error                    |
      | invalid@test.com  | ValidPass123 | Invalid username         |
      | validuser@test.com| wrongpass    | Invalid password         |
      | empty             | ValidPass123 | Username is required     |
```

### Step Definitions Implementation

**Robust Step Definitions:**

```java
public class LoginStepDefinitions {

    private WebDriver driver;
    private LoginPage loginPage;

    @Given("I am on the login page")
    public void i_am_on_the_login_page() {
        driver = DriverManager.getDriver();
        loginPage = new LoginPage(driver);
        driver.get(ConfigManager.getProperty("base.url") + "/login");
        Assert.assertTrue(loginPage.isLoginPageDisplayed(),
                         "Login page is not displayed");
    }

    @When("I enter username {string}")
    public void i_enter_username(String username) {
        if ("empty".equals(username)) {
            loginPage.clearUsername();
        } else {
            loginPage.enterUsername(username);
        }
    }

    @Then("I should see error message {string}")
    public void i_should_see_error_message(String expectedError) {
        String actualError = loginPage.getErrorMessage();
        Assert.assertEquals(actualError, expectedError,
                           "Error message does not match");
    }
}
```

### Hooks and Setup

**Cucumber Hooks Implementation:**

```java
public class Hooks {

    private WebDriver driver;

    @Before
    public void setUp(Scenario scenario) {
        String browserName = System.getProperty("browser", "chrome");
        driver = DriverFactory.initializeDriver(browserName);
        DriverManager.setDriver(driver);

        // Tag-based setup
        Collection tags = scenario.getSourceTagNames();
        if (tags.contains("@mobile")) {
            setupMobileEmulation();
        }
    }

    @After
    public void tearDown(Scenario scenario) {
        if (scenario.isFailed()) {
            // Screenshot on failure
            final byte[] screenshot = ((TakesScreenshot) driver).getScreenshotAs(OutputType.BYTES);
            scenario.attach(screenshot, "image/png", "Screenshot");
        }

        if (driver != null) {
            driver.quit();
            DriverManager.removeDriver();
        }
    }

    @Before("@database")
    public void setUpDatabase() {
        // Database setup for tests requiring database
        DatabaseManager.setupTestData();
    }
}
```

## Complex XPath Strategies

### Advanced XPath Techniques

#### Relative XPath Mastery

**1. Axes-Based Navigation:**

```java
// Following sibling - next element at same level
"//label[text()='Email']/following-sibling::input"

// Preceding sibling - previous element at same level
"//input[@type='password']/preceding-sibling::label"

// Parent navigation
"//span[text()='Delete']/parent::button"

// Ancestor navigation - find any ancestor matching condition
"//td[text()='John']/ancestor::tr[contains(@class,'user-row')]"

// Descendant - find any descendant matching condition
"//div[@class='form-group']//descendant::input[@type='email']"
```

**2. Advanced Text Matching:**

```java
// Exact text match
"//button[text()='Submit Form']"

// Contains text (case-sensitive)
"//div[contains(text(),'Error')]"

// Normalize space for handling whitespace
"//span[normalize-space(text())='Success Message']"

// Text starts with
"//h1[starts-with(text(),'Welcome')]"

// Multiple text conditions
"//div[contains(text(),'Error') and contains(text(),'validation')]"
```

**3. Attribute-Based Complex Selectors:**

```java
// Multiple attribute conditions
"//input[@type='text' and @name='username' and @required='true']"

// Attribute contains value
"//div[contains(@class,'error-message')]"

// Attribute starts with
"//input[starts-with(@id,'user_')]"

// Not contains condition
"//button[not(contains(@class,'disabled'))]"

// Attribute value comparison
"//input[@maxlength>10]"
```

**4. Position-Based Selection:**

```java
// First element of type
"(//div[@class='product-card'])[1]"

// Last element
"(//li[@class='menu-item'])[last()]"

// Position greater than
"//tr[position()>2 and position() attributes) {
    StringBuilder xpath = new StringBuilder("//" + elementType);

    if (!attributes.isEmpty()) {
        xpath.append("[");
        List conditions = new ArrayList<>();

        attributes.forEach((key, value) -> {
            if (value.startsWith("contains:")) {
                conditions.add(String.format("contains(@%s,'%s')",
                    key, value.replace("contains:", "")));
            } else if (value.startsWith("starts:")) {
                conditions.add(String.format("starts-with(@%s,'%s')",
                    key, value.replace("starts:", "")));
            } else {
                conditions.add(String.format("@%s='%s'", key, value));
            }
        });

        xpath.append(String.join(" and ", conditions));
        xpath.append("]");
    }

    return xpath.toString();
}
```

## Driver & Locator Strategies

### Advanced Locator Strategy Framework

**1. Locator Priority Hierarchy:**

```java
public class LocatorStrategy {

    public WebElement findElement(WebDriver driver, String... locators) {
        for (String locator : locators) {
            try {
                return driver.findElement(parseLocator(locator));
            } catch (NoSuchElementException e) {
                // Continue to next locator
            }
        }
        throw new NoSuchElementException("Element not found with any provided locator");
    }

    private By parseLocator(String locator) {
        String[] parts = locator.split(":", 2);
        String type = parts[0].toLowerCase();
        String value = parts[1];

        switch (type) {
            case "id": return By.id(value);
            case "name": return By.name(value);
            case "css": return By.cssSelector(value);
            case "xpath": return By.xpath(value);
            case "class": return By.className(value);
            case "tag": return By.tagName(value);
            case "linktext": return By.linkText(value);
            case "partiallinktext": return By.partialLinkText(value);
            default: throw new IllegalArgumentException("Unsupported locator type: " + type);
        }
    }
}
```

**2. Smart Locator Selection:**

```java
public class SmartLocatorBuilder {

    public List buildLocatorCandidates(String elementDescription) {
        List locators = new ArrayList<>();

        // Priority 1: ID (most reliable)
        if (hasId(elementDescription)) {
            locators.add("id:" + extractId(elementDescription));
        }

        // Priority 2: Name attribute
        if (hasName(elementDescription)) {
            locators.add("name:" + extractName(elementDescription));
        }

        // Priority 3: CSS with specific class
        if (hasUniqueClass(elementDescription)) {
            locators.add("css:." + extractUniqueClass(elementDescription));
        }

        // Priority 4: Relative XPath
        locators.add("xpath:" + buildRelativeXPath(elementDescription));

        // Priority 5: CSS selector combination
        locators.add("css:" + buildCssSelector(elementDescription));

        return locators;
    }
}
```

### Driver Management Patterns

**1. ThreadLocal Driver Management:**

```java
public class DriverManager {

    private static ThreadLocal driverThreadLocal = new ThreadLocal<>();
    private static ThreadLocal browserThreadLocal = new ThreadLocal<>();

    public static void setDriver(WebDriver driver, String browserName) {
        driverThreadLocal.set(driver);
        browserThreadLocal.set(browserName);
    }

    public static WebDriver getDriver() {
        return driverThreadLocal.get();
    }

    public static String getBrowserName() {
        return browserThreadLocal.get();
    }

    public static void quitDriver() {
        WebDriver driver = driverThreadLocal.get();
        if (driver != null) {
            driver.quit();
            driverThreadLocal.remove();
            browserThreadLocal.remove();
        }
    }
}
```

**2. Browser-Specific Capabilities:**

```java
public class BrowserCapabilities {

    public static ChromeOptions getChromeOptions(boolean headless, boolean mobile) {
        ChromeOptions options = new ChromeOptions();

        // Performance optimizations
        options.addArguments("--disable-extensions");
        options.addArguments("--disable-gpu");
        options.addArguments("--disable-dev-shm-usage");
        options.addArguments("--no-sandbox");

        if (headless) {
            options.addArguments("--headless");
        }

        if (mobile) {
            Map mobileEmulation = new HashMap<>();
            mobileEmulation.put("deviceName", "iPhone 12");
            options.setExperimentalOption("mobileEmulation", mobileEmulation);
        }

        // Download preferences
        Map prefs = new HashMap<>();
        prefs.put("download.default_directory", System.getProperty("user.dir") + "/downloads");
        prefs.put("download.prompt_for_download", false);
        options.setExperimentalOption("prefs", prefs);

        return options;
    }

    public static FirefoxOptions getFirefoxOptions(boolean headless) {
        FirefoxOptions options = new FirefoxOptions();

        if (headless) {
            options.addArguments("--headless");
        }

        // Firefox-specific preferences
        FirefoxProfile profile = new FirefoxProfile();
        profile.setPreference("browser.download.folderList", 2);
        profile.setPreference("browser.download.dir", System.getProperty("user.dir") + "/downloads");
        profile.setPreference("browser.helperApps.neverAsk.saveToDisk", "application/pdf,text/csv");

        options.setProfile(profile);

        return options;
    }
}
```

## Element Wait Strategies

### Advanced Wait Implementations

**1. Custom Expected Conditions:**

```java
public class CustomExpectedConditions {

    public static ExpectedCondition textToBePresentInElementValue(
            WebElement element, String text) {
        return driver -> {
            try {
                String elementValue = element.getAttribute("value");
                return elementValue != null && elementValue.contains(text);
            } catch (StaleElementReferenceException e) {
                return false;
            }
        };
    }

    public static ExpectedCondition numberOfElementsToBeLessThan(
            By locator, int number) {
        return driver -> {
            try {
                List elements = driver.findElements(locator);
                return elements.size()  elementToBeClickableIgnoringOverlay(
            WebElement element) {
        return driver -> {
            try {
                if (element.isDisplayed() && element.isEnabled()) {
                    // Check if element is not overlapped
                    Point location = element.getLocation();
                    Dimension size = element.getSize();

                    WebElement elementAtPoint = (WebElement) ((JavascriptExecutor) driver)
                        .executeScript("return document.elementFromPoint(arguments[0], arguments[1]);",
                                     location.getX() + size.getWidth()/2,
                                     location.getY() + size.getHeight()/2);

                    if (element.equals(elementAtPoint)) {
                        return element;
                    }
                }
                return null;
            } catch (Exception e) {
                return null;
            }
        };
    }
}
```

**2. Wait Strategy Framework:**

```java
public class WaitStrategy {

    private WebDriver driver;
    private WebDriverWait wait;
    private static final int DEFAULT_TIMEOUT = 10;

    public WaitStrategy(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(DEFAULT_TIMEOUT));
    }

    public WebElement waitForElementToBeVisible(By locator) {
        return wait.until(ExpectedConditions.visibilityOfElementLocated(locator));
    }

    public WebElement waitForElementToBeClickable(By locator, int timeoutInSeconds) {
        WebDriverWait customWait = new WebDriverWait(driver, Duration.ofSeconds(timeoutInSeconds));
        return customWait.until(ExpectedConditions.elementToBeClickable(locator));
    }

    public boolean waitForTextToAppear(By locator, String text, int timeoutInSeconds) {
        try {
            WebDriverWait customWait = new WebDriverWait(driver, Duration.ofSeconds(timeoutInSeconds));
            return customWait.until(ExpectedConditions.textToBePresentInElementLocated(locator, text));
        } catch (TimeoutException e) {
            return false;
        }
    }

    public void waitForPageLoadComplete() {
        wait.until(driver -> ((JavascriptExecutor) driver)
            .executeScript("return document.readyState").equals("complete"));
    }

    public void waitForAjaxToComplete() {
        wait.until(driver -> ((JavascriptExecutor) driver)
            .executeScript("return jQuery.active == 0"));
    }

    public void waitForAngularToLoad() {
        wait.until(driver -> ((JavascriptExecutor) driver)
            .executeScript("return angular.element(document).injector().get('$http').pendingRequests.length === 0"));
    }
}
```

**3. Fluent Wait Implementation:**

```java
public class FluentWaitHelper {

    public static  T waitUntil(WebDriver driver, Function condition,
                                  int timeoutInSeconds, int pollingIntervalInMillis) {
        FluentWait wait = new FluentWait<>(driver)
            .withTimeout(Duration.ofSeconds(timeoutInSeconds))
            .pollingEvery(Duration.ofMillis(pollingIntervalInMillis))
            .ignoring(NoSuchElementException.class)
            .ignoring(StaleElementReferenceException.class)
            .ignoring(ElementNotInteractableException.class);

        return wait.until(condition);
    }

    public static void waitForElementAttributeToContain(WebDriver driver, WebElement element,
                                                       String attribute, String value) {
        waitUntil(driver,
            d -> element.getAttribute(attribute) != null &&
                 element.getAttribute(attribute).contains(value),
            10, 500);
    }

    public static void waitForElementCountToStabilize(WebDriver driver, By locator,
                                                     int expectedCount) {
        waitUntil(driver,
            d -> d.findElements(locator).size() == expectedCount,
            15, 1000);
    }
}
```

## Data-Driven Testing with Data Providers

### TestNG Data Providers

**1. Basic Data Provider Implementation:**

```java
public class DataProviders {

    @DataProvider(name = "loginData")
    public Object[][] getLoginData() {
        return new Object[][] {
            {"validuser@test.com", "ValidPass123", "success"},
            {"invaliduser@test.com", "ValidPass123", "invalid_user"},
            {"validuser@test.com", "wrongpass", "invalid_password"},
            {"", "ValidPass123", "empty_username"},
            {"validuser@test.com", "", "empty_password"}
        };
    }

    @DataProvider(name = "searchData")
    public Object[][] getSearchData() {
        return new Object[][] {
            {"laptop", 5, "electronics"},
            {"book", 10, "literature"},
            {"phone", 3, "electronics"},
            {"", 0, "no_results"}
        };
    }
}

@Test(dataProvider = "loginData", dataProviderClass = DataProviders.class)
public void testLogin(String username, String password, String expectedResult) {
    LoginPage loginPage = new LoginPage(driver);
    loginPage.enterUsername(username);
    loginPage.enterPassword(password);
    loginPage.clickLogin();

    switch (expectedResult) {
        case "success":
            Assert.assertTrue(loginPage.isDashboardDisplayed());
            break;
        case "invalid_user":
            Assert.assertEquals(loginPage.getErrorMessage(), "Invalid username");
            break;
        case "invalid_password":
            Assert.assertEquals(loginPage.getErrorMessage(), "Invalid password");
            break;
    }
}
```

**2. Excel-Based Data Provider:**

```java
public class ExcelDataProvider {

    @DataProvider(name = "excelData")
    public Object[][] getExcelData() throws IOException {
        String filePath = "src/test/resources/testdata/LoginData.xlsx";
        return ExcelUtils.getTestData(filePath, "LoginTests");
    }
}

public class ExcelUtils {

    public static Object[][] getTestData(String filePath, String sheetName) throws IOException {
        FileInputStream fis = new FileInputStream(filePath);
        XSSFWorkbook workbook = new XSSFWorkbook(fis);
        XSSFSheet sheet = workbook.getSheet(sheetName);

        int rowCount = sheet.getLastRowNum();
        int colCount = sheet.getRow(0).getLastCellNum();

        Object[][] data = new Object[rowCount][colCount];

        for (int i = 1; i  testData = new ArrayList<>();

        try (Connection conn = DatabaseConnection.getConnection()) {
            String query = "SELECT username, password, user_type, status FROM test_users WHERE active = 1";
            PreparedStatement stmt = conn.prepareStatement(query);
            ResultSet rs = stmt.executeQuery();

            while (rs.next()) {
                testData.add(new Object[] {
                    rs.getString("username"),
                    rs.getString("password"),
                    rs.getString("user_type"),
                    rs.getString("status")
                });
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }

        return testData.toArray(new Object[0][]);
    }
}

public class DatabaseConnection {

    public static Connection getConnection() throws SQLException {
        String url = ConfigManager.getProperty("db.url");
        String username = ConfigManager.getProperty("db.username");
        String password = ConfigManager.getProperty("db.password");

        return DriverManager.getConnection(url, username, password);
    }
}
```

## Keyword-Driven Testing Framework

### Framework Architecture

**1. Keyword Definition Structure:**

```java
public class KeywordEngine {

    private WebDriver driver;
    private Map keywords;

    public KeywordEngine(WebDriver driver) {
        this.driver = driver;
        this.keywords = loadKeywords();
    }

    private Map loadKeywords() {
        Map keywordMap = new HashMap<>();
        Method[] methods = this.getClass().getDeclaredMethods();

        for (Method method : methods) {
            if (method.isAnnotationPresent(Keyword.class)) {
                Keyword keyword = method.getAnnotation(Keyword.class);
                keywordMap.put(keyword.value(), method);
            }
        }

        return keywordMap;
    }

    public void executeKeyword(String keywordName, String... parameters) {
        try {
            Method keywordMethod = keywords.get(keywordName);
            if (keywordMethod != null) {
                keywordMethod.invoke(this, (Object[]) parameters);
            } else {
                throw new RuntimeException("Keyword not found: " + keywordName);
            }
        } catch (Exception e) {
            throw new RuntimeException("Error executing keyword: " + keywordName, e);
        }
    }
}

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Keyword {
    String value();
}
```

**2. Keyword Implementation:**

```java
public class WebKeywords extends KeywordEngine {

    public WebKeywords(WebDriver driver) {
        super(driver);
    }

    @Keyword("OPEN_BROWSER")
    public void openBrowser(String browserName) {
        driver = DriverFactory.initializeDriver(browserName);
        DriverManager.setDriver(driver);
    }

    @Keyword("NAVIGATE_TO")
    public void navigateTo(String url) {
        driver.get(url);
    }

    @Keyword("ENTER_TEXT")
    public void enterText(String locator, String text) {
        WebElement element = ElementHelper.findElement(driver, locator);
        element.clear();
        element.sendKeys(text);
    }

    @Keyword("CLICK_ELEMENT")
    public void clickElement(String locator) {
        WebElement element = ElementHelper.waitForElementToBeClickable(driver, locator);
        element.click();
    }

    @Keyword("VERIFY_TEXT")
    public void verifyText(String locator, String expectedText) {
        WebElement element = ElementHelper.findElement(driver, locator);
        String actualText = element.getText();
        Assert.assertEquals(actualText, expectedText,
                           "Text verification failed");
    }

    @Keyword("VERIFY_ELEMENT_PRESENT")
    public void verifyElementPresent(String locator) {
        boolean isPresent = ElementHelper.isElementPresent(driver, locator);
        Assert.assertTrue(isPresent, "Element not present: " + locator);
    }

    @Keyword("WAIT_FOR_ELEMENT")
    public void waitForElement(String locator, String timeoutStr) {
        int timeout = Integer.parseInt(timeoutStr);
        ElementHelper.waitForElement(driver, locator, timeout);
    }

    @Keyword("CLOSE_BROWSER")
    public void closeBrowser() {
        if (driver != null) {
            driver.quit();
        }
    }
}
```

**3. Test Case Execution Engine:**

```java
public class KeywordTestExecutor {

    private WebKeywords webKeywords;
    private List testSteps;

    public KeywordTestExecutor() {
        this.webKeywords = new WebKeywords(null);
        this.testSteps = new ArrayList<>();
    }

    public void loadTestStepsFromExcel(String filePath, String sheetName) throws IOException {
        FileInputStream fis = new FileInputStream(filePath);
        XSSFWorkbook workbook = new XSSFWorkbook(fis);
        XSSFSheet sheet = workbook.getSheet(sheetName);

        for (int i = 1; i  params = new ArrayList<>();

        if (step.getLocator() != null && !step.getLocator().isEmpty()) {
            params.add(step.getLocator());
        }

        if (step.getTestData() != null && !step.getTestData().isEmpty()) {
            params.add(step.getTestData());
        }

        return params.toArray(new String[0]);
    }
}

public class TestStep {
    private String stepNumber;
    private String keyword;
    private String locator;
    private String testData;
    private String expectedResult;

    // Getters and setters
}
```

**4. Configuration and Reporting:**

```java
public class KeywordFrameworkConfig {

    private static Properties properties = new Properties();

    static {
        try {
            InputStream input = KeywordFrameworkConfig.class
                .getClassLoader().getResourceAsStream("keyword-config.properties");
            properties.load(input);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static String getProperty(String key) {
        return properties.getProperty(key);
    }

    public static int getTimeout() {
        return Integer.parseInt(properties.getProperty("default.timeout", "10"));
    }

    public static String getTestDataPath() {
        return properties.getProperty("testdata.path", "src/test/resources/testdata/");
    }
}

public class KeywordTestReporter {

    private List testResults = new ArrayList<>();

    public void addTestResult(String testName, String status, String details) {
        TestResult result = new TestResult();
        result.setTestName(testName);
        result.setStatus(status);
        result.setDetails(details);
        result.setExecutionTime(new Date());

        testResults.add(result);
    }

    public void generateHtmlReport(String outputPath) {
        // HTML report generation logic
        StringBuilder htmlContent = new StringBuilder();
        htmlContent.append("Keyword Test Report");
        htmlContent.append("Test Execution Report");
        htmlContent.append("");
        htmlContent.append("Test NameStatusDetailsTime");

        for (TestResult result : testResults) {
            htmlContent.append("");
            htmlContent.append("").append(result.getTestName()).append("");
            htmlContent.append("").append(result.getStatus()).append("");
            htmlContent.append("").append(result.getDetails()).append("");
            htmlContent.append("").append(result.getExecutionTime()).append("");
            htmlContent.append("");
        }

        htmlContent.append("");

        // Write to file
        try (FileWriter writer = new FileWriter(outputPath)) {
            writer.write(htmlContent.toString());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

This comprehensive guide covers all essential aspects of Selenium automation testing with TestNG and Cucumber BDD, including advanced XPath strategies, sophisticated wait mechanisms, and flexible data-driven testing approaches. The bonus sections on complex XPaths, driver management, and wait strategies provide the deep technical knowledge needed to handle challenging automation scenarios in real-world applications.
