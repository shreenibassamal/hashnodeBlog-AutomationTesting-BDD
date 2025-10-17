---
title: "BDD Cucumber – Complete Rules"
datePublished: Fri Oct 17 2025 03:54:11 GMT+0000 (Coordinated Universal Time)
cuid: cmgube7tg000202l5b0pj36os
slug: bdd-cucumber-complete-rules
tags: shreenibas

---

# 🧩 **📘 BDD Cucumber – Complete Rules, Usage & Best Practices**

---

## ⚙️ **1️⃣ What is BDD (Behavior Driven Development)?**

**BDD (Behavior Driven Development)** is a software development approach that:

* Focuses on **behavior of the application** (not implementation)
    
* Uses **plain English language (Gherkin syntax)** to describe test scenarios
    
* Encourages **collaboration** between developers, testers, and business teams
    

💡 **Goal:** To make sure everyone understands what the system should do — before writing code.

---

## 🧱 **2️⃣ Structure of a BDD Feature File**

A `.feature` file has this structure:

```java
Feature: <Feature Name>
  Description: Optional details about the feature
  
  Background:
    Given <precondition>

  @Tag1 @Tag2
  Scenario: <Scenario Name>
    Given <context or setup>
    When <action performed>
    Then <expected result>

  Scenario Outline: <Reusable scenario with examples>
    Given ...
    When ...
    Then ...
    Examples:
      | column1 | column2 |
      | value1  | value2  |
```

---

## 🧩 **3️⃣ Cucumber Keywords and When to Use Them**

| Keyword | Purpose | Rule / When to Use |
| --- | --- | --- |
| **Feature** | Defines a module or functionality | One `.feature` file per module or story |
| **Background** | Common precondition steps | Use when same steps repeat in all scenarios |
| **Scenario** | Describes a specific test case | Use one scenario per use case |
| **Scenario Outline** | Reusable scenario with multiple data sets | Use for data-driven tests |
| **Given** | Precondition (setup / state) | “System is in a known state” |
| **When** | Action or event | “User performs an operation” |
| **Then** | Verification or expected outcome | “System responds or shows result” |
| **And / But** | Chain similar Given/When/Then | Use for readability |
| **Examples** | Provide data for Scenario Outline | Add multiple test data combinations |
| **@Tags** | Group scenarios logically | Used to filter test runs (UI, API, Smoke, etc.) |

---

## 🧩 **4️⃣ Example – Correct BDD Style**

```java
@UI @Login
Feature: Login Functionality

  Background:
    Given User is on the login page

  Scenario: Successful login with valid credentials
    When User enters username "admin" and password "admin123"
    And User clicks on login button
    Then User should be navigated to the dashboard

  Scenario Outline: Invalid login with different credentials
    When User enters username "<username>" and password "<password>"
    And User clicks on login button
    Then Error message "<message>" should be displayed

    Examples:
      | username | password | message                |
      | admin    | wrong123 | Invalid credentials    |
      | blank    | admin123 | Username cannot be empty |
```

---

## 🧩 **5️⃣ BDD “Golden Rules” — Follow These Strictly**

### ✅ **Rule 1: Write in Business Language**

* Always use **plain English**, understandable by non-technical people.
    
* ❌ Don’t use words like *click button*, *sendKeys*, *driver.findElement* in feature files.
    

✅ Example:  
✔️ `When user logs into the application`  
❌ `When user enters username in input field`

---

### ✅ **Rule 2: One Behavior = One Scenario**

* A single scenario should test **one behavior only**.
    
* Avoid long scenarios with multiple outcomes.
    

✅ Example:  
✔️ Scenario: Successful login  
✔️ Scenario: Invalid login  
❌ Scenario: Successful and invalid login together

---

### ✅ **Rule 3: Each Step Must Be Independent**

* Don’t depend on one scenario’s result in another.
    
* Scenarios should be **isolated**.
    

---

### ✅ **Rule 4: Background for Common Steps Only**

* If multiple scenarios share the same setup, use `Background`.
    

✅ Example:

```java
Background:
  Given user has opened the login page
```

---

### ✅ **Rule 5: Reuse Step Definitions**

* Write reusable and generic step methods.
    
* Keep `stepDefinitions` **short and meaningful**.
    

✅ Example:

```java
@When("User logs in with username {string} and password {string}")
public void login(String user, String pass) {
   // use parameters for reuse
}
```

