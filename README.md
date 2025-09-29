# Playwright Learning

A comprehensive guide and practice repository for learning Playwright end-to-end testing.

## Table of Contents
- [Prerequisites](#Node )
- [Installation](#installation)
- [Configuration](#configuration)
- [Running Tests](#running-tests)
- [Project Structure](#project-structure)
- [Writing Your First Test](#writing-your-first-test)
- [Advanced Configuration](#advanced-configuration)
- [Best Practices](#best-practices)
- [Resources](#resources)

## Prerequisites

- Node.js (version 16 or higher)
- npm or yarn package manager

## Installation

### 1. Initialize a new project (if starting fresh)
```bash
npm init -y
```

### 2. Install Playwright
```bash
# Install Playwright
npm init playwright@latest

# Or install manually
npm install -D @playwright/test
npx playwright install
```

### 3. Install browsers (if not done automatically)
```bash
npx playwright install
```

## Configuration

### Basic Configuration (`playwright.config.js`)
The main configuration file controls test behavior, browsers, and reporting.

```javascript
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: 'html',
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
  },
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
  ],
});
```

### Environment Variables
Create a `.env` file for sensitive data:
```
BASE_URL=https://your-app.com
TEST_USERNAME=testuser
TEST_PASSWORD=testpass
```

## Running Tests

### Basic Commands
```bash
# Run all tests
npx playwright test

# Run tests in headed mode (visible browser)
npx playwright test --headed

# Run tests in specific browser
npx playwright test --project=chromium

# Run specific test file
npx playwright test tests/example.spec.js

# Run tests with specific tag
npx playwright test --grep "@smoke"

# Debug mode (step through tests)
npx playwright test --debug
```

### Test Reports
```bash
# Show HTML report
npx playwright show-report

# Generate and show trace
npx playwright show-trace trace.zip
```

## Project Structure

```
playwright-learning/
├── tests/
│   ├── example.spec.js
│   ├── login.spec.js
│   └── utils/
│       └── helpers.js
├── playwright.config.js
├── package.json
├── .env
└── README.md
```

## Writing Your First Test

Create `tests/first-test.spec.js`:

```javascript
import { test, expect } from '@playwright/test';

test('basic test example', async ({ page }) => {
  await page.goto('https://playwright.dev/');
  
  // Expect a title "to contain" a substring
  await expect(page).toHaveTitle(/Playwright/);
  
  // Expect page to have a heading with the name of Installation
  await expect(page.getByRole('heading', { name: 'Installation' })).toBeVisible();
});
```

## Advanced Configuration

### Custom Fixtures
```javascript
// tests/fixtures.js
import { test as base } from '@playwright/test';

export const test = base.extend({
  loginPage: async ({ page }, use) => {
    // Custom page object or helper
    await use(new LoginPage(page));
  },
});
```

### Global Setup
```javascript
// global-setup.js
async function globalSetup() {
  // Setup before all tests
  console.log('Global setup');
}

export default globalSetup;
```

### Multiple Environments
```javascript
// playwright.config.js - environment-specific configs
const config = {
  // ...base config
  use: {
    baseURL: process.env.BASE_URL || 'http://localhost:3000',
  },
};
```

## Best Practices

### 1. Page Object Model
```javascript
// pages/LoginPage.js
export class LoginPage {
  constructor(page) {
    this.page = page;
    this.usernameInput = page.locator('#username');
    this.passwordInput = page.locator('#password');
    this.loginButton = page.getByRole('button', { name: 'Login' });
  }

  async login(username, password) {
    await this.usernameInput.fill(username);
    await this.passwordInput.fill(password);
    await this.loginButton.click();
  }
}
```

### 2. Test Organization
- Group related tests in describe blocks
- Use meaningful test names
- Keep tests independent
- Use data-testid attributes for reliable selectors

### 3. Assertions
```javascript
// Good assertions
await expect(page.getByTestId('success-message')).toBeVisible();
await expect(page).toHaveURL('/dashboard');
await expect(page.getByRole('heading')).toContainText('Welcome');
```

## Common Commands Reference

| Command | Description |
|---------|-------------|
| `npx playwright test` | Run all tests |
| `npx playwright test --ui` | Run tests in UI mode |
| `npx playwright codegen` | Record tests interactively |
| `npx playwright install` | Install browser binaries |
| `npx playwright show-report` | Open HTML report |

## Debugging

### Visual Debugging
```bash
# Run with browser visible
npx playwright test --headed

# Run in debug mode
npx playwright test --debug

# Record trace for failed tests
npx playwright test --trace on
```

### Console Debugging
```javascript
test('debug example', async ({ page }) => {
  await page.goto('/');
  await page.pause(); // Pauses execution
  console.log(await page.title()); // Log information
});
```

## Resources

- [Official Playwright Documentation](https://playwright.dev/)
- [Playwright API Reference](https://playwright.dev/docs/api/class-playwright)
- [Best Practices Guide](https://playwright.dev/docs/best-practices)
- [Playwright Community Discord](https://aka.ms/playwright/discord)

## Learning Path

1. ✅ Install and setup Playwright
2. ⬜ Write basic tests with assertions
3. ⬜ Learn selectors and locators
4. ⬜ Implement Page Object Model
5. ⬜ Handle authentication and state
6. ⬜ Configure CI/CD integration
7. ⬜ Advanced features (visual testing, API testing)

---

Happy testing! 🎭
