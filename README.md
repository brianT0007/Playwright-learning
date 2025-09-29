# Playwright Learning

As part of the QE cross-skilling assesements, this repo contains playwright tests based on the saucedemo website to demonstrate test automation fundamentals

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Running Tests](#running-tests)
- [Project Structure](#project-structure)
- [Resources](#resources)

## Prerequisites

- Node.js (version 23 or higher) - [Download here](https://nodejs.org/)
- npm (version 10 or higher) *automatically included with Node.js*

## Installation

node --version - to verify node v23 or higher is installed
npm version - to verify npm v10 or higher is isnatlled
Clone the repo- git clone https://github.com/brianT0007/Playwright-learning.git
npm install - to install project dependencies 


# Install browser binaries
npx playwright install

### Verify playwright Installation

npx playwright --version

# Run all playwright tests
npx playwright test

# Run specific test file
npx playwright test tests/Saucedemo.spec.ts

# Run specific test 
npx playwright test --grep "About page"

# Run specific test by name/title
npx playwright test --grep "About page"

# Run tests with debugging
Add the debug flag at the end of the test command --debug

# Run tests with headed mode
Add the headed flag at the end of the test command --headed

# Open test results
npx playwright show-report


## Configuration

The main configuration file for this project is playwright.config.js


## Project Structure

playwright-learning/
├── tests/
│   ├── Saucedemo.spec.ts #Main sauce demo tests
├── playwright.config.js #Playwright configuration
├── package.json #Project dependencies
└── README.md

## Resources

- [Official Playwright Documentation](https://playwright.dev/)
- [Sauce demo website](https://www.saucedemo.com/)
- [QE cross-skilling guide](https://www.saucedemo.com/)