---

### ✅ **Rule 6: Use Tags to Organize**

| Tag | Purpose |
| --- | --- |
| `@Smoke` | For critical smoke tests |
| `@Regression` | For full regression runs |
| `@UI` | For Selenium UI tests |
| `@API` | For REST Assured API tests |
| `@DB` | For database tests |

You can run them selectively using TestNG runner:

```java
tags = "@Smoke or @API"
```

---

### ✅ **Rule 7: Keep Feature Files Small**

* Each feature file should describe **one module or functionality**.
    
* Prefer multiple smaller feature files over one large file.
    

---

### ✅ **Rule 8: Avoid Technical Details in Gherkin**

Feature files should not mention:

* Element locators (like `#username` or `id=loginBtn`)
    
* API endpoints or JSON payloads
    
* File paths or test data
    

All technical parts belong in **Step Definition code**, not in `.feature`.

---

### ✅ **Rule 9: Use Scenario Outline for Data-Driven Tests**

When you test same steps with multiple input/output combinations:

```java
Scenario Outline: Login validation
  When User logs in with username "<user>" and password "<pass>"
  Then Message "<message>" should appear

  Examples:
    | user  | pass    | message              |
    | admin | 12345   | Login successful     |
    | test  | wrong   | Invalid credentials  |
```

---

### ✅ **Rule 10: Keep Steps Declarative, Not Imperative**

BDD focuses on **what** the user does, not **how** they do it.

✅ Example (Good):

```java
When user logs into the app
Then dashboard should be visible
```

❌ Example (Bad):

```java
When user enters username in textbox
And user clicks login button
```

---

## 🧩 **6️⃣ DON’Ts — Common Mistakes to Avoid**

| ❌ Mistake | ✅ Correct Practice |
| --- | --- |
| Using too many technical details | Keep business-focused |
| Having long scenarios | Split into smaller ones |
| Repeating Given steps | Use Background |
| Copy-pasting same steps | Use reusable step definitions |
| Mixing UI and API steps in same scenario | Separate by feature or tag |
| Using "And" for unrelated steps | Use only for continuation |
| Hardcoding data inside step definitions | Use parameters or Examples table |
| Using loops/conditions inside step definitions | Move logic to utility classes |

---

## 🧩 **7️⃣ When to Use BDD**

Use BDD when:  
✅ You need **clear communication** among QA, devs, and product team  
✅ You’re working in **Agile / Scrum** environment  
✅ You want **living documentation** of system behavior  
✅ You have **repetitive UI/API flows** that can be described in natural language

---

## 🧩 **8️⃣ When *Not* to Use BDD**

Avoid BDD when:  
❌ You’re testing **complex backend logic** only understood by developers  
❌ The team doesn’t participate in feature writing  
❌ You don’t have time to maintain `.feature` files properly  
❌ You need **quick one-off API checks** (use Postman or direct RestAssured instead)

---

## 🧩 **9️⃣ BDD Workflow in Automation**

| Stage | Who does it | Example |
| --- | --- | --- |
| Write Feature file | QA / BA / Product Owner | `Project_UI.feature` |
| Review Gherkin | Dev + QA + BA | Verify behavior correctness |
| Implement Step Definitions | Automation Engineer | Selenium / API code |
| Run Tests | CI/CD (Jenkins) | Trigger via `@Smoke` or `@Regression` |

---

## 🧩 **🔟 Folder Naming Rules**

```java
features/               → all .feature files
stepDefinitions/         → all step implementation files
hooks/                   → setup & teardown
runners/                 → TestNG runners
utils/                   → helper classes (DriverFactory, APIUtils)
```

---

## 🧩 **11️⃣ Bonus – Hooks Usage Rules Recap**

| Hook | When to Use | Example |
| --- | --- | --- |
| `@Before` | Launch browser / setup API base URI | before each scenario |
| `@After` | Quit browser / close connection | after each scenario |
| `@Before("@tag")` | Tag-specific setup | UI or API based |
| `@AfterStep` | Screenshot or log step | optional |

---

## 🧩 **12️⃣ Summary – BDD Cucumber Rules Checklist ✅**

