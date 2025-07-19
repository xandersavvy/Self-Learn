# Playwright Complete Mastery Guide

_Zero to Expert: Build robust automation systems and ace technical interviews_

## Table of Contents

- [Introduction & Core Concepts](#introduction--core-concepts)
- [Environment Setup & Installation](#environment-setup--installation)
- [Playwright Fundamentals](#playwright-fundamentals)
- [Advanced Testing Patterns](#advanced-testing-patterns)
- [Framework Architecture](#framework-architecture)
- [API Testing with Playwright](#api-testing-with-playwright)
- [Visual & Mobile Testing](#visual--mobile-testing)
- [CI/CD Integration](#cicd-integration)
- [Debugging & Troubleshooting](#debugging--troubleshooting)
- [Interview Preparation](#interview-preparation)
- [Best Practices & Patterns](#best-practices--patterns)

## Introduction & Core Concepts

### What Makes Playwright Unique

Playwright is Microsoft's modern end-to-end testing framework that addresses critical limitations of traditional testing tools:

**Key Advantages Over Selenium:**

- **Native Browser Support**: Built-in support for Chromium, Firefox, and WebKit
- **Auto-Waiting**: Intelligent waiting without explicit wait commands
- **Network Interception**: Built-in request/response mocking and monitoring
- **Multiple Contexts**: Isolated browser contexts for parallel testing
- **Mobile Emulation**: Native mobile device testing capabilities
- **Headless by Default**: Faster execution with optional headed mode

**Architecture Overview:**

```
Test Script → Playwright API → Browser Driver → Browser Engine
                    ↓
            WebSocket Connection (No Selenium Grid needed)
```

### Browser Support Matrix

| Browser      | Engine | Supported Versions | Key Features                           |
| ------------ | ------ | ------------------ | -------------------------------------- |
| **Chromium** | Blink  | Latest stable      | DevTools Protocol, Performance metrics |
| **Firefox**  | Gecko  | Latest stable      | Cross-platform consistency             |
| **WebKit**   | WebKit | Safari equivalent  | iOS/macOS behavior simulation          |

## Environment Setup & Installation

### Installation Methods

**Node.js Setup:**

```bash
# Install Playwright
npm init playwright@latest

# Install specific browsers
npx playwright install

# Install system dependencies (Linux)
npx playwright install-deps
```

**Python Setup:**

```bash
# Install via pip
pip install playwright

# Install browsers
playwright install

# Install system dependencies
playwright install-deps
```

**Java Setup:**

```xml


    com.microsoft.playwright
    playwright
    1.40.0

```

**C# Setup:**

```bash
# Install via NuGet
dotnet add package Microsoft.Playwright

# Build and install browsers
pwsh bin/Debug/netX/playwright.ps1 install
```

### Project Structure Best Practices

**Recommended Directory Structure:**

```
project-root/
├── tests/
│   ├── specs/
│   │   ├── login.spec.ts
│   │   └── checkout.spec.ts
│   ├── pages/
│   │   ├── LoginPage.ts
│   │   └── CheckoutPage.ts
│   └── fixtures/
│       └── testData.json
├── playwright.config.ts
├── package.json
└── .gitignore
```

## Playwright Fundamentals

### Basic Test Structure

**Simple Test Example:**

```typescript
import { test, expect } from "@playwright/test";

test("basic navigation and interaction", async ({ page }) => {
  // Navigate to page
  await page.goto("https://demo-store.com");

  // Auto-waiting: Playwright waits for element to be actionable
  await page.click("#login-button");

  // Fill form fields
  await page.fill("#username", "testuser@example.com");
  await page.fill("#password", "securepassword");

  // Submit form
  await page.click('button[type="submit"]');

  // Assertions with auto-retry
  await expect(page.locator(".welcome-message")).toBeVisible();
  await expect(page.locator(".user-name")).toHaveText("Test User");
});
```

### Locator Strategies

**Playwright's Resilient Locators:**

```typescript
// Best Practice: Use user-facing attributes
await page.click("text=Sign In"); // Text content
await page.click('role=button[name="Submit"]'); // Accessibility role
await page.click("data-testid=login-submit"); // Test IDs

// CSS Selectors (when needed)
await page.click(".btn-primary");
await page.click("#submit-form");

// XPath (use sparingly)
await page.click('xpath=//button[contains(text(), "Submit")]');

// Combination selectors
await page.click("form >> text=Submit"); // Scoped search
```

**Advanced Locator Patterns:**

```typescript
// Filtering locators
const products = page.locator(".product");
const firstExpensiveProduct = products
  .filter({ hasText: "$" })
  .filter({ has: page.locator(".price >> text=/^[1-9]\\d{2,}$/") })
  .first();

// Chaining locators
const navigationMenu = page.locator("nav");
const userSection = navigationMenu.locator(".user-section");
await userSection.locator("text=Profile").click();

// Dynamic locators
const getProductLocator = (productName: string) => {
  return page.locator(`.product:has-text("${productName}")`);
};
```

### Auto-Waiting Mechanisms

**Built-in Intelligence:**
Playwright automatically waits for elements to be:

- **Attached** to the DOM
- **Visible** on the page
- **Stable** (not animating)
- **Enabled** (not disabled)
- **Editable** (for input actions)

**Custom Wait Conditions:**

```typescript
// Wait for network requests
await page.waitForResponse("**/api/data");
await page.waitForRequest("**/api/submit");

// Wait for DOM state
await page.waitForLoadState("domcontentloaded");
await page.waitForLoadState("networkidle");

// Wait for specific conditions
await page.waitForFunction(() => window.appReady === true);
await page.waitForTimeout(1000); // Use sparingly

// Wait for element state
await page.locator("#loading").waitFor({ state: "hidden" });
await page.locator("#content").waitFor({ state: "visible" });
```

### Page Object Model Implementation

**Robust Page Object Design:**

```typescript
export class LoginPage {
  private readonly page: Page;

  // Locators as properties for reusability
  private readonly usernameInput = 'input[name="username"]';
  private readonly passwordInput = 'input[name="password"]';
  private readonly loginButton = 'button[type="submit"]';
  private readonly errorMessage = ".error-message";
  private readonly successMessage = ".success-message";

  constructor(page: Page) {
    this.page = page;
  }

  async navigate() {
    await this.page.goto("/login");
    await this.page.waitForLoadState("domcontentloaded");
  }

  async login(username: string, password: string) {
    await this.page.fill(this.usernameInput, username);
    await this.page.fill(this.passwordInput, password);
    await this.page.click(this.loginButton);
  }

  async getErrorMessage(): Promise {
    return (await this.page.locator(this.errorMessage).textContent()) || "";
  }

  async isLoginSuccessful(): Promise {
    try {
      await this.page.locator(this.successMessage).waitFor({ timeout: 5000 });
      return true;
    } catch {
      return false;
    }
  }

  // Fluent interface pattern
  async fillUsername(username: string): Promise {
    await this.page.fill(this.usernameInput, username);
    return this;
  }

  async fillPassword(password: string): Promise {
    await this.page.fill(this.passwordInput, password);
    return this;
  }

  async submit(): Promise {
    await this.page.click(this.loginButton);
  }
}
```

## Advanced Testing Patterns

### Browser Context Management

**Context Isolation:**

```typescript
import { test, Browser, BrowserContext } from "@playwright/test";

test.describe("Multi-user scenarios", () => {
  let browser: Browser;
  let adminContext: BrowserContext;
  let userContext: BrowserContext;

  test.beforeAll(async ({ browserName }) => {
    browser = await playwright[browserName].launch();

    // Admin context with permissions
    adminContext = await browser.newContext({
      permissions: ["notifications", "geolocation"],
      viewport: { width: 1920, height: 1080 },
    });

    // Regular user context
    userContext = await browser.newContext({
      viewport: { width: 1366, height: 768 },
    });
  });

  test("admin and user concurrent actions", async () => {
    const adminPage = await adminContext.newPage();
    const userPage = await userContext.newPage();

    await adminPage.goto("/admin");
    await userPage.goto("/app");

    // Perform concurrent actions
    await Promise.all([
      adminPage.click("text=Update Settings"),
      userPage.reload(),
    ]);

    // Verify changes reflected for user
    await expect(userPage.locator(".updated-content")).toBeVisible();
  });

  test.afterAll(async () => {
    await adminContext.close();
    await userContext.close();
    await browser.close();
  });
});
```

### Network Interception and Mocking

**Request/Response Manipulation:**

```typescript
test("API response mocking", async ({ page }) => {
  // Mock API response
  await page.route("**/api/products", async (route) => {
    const mockData = {
      products: [
        { id: 1, name: "Mock Product", price: 29.99 },
        { id: 2, name: "Test Item", price: 49.99 },
      ],
    };

    await route.fulfill({
      status: 200,
      contentType: "application/json",
      body: JSON.stringify(mockData),
    });
  });

  await page.goto("/products");
  await expect(page.locator("text=Mock Product")).toBeVisible();
});

test("Network monitoring", async ({ page }) => {
  const apiCalls: string[] = [];

  // Monitor all API calls
  page.on("request", (request) => {
    if (request.url().includes("/api/")) {
      apiCalls.push(request.url());
    }
  });

  await page.goto("/dashboard");
  await page.click("text=Load Data");

  // Verify expected API calls were made
  expect(apiCalls).toContain("/api/user-data");
  expect(apiCalls).toContain("/api/dashboard-metrics");
});

test("Request modification", async ({ page }) => {
  // Modify outgoing requests
  await page.route("**/api/submit", async (route) => {
    const request = route.request();
    const postData = request.postDataJSON();

    // Add test header
    await route.continue({
      headers: {
        ...request.headers(),
        "X-Test-Run": "true",
      },
    });
  });

  await page.goto("/form");
  await page.fill("#data", "test data");
  await page.click("text=Submit");
});
```

### File Upload and Download Handling

**File Operations:**

```typescript
test("file upload handling", async ({ page }) => {
  await page.goto("/upload");

  // Single file upload
  await page.setInputFiles("#file-input", "tests/fixtures/test-document.pdf");

  // Multiple file upload
  await page.setInputFiles("#multiple-files", [
    "tests/fixtures/image1.jpg",
    "tests/fixtures/image2.jpg",
  ]);

  // Dynamic file creation and upload
  const fileContent = "Dynamic test content";
  await page.setInputFiles("#dynamic-file", {
    name: "test.txt",
    mimeType: "text/plain",
    buffer: Buffer.from(fileContent),
  });

  await page.click("text=Upload");
  await expect(page.locator(".upload-success")).toBeVisible();
});

test("file download verification", async ({ page }) => {
  await page.goto("/downloads");

  // Handle download
  const downloadPromise = page.waitForEvent("download");
  await page.click("text=Download Report");
  const download = await downloadPromise;

  // Verify download properties
  expect(download.suggestedFilename()).toBe("report.pdf");

  // Save and verify file content
  const downloadPath = "downloads/" + download.suggestedFilename();
  await download.saveAs(downloadPath);

  const fs = require("fs");
  const fileContent = fs.readFileSync(downloadPath);
  expect(fileContent.length).toBeGreaterThan(0);
});
```

### Authentication and Session Management

**Authentication Patterns:**

```typescript
// Global authentication setup
import { test as baseTest, expect } from "@playwright/test";

type TestFixtures = {
  authenticatedPage: Page;
};

const test = baseTest.extend({
  authenticatedPage: async ({ page }, use) => {
    // Perform login
    await page.goto("/login");
    await page.fill("#username", process.env.TEST_USERNAME!);
    await page.fill("#password", process.env.TEST_PASSWORD!);
    await page.click("text=Sign In");

    // Wait for authentication
    await expect(page.locator('[data-testid="user-menu"]')).toBeVisible();

    await use(page);
  },
});

// Use authenticated page in tests
test("user dashboard access", async ({ authenticatedPage }) => {
  await authenticatedPage.goto("/dashboard");
  await expect(authenticatedPage.locator("h1")).toHaveText("Dashboard");
});

// Session state management
test("save authentication state", async ({ page, context }) => {
  await page.goto("/login");
  await page.fill("#username", "user@example.com");
  await page.fill("#password", "password");
  await page.click("text=Login");

  // Save state for reuse
  await context.storageState({ path: "auth-state.json" });
});

// Reuse saved state
test.use({ storageState: "auth-state.json" });
test("authenticated test with saved state", async ({ page }) => {
  await page.goto("/protected-area");
  // Already authenticated from saved state
  await expect(page.locator(".user-content")).toBeVisible();
});
```

## Framework Architecture

### Configuration Management

**playwright.config.ts Setup:**

```typescript
import { defineConfig, devices } from "@playwright/test";

export default defineConfig({
  testDir: "./tests",
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: [
    ["html", { outputFolder: "playwright-report" }],
    ["junit", { outputFile: "results.xml" }],
    ["allure-playwright"],
  ],

  use: {
    baseURL: process.env.BASE_URL || "http://localhost:3000",
    trace: "on-first-retry",
    screenshot: "only-on-failure",
    video: "retain-on-failure",
  },

  projects: [
    {
      name: "chromium",
      use: { ...devices["Desktop Chrome"] },
    },
    {
      name: "firefox",
      use: { ...devices["Desktop Firefox"] },
    },
    {
      name: "webkit",
      use: { ...devices["Desktop Safari"] },
    },
    {
      name: "mobile-chrome",
      use: { ...devices["Pixel 5"] },
    },
    {
      name: "mobile-safari",
      use: { ...devices["iPhone 12"] },
    },
  ],

  webServer: {
    command: "npm run start",
    port: 3000,
    reuseExistingServer: !process.env.CI,
  },
});
```

### Data Management Strategies

**Test Data Organization:**

```typescript
// Test data fixtures
export const TestData = {
  users: {
    admin: {
      username: "admin@example.com",
      password: "AdminPass123",
      role: "administrator",
    },
    regular: {
      username: "user@example.com",
      password: "UserPass123",
      role: "user",
    },
  },

  products: {
    electronics: [
      { name: "Laptop", price: 999.99, category: "computers" },
      { name: "Phone", price: 699.99, category: "mobile" },
    ],
  },
};

// Environment-specific configuration
export class EnvironmentConfig {
  static getConfig() {
    const env = process.env.NODE_ENV || "development";

    const configs = {
      development: {
        baseUrl: "http://localhost:3000",
        apiUrl: "http://localhost:3001/api",
        timeout: 30000,
      },
      staging: {
        baseUrl: "https://staging.example.com",
        apiUrl: "https://staging-api.example.com",
        timeout: 60000,
      },
      production: {
        baseUrl: "https://example.com",
        apiUrl: "https://api.example.com",
        timeout: 45000,
      },
    };

    return configs[env] || configs.development;
  }
}

// Dynamic test data generation
export class DataGenerator {
  static generateUser() {
    return {
      username: `user_${Date.now()}@example.com`,
      password: "TempPass123",
      firstName: "Test",
      lastName: "User",
    };
  }

  static generateProduct() {
    return {
      name: `Product_${Math.random().toString(36).substr(2, 9)}`,
      price: Math.round(Math.random() * 1000 * 100) / 100,
      category: ["electronics", "clothing", "books"][
        Math.floor(Math.random() * 3)
      ],
    };
  }
}
```

### Custom Fixtures and Extensions

**Reusable Test Fixtures:**

```typescript
import { test as baseTest } from "@playwright/test";

// Define custom fixture types
type CustomFixtures = {
  loginPage: LoginPage;
  dashboardPage: DashboardPage;
  testUser: UserData;
  apiHelper: ApiHelper;
};

const test = baseTest.extend({
  // Page object fixtures
  loginPage: async ({ page }, use) => {
    await use(new LoginPage(page));
  },

  dashboardPage: async ({ page }, use) => {
    await use(new DashboardPage(page));
  },

  // Test data fixtures
  testUser: async ({}, use) => {
    const user = DataGenerator.generateUser();
    // Setup user via API
    await ApiHelper.createUser(user);

    await use(user);

    // Cleanup
    await ApiHelper.deleteUser(user.username);
  },

  // Helper service fixtures
  apiHelper: async ({ request }, use) => {
    const apiHelper = new ApiHelper(request);
    await use(apiHelper);
  },
});

// Use custom fixtures in tests
test("user workflow with fixtures", async ({
  loginPage,
  dashboardPage,
  testUser,
}) => {
  await loginPage.navigate();
  await loginPage.login(testUser.username, testUser.password);

  await expect(dashboardPage.welcomeMessage).toContainText(testUser.firstName);
});

export { test, expect };
```

## API Testing with Playwright

### HTTP Request Testing

**API Test Implementation:**

```typescript
import { test, expect } from "@playwright/test";

test.describe("API Testing", () => {
  let apiContext;

  test.beforeAll(async ({ playwright }) => {
    // Create API context
    apiContext = await playwright.request.newContext({
      baseURL: "https://api.example.com",
      extraHTTPHeaders: {
        Authorization: `Bearer ${process.env.API_TOKEN}`,
        "Content-Type": "application/json",
      },
    });
  });

  test("GET request with validation", async () => {
    const response = await apiContext.get("/users/1");

    expect(response.status()).toBe(200);

    const user = await response.json();
    expect(user).toHaveProperty("id", 1);
    expect(user).toHaveProperty("name");
    expect(user.email).toContain("@");
  });

  test("POST request with data validation", async () => {
    const newUser = {
      name: "Test User",
      email: "test@example.com",
      role: "user",
    };

    const response = await apiContext.post("/users", {
      data: newUser,
    });

    expect(response.status()).toBe(201);

    const createdUser = await response.json();
    expect(createdUser).toHaveProperty("id");
    expect(createdUser.name).toBe(newUser.name);
  });

  test("Error handling and status codes", async () => {
    // Test 404 - Not Found
    const notFoundResponse = await apiContext.get("/users/999999");
    expect(notFoundResponse.status()).toBe(404);

    // Test 400 - Bad Request
    const badRequest = await apiContext.post("/users", {
      data: { invalidField: "value" },
    });
    expect(badRequest.status()).toBe(400);

    const errorResponse = await badRequest.json();
    expect(errorResponse).toHaveProperty("error");
  });

  test.afterAll(async () => {
    await apiContext.dispose();
  });
});
```

### Combined UI and API Testing

**Full-Stack Test Scenarios:**

```typescript
test("end-to-end user creation flow", async ({ page, request }) => {
  // API Setup: Create test data
  const adminToken = await getAdminToken(request);
  const apiContext = await playwright.request.newContext({
    baseURL: process.env.API_URL,
    extraHTTPHeaders: {
      Authorization: `Bearer ${adminToken}`,
    },
  });

  // API: Create user via API
  const userData = {
    username: "newuser@example.com",
    password: "SecurePass123",
    role: "user",
  };

  const createResponse = await apiContext.post("/admin/users", {
    data: userData,
  });
  expect(createResponse.status()).toBe(201);

  const createdUser = await createResponse.json();

  // UI: Verify user can login
  await page.goto("/login");
  await page.fill("#username", userData.username);
  await page.fill("#password", userData.password);
  await page.click("text=Sign In");

  await expect(page.locator(".dashboard")).toBeVisible();

  // API: Verify user activity logged
  const activityResponse = await apiContext.get(
    `/users/${createdUser.id}/activity`
  );
  const activity = await activityResponse.json();

  expect(activity.some((item) => item.action === "login")).toBeTruthy();

  // Cleanup
  await apiContext.delete(`/admin/users/${createdUser.id}`);
});
```

## Visual & Mobile Testing

### Visual Regression Testing

**Screenshot Comparison:**

```typescript
test("visual regression testing", async ({ page }) => {
  await page.goto("/dashboard");
  await page.waitForLoadState("networkidle");

  // Full page screenshot
  await expect(page).toHaveScreenshot("dashboard-full.png");

  // Element-specific screenshot
  const chartElement = page.locator(".analytics-chart");
  await expect(chartElement).toHaveScreenshot("analytics-chart.png");

  // Masked screenshot (hide dynamic content)
  await expect(page).toHaveScreenshot("dashboard-masked.png", {
    mask: [page.locator(".timestamp"), page.locator(".user-avatar")],
  });

  // Cross-browser visual testing
  await expect(page).toHaveScreenshot(`dashboard-${browserName}.png`);
});

// Visual diff configuration in playwright.config.ts
export default defineConfig({
  expect: {
    toHaveScreenshot: {
      threshold: 0.2,
      mode: "strict",
    },
    toMatchSnapshot: {
      maxDiffPixels: 1000,
    },
  },

  projects: [
    {
      name: "visual-chrome",
      use: {
        ...devices["Desktop Chrome"],
        viewport: { width: 1280, height: 720 },
      },
      testMatch: "**/*.visual.spec.ts",
    },
  ],
});
```

### Mobile Device Testing

**Mobile-Specific Test Patterns:**

```typescript
test.describe('Mobile Testing', () => {
  test('responsive design validation', async ({ page }) => {
    // Test multiple viewport sizes
    const viewports = [
      { width: 375, height: 667 },  // iPhone SE
      { width: 414, height: 896 },  // iPhone 11
      { width: 768, height: 1024 }, // iPad
    ];

    for (const viewport of viewports) {
      await page.setViewportSize(viewport);
      await page.goto('/');

      // Verify mobile navigation
      if (viewport.width  {
    test.skip(browserName !== 'webkit', 'Touch gestures only on WebKit');

    await page.goto('/gallery');

    const imageGallery = page.locator('.image-gallery');

    // Swipe gestures
    await imageGallery.hover();
    await page.mouse.down();
    await page.mouse.move(100, 0);
    await page.mouse.up();

    // Verify swipe effect
    await expect(page.locator('.active-image')).toHaveAttribute('data-index', '2');
  });

  test('device-specific features', async ({ page, context }) => {
    // Grant geolocation permission
    await context.grantPermissions(['geolocation'], {
      origin: 'https://example.com'
    });

    // Mock geolocation
    await context.setGeolocation({ latitude: 37.7749, longitude: -122.4194 });

    await page.goto('/location-service');
    await page.click('text=Get Location');

    await expect(page.locator('.location-display')).toContainText('San Francisco');
  });
});
```

## CI/CD Integration

### GitHub Actions Integration

**Workflow Configuration:**

```yaml
# .github/workflows/playwright.yml
name: Playwright Tests

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    timeout-minutes: 60
    runs-on: ubuntu-latest

    strategy:
      matrix:
        browser: [chromium, firefox, webkit]

    steps:
      - uses: actions/checkout@v3

      - uses: actions/setup-node@v3
        with:
          node-version: "18"
          cache: "npm"

      - name: Install dependencies
        run: npm ci

      - name: Install Playwright Browsers
        run: npx playwright install --with-deps

      - name: Run Playwright tests
        run: npx playwright test --project=${{ matrix.browser }}
        env:
          BASE_URL: ${{ secrets.BASE_URL }}
          API_TOKEN: ${{ secrets.API_TOKEN }}

      - name: Upload test results
        uses: actions/upload-artifact@v3
        if: failure()
        with:
          name: playwright-report-${{ matrix.browser }}
          path: playwright-report/
          retention-days: 7

      - name: Upload to artifact registry
        if: success()
        run: |
          npx playwright show-report --host 0.0.0.0 &
          sleep 5
          curl -X POST ${{ secrets.RESULTS_WEBHOOK }} \
            -H "Content-Type: application/json" \
            -d '{"status": "passed", "browser": "${{ matrix.browser }}"}'
```

### Docker Integration

**Containerized Test Execution:**

```dockerfile
# Dockerfile
FROM mcr.microsoft.com/playwright:v1.40.0-focal

WORKDIR /app

# Copy package files
COPY package*.json ./
RUN npm ci

# Copy test files
COPY . .

# Set environment variables
ENV NODE_ENV=production
ENV HEADLESS=true

# Run tests
CMD ["npx", "playwright", "test"]
```

**Docker Compose Setup:**

```yaml
version: "3.8"
services:
  playwright-tests:
    build: .
    environment:
      - BASE_URL=http://app:3000
      - HEADLESS=true
    depends_on:
      - app
    volumes:
      - ./test-results:/app/test-results
      - ./playwright-report:/app/playwright-report

  app:
    image: nginx:alpine
    ports:
      - "3000:80"
    volumes:
      - ./dist:/usr/share/nginx/html
```

## Debugging & Troubleshooting

### Debugging Strategies

**Debug Mode Configuration:**

```typescript
// Debug-specific test configuration
test.describe("Debug Tests", () => {
  test.use({
    headless: false,
    slowMo: 1000,
    video: "retain-on-failure",
    trace: "on",
  });

  test("debug failing test", async ({ page }) => {
    await page.goto("/problematic-page");

    // Add debugging breakpoints
    await page.pause(); // Opens Playwright Inspector

    // Console logging
    page.on("console", (msg) => console.log("PAGE:", msg.text()));

    // Network monitoring
    page.on("request", (request) => {
      console.log("REQUEST:", request.method(), request.url());
    });

    page.on("response", (response) => {
      console.log("RESPONSE:", response.status(), response.url());
    });

    await page.click("text=Submit");
  });
});
```

**Error Handling Patterns:**

```typescript
test("robust error handling", async ({ page }) => {
  try {
    await page.goto("/unstable-page", { timeout: 30000 });

    // Wait for critical elements with timeout
    await page.waitForSelector(".main-content", { timeout: 10000 });
  } catch (error) {
    // Screenshot on error
    await page.screenshot({
      path: `error-${Date.now()}.png`,
      fullPage: true,
    });

    // Log page state
    const title = await page.title();
    const url = page.url();
    console.log(`Error on page: ${title} (${url})`);

    // Re-throw with additional context
    throw new Error(`Test failed on ${url}: ${error.message}`);
  }
});

// Global error handler
test.beforeEach(async ({ page }) => {
  page.on("pageerror", (error) => {
    console.error("Page error:", error);
  });

  page.on("requestfailed", (request) => {
    console.error(
      "Failed request:",
      request.url(),
      request.failure()?.errorText
    );
  });
});
```

### Performance Monitoring

**Performance Metrics Collection:**

```typescript
test("performance monitoring", async ({ page }) => {
  await page.goto("/performance-test");

  // Collect Core Web Vitals
  const metrics = await page.evaluate(() => {
    return new Promise((resolve) => {
      new PerformanceObserver((list) => {
        const entries = list.getEntries();
        const vitals = {};

        entries.forEach((entry) => {
          if (entry.entryType === "largest-contentful-paint") {
            vitals.LCP = entry.startTime;
          }
          if (entry.entryType === "first-input") {
            vitals.FID = entry.processingStart - entry.startTime;
          }
          if (entry.entryType === "layout-shift" && !entry.hadRecentInput) {
            vitals.CLS = (vitals.CLS || 0) + entry.value;
          }
        });

        resolve(vitals);
      }).observe({
        entryTypes: ["largest-contentful-paint", "first-input", "layout-shift"],
      });
    });
  });

  // Assert performance thresholds
  expect(metrics.LCP).toBeLessThan(2500); // 2.5 seconds
  expect(metrics.CLS).toBeLessThan(0.1); // 0.1 CLS score
});
```

## Interview Preparation

### Common Playwright Interview Questions

**Technical Concepts:**

1. **Q: How does Playwright's auto-waiting mechanism work?**

   - Playwright automatically waits for elements to be actionable before performing actions
   - Checks for attached, visible, stable, enabled, and editable states
   - Reduces need for explicit waits and makes tests more reliable

2. **Q: Explain the difference between Playwright and Selenium**

   - Playwright uses WebSocket connections vs HTTP requests
   - Built-in browser support vs external driver management
   - Auto-waiting vs manual explicit waits
   - Better network interception and mobile testing capabilities

3. **Q: How would you handle flaky tests in Playwright?**
   - Use auto-retry mechanisms with `retries` configuration
   - Implement proper wait strategies instead of fixed timeouts
   - Use network state waiting (`networkidle`, `domcontentloaded`)
   - Mock external dependencies for consistent test data

**Framework Design Questions:**

1. **Q: Design a Playwright test automation framework for an e-commerce application**

   - Page Object Model with TypeScript
   - Custom fixtures for authentication and test data
   - Environment-specific configuration
   - CI/CD integration with parallel execution
   - Visual regression testing for UI components

2. **Q: How would you implement data-driven testing in Playwright?**
   - Use test.describe.parallel for test iteration
   - JSON/CSV data files with test fixtures
   - Database integration for dynamic test data
   - Parameterized test methods with multiple data sets

**Advanced Scenarios:**

1. **Q: How would you test a real-time chat application?**

   - Multiple browser contexts for different users
   - WebSocket message monitoring
   - Race condition handling
   - Message ordering verification

2. **Q: Explain cross-browser testing strategy with Playwright**
   - Project configuration for different browsers
   - Browser-specific feature detection
   - Visual regression across browser engines
   - Performance variation analysis

### Practical Coding Challenges

**Challenge 1: Login Flow with Error Handling**

```typescript
class LoginTest {
  async testLoginScenarios(page: Page) {
    const testCases = [
      { user: "valid@example.com", pass: "correct", expected: "success" },
      { user: "invalid@example.com", pass: "wrong", expected: "invalid_user" },
      {
        user: "valid@example.com",
        pass: "wrong",
        expected: "invalid_password",
      },
    ];

    for (const testCase of testCases) {
      await this.executeLoginTest(page, testCase);
    }
  }

  async executeLoginTest(page: Page, testCase: any) {
    await page.goto("/login");
    await page.fill("#username", testCase.user);
    await page.fill("#password", testCase.pass);
    await page.click("text=Login");

    switch (testCase.expected) {
      case "success":
        await expect(page.locator(".dashboard")).toBeVisible();
        break;
      case "invalid_user":
        await expect(page.locator(".error")).toContainText("User not found");
        break;
      case "invalid_password":
        await expect(page.locator(".error")).toContainText("Invalid password");
        break;
    }
  }
}
```

**Challenge 2: Dynamic Content Testing**

```typescript
async function testDynamicTable(page: Page) {
  await page.goto('/dynamic-table');

  // Wait for table to load
  await page.waitForSelector('table tbody tr', { timeout: 10000 });

  // Get all rows
  const rows = page.locator('table tbody tr');
  const rowCount = await rows.count();

  // Verify data in each row
  for (let i = 0; i  {
  test.beforeEach(async ({ page }) => {
    await page.goto('/admin/users');
    // Common setup
  });

  test.describe('Create User', () => {
    test('with valid data', async ({ page }) => {
      // Test implementation
    });

    test('with duplicate email', async ({ page }) => {
      // Test implementation
    });
  });

  test.describe('User List', () => {
    test('displays all users', async ({ page }) => {
      // Test implementation
    });

    test('filters by role', async ({ page }) => {
      // Test implementation
    });
  });
});
```

### Error Prevention Strategies

**Robust Test Patterns:**

```typescript
// 1. Defensive Element Interaction
async function safeClick(page: Page, selector: string) {
  await page.waitForSelector(selector, { state: 'visible' });
  await page.waitForSelector(selector, { state: 'attached' });
  await page.click(selector);
}

// 2. Retry Mechanism for Flaky Operations
async function retryOperation(
  operation: () => Promise,
  maxRetries: number = 3
): Promise {
  for (let i = 0; i  setTimeout(resolve, 1000 * (i + 1)));
    }
  }
  throw new Error('Max retries exceeded');
}

// 3. Environment State Validation
async function validateEnvironmentState(page: Page) {
  // Check if application is ready
  await page.waitForFunction(() => window.appReady === true);

  // Verify no error messages on page
  const errorElements = await page.locator('.error, .alert-danger').count();
  if (errorElements > 0) {
    throw new Error('Page contains error messages');
  }

  // Check for loading indicators
  await page.waitForSelector('.loading', { state: 'hidden' });
}
```

### Performance Optimization

**Test Execution Optimization:**

```typescript
// 1. Parallel Test Execution
test.describe.configure({ mode: "parallel" });

// 2. Browser Context Reuse
const browserContext = await browser.newContext();
test.use({ context: browserContext });

// 3. Selective Test Running
test.describe("Critical Path Tests", () => {
  test.use({ tag: "@smoke" });
  // Only run smoke tests in PR builds
});

// 4. Resource Management
test.afterEach(async ({ page }) => {
  // Clear cache and cookies
  await page.context().clearCookies();
  await page.context().clearPermissions();
});
```

In Playwright, data-driven testing can be done **cleanly and flexibly** using a few different patterns, depending on your stack (JS/TS/Python/Java). Here's a quick guide for **JavaScript/TypeScript**, which is the most commonly used setup with Playwright.

---

## ✅ Best Way to Handle DATA Driven Testing (with Playwright Test Runner)

### 📦 1. Use `test.describe()` with `.each()` for looping data

```ts
import { test, expect } from "@playwright/test";

const loginData = [
  { username: "admin", password: "admin123" },
  { username: "user", password: "userpass" },
  { username: "invalid", password: "wrongpass" },
];

test.describe("Login tests", () => {
  for (const data of loginData) {
    test(`Login test for ${data.username}`, async ({ page }) => {
      await page.goto("https://example.com/login");
      await page.fill("#username", data.username);
      await page.fill("#password", data.password);
      await page.click('button[type="submit"]');

      // Add validation depending on expected result
      if (data.username === "invalid") {
        await expect(page.locator(".error")).toBeVisible();
      } else {
        await expect(page.locator("text=Welcome")).toBeVisible();
      }
    });
  }
});
```

> **✨ Pro Tip:** You can use `test.each()` if you're using Jest-like syntax.

---

### 📁 2. Externalize data (like JSON or CSV)

```ts
// testData.json
[
  { username: "admin", password: "admin123" },
  { username: "user", password: "userpass" },
];
```

```ts
// test.spec.ts
import { test, expect } from "@playwright/test";
import testData from "./testData.json";

for (const data of testData) {
  test(`Login test for ${data.username}`, async ({ page }) => {
    await page.goto("https://example.com/login");
    await page.fill("#username", data.username);
    await page.fill("#password", data.password);
    await page.click('button[type="submit"]');
    await expect(page.locator("text=Welcome")).toBeVisible();
  });
}
```

---

### 📊 3. Using Excel, YAML, or CSV?

You can use external packages:

- **Excel** → use `xlsx` npm package
- **CSV** → use `csv-parse`
- **YAML** → use `js-yaml`

Example with YAML:

```ts
// testData.yaml
- username: user1
  password: pass1
- username: user2
  password: pass2
```

```ts
import yaml from "js-yaml";
import fs from "fs";

const testData = yaml.load(fs.readFileSync("./testData.yaml", "utf8"));
```

---

## 🔁 Bonus: Parallel or Retry with Playwright Config

You can make it parallelized, isolated, or even retry flaky ones using Playwright’s built-in features in `playwright.config.ts`.

---

## 🚀 TL;DR

| Technique              | Description                    |
| ---------------------- | ------------------------------ |
| `for...of` loop        | Clean and easy for small sets  |
| `.each()`              | Jest-style sugar syntax        |
| External JSON/YAML/CSV | For larger or shared test data |
| Parameterized tests    | More readable and dynamic      |
| Use plugins/libs       | For Excel, DB, YAML, etc.      |

---

## ✅ 1. **Reports in Playwright**

By default, Playwright uses the **HTML reporter** and others like `list`, `dot`, `json`, etc.

### ▶ How to generate a report:

Just run:

```bash
npx playwright test
```

And then:

```bash
npx playwright show-report
```

This opens a clean, clickable HTML report with test names, duration, retries, traces, screenshots, etc.

### ▶ Customize in `playwright.config.ts`:

```ts
reporter: [
  ["html", { open: "never" }],
  ["json", { outputFile: "report.json" }],
];
```

> You can add `allure-playwright` too if you're a sucker for Allure's slickness:

```bash
npm install -D allure-playwright
```

Then in config:

```ts
reporter: [["allure-playwright"]];
```

---

## 🏷️ 2. **Tag-like Suite Execution (Cucumber-Style)**

No built-in tags, but Playwright gives you **equivalent power with custom project names or test titles**.

### Option A: **Use `.grep` in CLI or config**

```bash
npx playwright test --grep "Login"
```

This will only run tests whose names match `"Login"`.

In code:

```ts
test('[@Smoke] Login with valid user', async () => { ... })
```

Or use regex:

```bash
npx playwright test --grep "@Smoke"
```

### Option B: **Custom Tags via test titles**

Just prefix the title:

```ts
test("[smoke] Validate search", async () => {});
test("[regression] Validate cart", async () => {});
```

Then filter them:

```bash
npx playwright test --grep "\[smoke\]"
```

---

## 🧪 3. **Simulate Suite Like TestNG (Method-wise)**

Use `test.describe()` blocks as your "test class" or "suite", and each `test()` inside becomes a method.

```ts
test.describe('Login Suite', () => {
  test('should login with valid credentials', async () => { ... });
  test('should fail with invalid credentials', async () => { ... });
});
```

This outputs like:

```
Login Suite › should login with valid credentials
Login Suite › should fail with invalid credentials
```

And you can grep by method name or suite name just like method filtering in TestNG:

```bash
npx playwright test --grep "valid credentials"
```

---

## 🌀 Bonus: Group Tests Like a Boss

Use **multiple projects** in `playwright.config.ts` to simulate environments or tags:

```ts
projects: [
  {
    name: "Chrome @smoke",
    use: { browserName: "chromium" },
  },
  {
    name: "Firefox @regression",
    use: { browserName: "firefox" },
  },
];
```

Then:

```bash
npx playwright test --project="*smoke*"
```

---

## 🛠️ TL;DR Cheat Sheet

| What You Want              | How to Do It in Playwright             |
| -------------------------- | -------------------------------------- |
| Suite-like grouping        | `test.describe()`                      |
| Method-wise test filtering | `--grep "method name"`                 |
| Cucumber-like tags         | Prefix test name + use `--grep`        |
| Data-driven tests          | Use `for...of`, `test.each()`, or JSON |
| Reports                    | `npx playwright show-report` or Allure |
| Environment-based grouping | Use `projects` in config               |

---

[2] https://playwright.dev/docs/browsers
[1] https://www.accelq.com/blog/playwright-vs-selenium/
[3] https://www.zenrows.com/blog/playwright-vs-selenium
[4] https://www.browserstack.com/guide/playwright-vs-selenium
[5] https://www.npmjs.com/package/playwright
[6] https://dogq.io/blog/playwright-vs-selenium/
[7] https://playwright.dev
[8] https://www.browserstack.com/docs/automate/playwright/browsers-and-os
[9] https://saucelabs.com/resources/blog/playwright-vs-selenium-guide
[10] https://playwright.dev/docs/release-notes
[11] https://www.youtube.com/watch?v=NcSk9fOGEac
[12] https://testomat.io/blog/playwright-vs-selenium-the-evolution-of-dominance-can-selenium-make-a-comeback/
[13] https://www.contextqa.com/news/playwright-expands-cross-browser-testing-capabilities-in-2025/
[14] https://github.com/microsoft/playwright/releases
[15] https://www.reddit.com/r/Playwright/comments/1jb29zu/is_playwright_the_best_alternative_to_selenium_in/
[16] https://www.cgvakindia.com/blog/mastering-playwright-automation-advanced-features-and-trends-2025/
[17] https://playwright.dev/java/docs/release-notes
[18] https://applitools.com/blog/playwright-vs-selenium/
[19] https://www.royalcyber.com/blogs/test-automation/appium-vs-playwright-complete-comparison-guide-for-2025/
[20] https://playwright.dev/python/docs/release-notes
[21] https://testsigma.com/blog/playwright-vs-selenium/
[22] https://www.hirist.tech/blog/top-15-playwright-interview-questions-and-answers/
[23] https://www.reddit.com/r/QualityAssurance/comments/1248csz/playwright_framework_best_practicesstructure/
[24] https://www.royalcyber.com/blogs/appium-vs-playwright-complete-comparison-guide-for-2025/
[25] https://www.linkedin.com/posts/tushar-desai-4881651b3_playwright-interview-questions-with-answers-activity-7302576097244672001-I_Oe
[26] https://betterstack.com/community/guides/testing/playwright-best-practices/
[27] https://katalon.com/resources-center/blog/playwright-vs-selenium
[28] https://www.softwaretestingmaterial.com/playwright-interview-questions/
[29] https://grotechminds.com/best-practices-for-non-coders-to-start-with-playwright-automation/
[30] https://testrigor.com/blog/why-playwright-sucks-for-end-to-end-tests/
[31] https://www.lambdatest.com/learning-hub/playwright-interview-questions
[32] https://autify.com/blog/playwright-best-practices
[33] https://testomat.io/blog/test-automation-with-playwright-definition-and-benefits-of-this-testing-framework/
[34] https://grotechminds.com/playwright-interview-questions-and-answers/
[35] https://www.frugaltesting.com/blog/best-practices-for-writing-scalable-playwright-test-scripts
[36] https://playwright.dev/docs/api/class-browsercontext
[37] https://autify.com/blog/playwright-waitforselector
[38] https://www.checklyhq.com/learn/playwright/testing-in-parallel/
[39] https://codoid.com/api-testing/api-testing-with-playwright-a-comprehensive-guide/
[40] https://stackoverflow.com/questions/77827006/how-do-you-differentiate-between-browser-and-context-in-playwright
[41] https://www.linkedin.com/pulse/mastering-playwrights-auto-waiting-feature-beginners-testing-povyf
[42] https://playwright.dev/docs/test-sharding
[43] https://rtcamp.com/resources/api-test-automation-with-playwright/
[44] https://www.neovasolutions.com/2024/07/25/a-comprehensive-guide-to-playwright-browser-contexts-and-sessions-in-typescript/
[45] https://www.browserstack.com/guide/playwright-waitforloadstate
[46] https://docs.currents.dev/guides/ci-optimization/fully-parallel-mode
[47] https://talent500.com/blog/api-testing-with-playwright/
[48] https://www.youtube.com/watch?v=0mfLHPLZ7_k
[49] https://grotechminds.com/playwrights-auto-wait-and-web-first-assertions-in-python/
[50] https://playwright.dev/docs/test-parallel
[51] https://jignect.tech/enhancing-api-automation-testing-with-playwright-and-typescript-get-and-post-operations-explained/
[52] https://www.checklyhq.com/guides/auto-waiting-methods/
[53] https://www.youtube.com/watch?v=8NIm1QCUXE0
[54] https://playwright.dev/docs/api-testing
[55] https://dzone.com/articles/fixtures-built-in-fixtures-in-playwright
[56] https://dev.to/zt4ff_1/playwright-tutorial-for-beginners-10-configuration-2gko
[57] https://www.perfecto.io/blog/playwright-mobile-automation
[58] https://codoid.com/automation-testing/playwright-fixtures-in-action-create-reusable-and-maintainable-tests/
[59] https://www.cuketest.com/playwright/docs/test-configuration/
[60] https://playwright.dev/docs/emulation
[61] https://www.checklyhq.com/blog/how-to-implement-custom-test-fixtures-in-playwright/
[62] https://playwright.dev/docs/test-configuration
[63] https://www.browserstack.com/docs/automate/playwright/playwright-test-emulation
[64] https://www.browserstack.com/guide/fixtures-in-playwright
[65] https://playwright.dev/docs/test-global-setup-teardown
[66] https://www.checklyhq.com/learn/playwright/emulating-mobile-devices/
[67] https://testomat.io/blog/what-is-the-use-of-fixtures-in-playwright/
[68] https://www.youtube.com/watch?v=JtA-AJbC45w
[69] https://www.reddit.com/r/Playwright/comments/1jc9j8k/mobile_testing/
[70] https://www.testleaf.com/blog/playwright-trace-viewer/
[71] https://www.checklyhq.com/learn/playwright/intercept-requests/
[72] https://www.browserstack.com/guide/visual-regression-testing-using-playwright
[73] https://playwright.dev/docs/trace-viewer
[74] https://www.youtube.com/watch?v=-pH6Dax-OyY
[75] https://mayashavin.com/articles/visual-testing-vitest-playwright
[76] https://www.neovasolutions.com/2024/07/04/how-to-handle-trace-viewer-in-playwright/
[77] https://www.checklyhq.com/blog/speed-up-playwright-scripts-request-interception/
[78] https://www.chromatic.com/blog/how-to-visual-test-ui-using-playwright/
[79] https://playwright.dev/dotnet/docs/trace-viewer-intro
[80] https://timdeschryver.dev/blog/intercepting-http-requests-with-playwright
[81] https://www.duncanmackenzie.net/blog/visual-regression-testing/
[82] https://www.youtube.com/watch?v=yP6AnTxC34s
[83] https://stackoverflow.com/questions/77840069/playwright-js-how-to-intercept-on-http-calls-on-button-click
[84] https://www.checklyhq.com/blog/visual-regression-testing-with-playwright/
[85] https://www.linkedin.com/pulse/deep-dive-playwrights-internal-architecture-e2e-test-automation-mjw7c
[86] https://playwright.dev/docs/locators
[87] https://www.browserstack.com/guide/page-object-model-with-playwright
[88] https://dev.to/aswani25/integrating-playwright-with-cicd-pipelines-1g1m
[89] https://www.testingdocs.com/playwright-architecture/
[90] https://bugbug.io/blog/testing-frameworks/playwright-locators/
[91] https://playwright.dev/docs/pom
[92] https://www.youtube.com/watch?v=ZiQhPD3i3Ho
[93] https://www.youtube.com/watch?v=f4y1pJdkdNA
[94] https://www.bondaracademy.com/blog/playwright-locators-best-practices
[95] https://dzone.com/articles/page-object-model-pom-with-playwright
[96] https://dev.to/aswani25/integrating-playwright-with-cicd-pipelines-using-gitlab-a-step-by-step-guide-3ik2
[97] https://www.youtube.com/watch?v=_3Ga-NeGtJ8
[98] https://codilime.com/blog/page-object-model-with-playwright-and-typescript/
[99] https://www.browserstack.com/docs/automate/playwright/ci-cd
[100] https://www.testingmavens.com/blogs/architecture-breakdown-selenium-cypress-and
[101] https://www.lambdatest.com/blog/playwright-locators/
[102] https://www.youtube.com/watch?v=4aiu6pRE0YQ
[103] https://www.youtube.com/watch?v=D44k45N6S58
[104] https://www.browserstack.com/guide/playwright-debugging
[105] https://www.youtube.com/watch?v=rRmfYu8hlbw
[106] https://playwright.bootcss.com/python/docs/debug
[107] https://autify.com/blog/playwright-debug
[108] https://www.youtube.com/watch?v=rhzrFiKfWwY
[109] https://playwright.dev/docs/debug
[110] https://playwright.dev/docs/test-fixtures
[111] https://www.headspin.io/blog/playwright-automation-framework-guide
[112] https://www.checklyhq.com/learn/playwright/what-is-playwright/
[113] https://research.aimultiple.com/playwright-vs-selenium/
[114] https://www.kyrre.dev/blog/the-pragmatic-guide-to-playwright-testing
[115] https://www.frugaltesting.com/blog/comprehensive-guide-to-end-to-end-testing-with-playwright-for-web-applications
[116] https://www.ideas2it.com/blogs/step-by-step-guide-to-setup-a-comprehensive-playwright-test-automation-framework-using-page-object-model
[117] https://automationqahub.com/the-ultimate-list-of-playwright-interview-questions/
[118] https://courses.bigbinaryacademy.com/learn-qa-automation-using-playwright/best-practices/
[119] https://dev.to/playwright/organizing-playwright-tests-effectively-2hi0
[120] https://playwright.dev/docs/writing-tests
[121] https://testguild.com/playwright-tutorial-getting-started-with-playwright-framework/
[122] https://www.spritecloud.com/guides/playwright
[123] https://www.youtube.com/watch?v=2poXBtifpzA
[124] https://www.thetesttribe.com/blog/playwright-tutorial/
[125] https://www.udemy.com/course/playwright-tutorials-automation-testing/
[126] https://www.youtube.com/watch?v=pq20Gd4LXeI
[127] https://www.geeksforgeeks.org/software-testing/difference-between-playwright-and-selenium/
[128] https://playwright.dev/docs/best-practices
[129] https://www.browserstack.com/guide/playwright-tutorial
[130] https://testgrid.io/blog/playwright-testing/
[131] https://stackoverflow.com/questions/74920905/how-to-give-fixed-wait-in-playwright-without-any-condition-like-we-had-in-cypres
[132] https://www.checklyhq.com/blog/playwright-auto-waiting/
[133] https://playwright.dev/docs/actionability
[134] https://playwright.dev/docs/ci
[135] https://www.browserstack.com/guide/playwright-wait-types
[136] https://codoid.com/automation-testing/step-by-step-playwright-page-object-model-implementation-tutorial/
[137] https://www.browserstack.com/guide/speed-up-ci-cd-pipelines-with-parallel-testing
[138] https://www.cuketest.com/playwright/docs/actionability/
[139] https://blogs.perficient.com/2025/07/15/implementing-end-to-end-testing-using-playwright-within-jenkins-ci-cd-pipelines/
[140] https://www.youtube.com/watch?v=srkXpc7Ehbs
[141] https://www.lambdatest.com/learning-hub/playwright-page-object-model
[142] https://github.com/LambdaTest/playwright-sample
[143] https://testomat.io/blog/page-object-model-pattern-javascript-with-playwright/
[144] https://www.browserstack.com/guide/cypress-vs-selenium-vs-playwright-vs-puppeteer
[145] https://www.lambdatest.com/blog/playwright-vs-selenium-vs-cypress/
[146] https://www.browserstack.com/guide/playwright-vs-cypress
[147] https://www.youtube.com/watch?v=788GvvcfwTY
[148] https://www.lambdatest.com/blog/cypress-vs-playwright/
[149] https://www.globalapptesting.com/blog/automation-testing-framework
[150] https://www.axelerant.com/blog/cypress-selenium-playwright
[151] https://www.youtube.com/watch?v=eP2V1fzW_Oo
[152] https://www.vskills.in/interview-questions/playwright-interview-questions
[153] https://www.pixelqa.com/blog/post/install-playwright-automation-guide-2025
[154] https://testomat.io/blog/playwright-vs-selenium-vs-cypress-a-detailed-comparison/
[155] https://www.youtube.com/watch?v=O2Yl3wnGH5Q
[156] https://www.alphabin.co/blog/using-playwright-for-mobile-web-testing
[157] https://codoid.com/automation-testing/playwright-mobile-automation-for-seamless-web-testing/
[158] https://jignect.tech/streamlining-automation-testing-in-mobile-with-playwright-and-typescript/
[159] https://testautomationtools.dev/playwright-overview/
[160] https://www.lambdatest.com/playwright
[161] https://playwright.dev/docs/api/class-android
[162] https://testomat.io/blog/playwright-api-testing-comprehensive-guide-with-examples/
[163] https://www.checklyhq.com/learn/playwright/writing-tests/
[164] https://oxylabs.io/blog/playwright-best-practices
[165] https://testquality.com/playwright-vs-selenium-ultimate-guide-test-automation/
[166] https://testguild.com/automation-testing-trends/
[167] https://qacomet.com/playwright/architecture/
[168] https://www.youtube.com/watch?v=EEjyLfp6DoQ
[169] https://www.qualiti.ai/playwright-test-automation/playwright-mobile-testing
[170] https://www.neovasolutions.com/2024/12/26/advanced-debugging-techniques-in-playwright-with-typescript/
[171] https://hackernoon.com/playwright-api-testing-a-comprehensive-guide-for-beginners
[172] https://playwright.dev/dotnet/docs/debug
[173] https://playwright.dev/python/docs/api-testing
[174] https://www.reddit.com/r/softwaretesting/comments/1bovaoa/selenium_vs_playwright/
[175] https://www.royalcyber.com/blogs/test-automation/playwright-vs-selenium/
[176] https://www.cromacampus.com/blogs/complete-guide-to-playwright-automation-tool-tutorial/
[177] https://jignect.tech/beginners-roadmap-to-test-automation-playwright-and-typescript-made-simple/
[178] https://www.linkedin.com/posts/bakkappa-n_top-50-playwright-interview-questions-and-activity-7231162063195160578-XI6I