| Rule | Must Follow |
| --- | --- |
| One feature per module | ✅ |
| One scenario per behavior | ✅ |
| Use plain English | ✅ |
| Reuse step definitions | ✅ |
| No technical details in .feature | ✅ |
| Use Background wisely | ✅ |
| Use Scenario Outline for data-driven | ✅ |
| Tag scenarios properly | ✅ |
| Keep feature files readable | ✅ |
| Collaborate before coding | ✅ |

Perfect 👏 — here’s your **complete BDD + Cucumber + TestNG automation framework template** that follows every best-practice rule we discussed.  
This structure works for **both UI (Selenium)** and **API (RestAssured)** testing, with tag-based control.

---

# 🧩 **1️⃣ Project Folder Structure (Maven Style)**

```java
BDDFramework/
│
├── pom.xml
├── testng.xml
└── src
    └── test
        ├── java
        │   ├── hooks/
        │   │   └── Hooks.java
        │   ├── stepDefinitions/
        │   │   ├── UISteps.java
        │   │   ├── APISteps.java
        │   │   └── CommonSteps.java
        │   ├── runners/
        │   │   └── TestRunner.java
        │   └── utils/
        │       ├── DriverFactory.java
        │       ├── ApiUtils.java
        │       └── ConfigReader.java
        └── resources/
            └── features/
                ├── UI_Login.feature
                ├── API_Project.feature
                └── Regression.feature
```

---

# 🧩 **2️⃣ pom.xml (Key Dependencies)**

```java
<project xmlns="http://maven.apache.org/POM/4.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0  
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.ninza.bdd</groupId>
  <artifactId>BDDFramework</artifactId>
  <version>1.0-SNAPSHOT</version>

  <dependencies>
    <!-- Selenium -->
    <dependency>
      <groupId>org.seleniumhq.selenium</groupId>
      <artifactId>selenium-java</artifactId>
      <version>4.25.0</version>
    </dependency>

    <!-- Cucumber -->
    <dependency>
      <groupId>io.cucumber</groupId>
      <artifactId>cucumber-java</artifactId>
      <version>7.17.0</version>
    </dependency>
    <dependency>
      <groupId>io.cucumber</groupId>
      <artifactId>cucumber-testng</artifactId>
      <version>7.17.0</version>
    </dependency>

    <!-- TestNG -->
    <dependency>
      <groupId>org.testng</groupId>
      <artifactId>testng</artifactId>
      <version>7.10.2</version>
      <scope>test</scope>
    </dependency>

    <!-- RestAssured -->
    <dependency>
      <groupId>io.rest-assured</groupId>
      <artifactId>rest-assured</artifactId>
      <version>5.5.0</version>
    </dependency>

    <!-- JSON -->
    <dependency>
      <groupId>org.json</groupId>
      <artifactId>json</artifactId>
      <version>20240303</version>
    </dependency>
  </dependencies>
</project>
```

---

# 🧩 **3️⃣ Feature Files**

### ✅ UI\_Login.feature

```java
@UI @Smoke
Feature: Validate login functionality of the web application

  Background:
    Given User launches the browser

  Scenario: Successful login with valid credentials
    When User logs in with username "admin" and password "admin123"
    Then Dashboard should be displayed

  Scenario Outline: Unsuccessful login
    When User logs in with username "<user>" and password "<pass>"
    Then Error message "<message>" should appear
    Examples:
      | user   | pass    | message                |
      | admin  | wrong   | Invalid credentials    |
      | blank  | admin12 | Username is required   |
```

---

### ✅ API\_Project.feature

```java
@API @Regression
Feature: Validate CRUD operations for Project API

  Scenario: Create new project via API
    Given API base URI is configured
    When User sends POST request with project data
    Then Response status should be 201
    And Response should contain project name "AutomationProject"

  Scenario: Get all projects
    Given API base URI is configured
    When User sends GET request to fetch all projects
    Then Response status should be 200
```

---

# 🧩 **4️⃣** [**Hooks.java**](http://Hooks.java)

```java
package hooks;

import io.cucumber.java.*;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import io.restassured.RestAssured;

public class Hooks {
    public static WebDriver driver;

    @Before("@UI")
    public void beforeUIScenario() {
        System.out.println("🔹 Launching browser for UI scenario");
        driver = new ChromeDriver();
        driver.manage().window().maximize();
    }

    @Before("@API")
    public void beforeAPIScenario() {
        System.out.println("🔹 Setting up base URI for API tests");
        RestAssured.baseURI = "https://dummyjson.com";
    }

    @After("@UI")
    public void afterUIScenario() {
        System.out.println("🔹 Closing browser");
        if (driver != null) driver.quit();
    }

    @AfterStep
    public void afterStep(Scenario scenario) {
        System.out.println("➡️ Completed step: " + scenario.getName());
    }
}
```

---

# 🧩 **5️⃣ Step Definitions**

### ✅ [UISteps.java](http://UISteps.java)

```java
package stepDefinitions;

import io.cucumber.java.en.*;
import hooks.Hooks;
import org.openqa.selenium.By;

public class UISteps {

    @Given("User launches the browser")
    public void user_launches_browser() {
        Hooks.driver.get("https://example.com/login");
        System.out.println("Opened login page");
    }

    @When("User logs in with username {string} and password {string}")
    public void user_logs_in(String username, String password) {
        Hooks.driver.findElement(By.id("username")).sendKeys(username);
        Hooks.driver.findElement(By.id("password")).sendKeys(password);
        Hooks.driver.findElement(By.id("loginBtn")).click();
    }

    @Then("Dashboard should be displayed")
    public void dashboard_should_be_displayed() {
        boolean dashboard = Hooks.driver.findElement(By.id("dashboard")).isDisplayed();
        assert dashboard;
        System.out.println("Dashboard visible");
    }

    @Then("Error message {string} should appear")
    public void error_message_should_appear(String msg) {
        String actual = Hooks.driver.findElement(By.id("errorMsg")).getText();
        assert actual.equals(msg);
    }
}
```

---

### ✅ [APISteps.java](http://APISteps.java)

```java
package stepDefinitions;

import io.cucumber.java.en.*;
import io.restassured.response.Response;
import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

public class APISteps {

    Response response;

    @Given("API base URI is configured")
    public void api_base_uri_is_configured() {
        System.out.println("Base URI: " + baseURI);
    }

    @When("User sends POST request with project data")
    public void user_sends_post_request() {
        String body = """
            {
              "title": "AutomationProject",
              "description": "BDD Created"
            }
            """;
        response = given()
            .header("Content-Type", "application/json")
            .body(body)
            .when().post("/projects");
    }

    @Then("Response status should be {int}")
    public void response_status_should_be(Integer code) {
        response.then().statusCode(code);
    }

    @Then("Response should contain project name {string}")
    public void response_should_contain_project_name(String name) {
        response.then().body("title", equalTo(name));
    }

    @When("User sends GET request to fetch all projects")
    public void user_sends_get_request_to_fetch_all_projects() {
        response = get("/projects");
    }
}
```

---

# 🧩 **6️⃣** [**CommonSteps.java**](http://CommonSteps.java)

```java
package stepDefinitions;

import io.cucumber.java.en.*;

public class CommonSteps {

    @Given("Application is ready")
    public void application_is_ready() {
        System.out.println("✅ Application ready for testing");
    }
}
```

---

# 🧩 **7️⃣ Runner Class**

```java
package runners;

import io.cucumber.testng.AbstractTestNGCucumberTests;
import io.cucumber.testng.CucumberOptions;

@CucumberOptions(
    features = "src/test/resources/features",
    glue = {"stepDefinitions", "hooks"},
    plugin = {
        "pretty",
        "html:target/cucumber-report.html",
        "json:target/cucumber.json"
    },
    tags = "@UI or @API",
    monochrome = true
)
public class TestRunner extends AbstractTestNGCucumberTests {}
```

---

# 🧩 **8️⃣ testng.xml (Optional)**

```java
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd" >
<suite name="BDD Suite">
  <test name="BDD Execution">
    <classes>
      <class name="runners.TestRunner" />
    </classes>
  </test>
</suite>
```

---

# 🧩 **9️⃣ How to Execute**

In Maven:

```java
mvn clean test
```

In TestNG:

```java
Right-click testng.xml → Run As → TestNG Suite
```

---

# 🧩 **🔟 Reports**

After run, open:

```java
target/cucumber-report.html
target/cucumber.json
```

---

# 🧩 **✅ Summary**

| Component | Purpose |
| --- | --- |
| `.feature` | Natural language scenarios |
| `stepDefinitions` | Java code for steps |
| `hooks` | Common setup/teardown |
| `runner` | Executes cucumber suite |
| `pom.xml` | Dependency manager |
| `testng.xml` | Integration with TestNG |