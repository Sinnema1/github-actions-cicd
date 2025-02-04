# Tech Quiz CI/CD Pipeline

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Description

**Tech Quiz CI/CD Pipeline** is a full-stack web application designed to provide an interactive quiz experience. It is built using the **MERN stack** and features a fully automated **CI/CD pipeline** using **GitHub Actions**.

This project ensures:
- ✅ **Cypress component tests run automatically** when a PR is made to `develop`.
- ✅ **Continuous Deployment (CD) to Render** when `develop` is merged into `main`.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Testing](#testing)
- [CI/CD Pipeline](#ci/cd-pipeline)
- [Branching Strategy](#branching-strategy)
- [License](#license)
- [Contributing](#contributing)
- [Questions](#questions)

## Installation

To run this project locally:

1. Clone the repository:

   ```bash
   git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
   cd YOUR_REPOSITORY
   ```
2. Install Dependencies:
   ```bash
   npm install
   ```
3. Set up the environment variables (Create a .env file inside server/):
   ```
   MONGODB_URI=your_mongodb_connection_string
   PORT=5000
   ```
4. Build the application:
   ```bash
   npm run build
   ```

## Usage

1. Start the server:
   ```bash
   npm run start
   ```
2.	Open http://localhost:3000 in your browser.
3.	Start the quiz, answer questions, and view your score!

## Testing

**Run Cypress Component Tests Locally**
  ```bash
  npm run cypress:open
  ```
**Run Tests in GitHub Actions (CI/CD)**
- Tests automatically run when a PR is made to develop.
- GitHub Actions will display structured test results.

## CI/CD Pipeline

**1. Testing Workflow (`checking_tests.yml`)**
- **Triggers:** When a PR is made to `develop`.
- **Runs:** Cypress component tests.

**Example Testing Workflow**
```yaml
name: Checking Tests

on:
  pull_request:
    branches:
      - develop

jobs:
  test:
    name: Run Cypress Component Tests
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 21.x

      - name: Install dependencies
        run: npm install

      - name: Build App
        run: npm run build

      - name: Run Cypress Component Tests
        uses: cypress-io/github-action@v5
        with:
          browser: chrome
          headless: true
          component: true
          record: false
          config-file: cypress.config.ts
```
**2. Deployment Workflow (`deploy_to_render.yml`)**
- **Triggers:** When `develop` is merged into `main`.
- **Runs:** Cypress tests **before deployment**.
- **Deploys to:** **Render** using a webhook.

**Example Deployment Workflow**
```yaml
name: Deploy To Render

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  deploy_to_render:
    name: Deploy To Render
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 21.x

      - name: Install dependencies
        run: npm install

      - name: Build App
        run: npm run build

      - name: Run Cypress Component Tests
        run: npx cypress run --component

      - name: Deploy
        if: github.ref == 'refs/heads/main'
        env:
          deploy_url: ${{ secrets.RENDER_DEPLOY_HOOK_URL }}
        run: curl "$deploy_url"
```

## Branching Strategy

| **Branch**  | **Purpose** |
|------------|------------|
| `main`  | Production-ready code, deployed automatically. |
| `develop` | Integration branch where all features merge before going to `main`. |
| `feature/*` | Feature branches merged into `develop`. |

---

## License

This project is licensed under the **MIT License**.

---

## Contributing

1. Fork the repository.  
2. Create a new branch:
   ```bash
   git checkout -b feature/YourFeature
   ```
3.	Commit your changes:
    ```bash
    git commit -m "Add new feature"
    ```
4.	Push to the branch:
    ```bash
    git push origin feature/YourFeature
    ```
5. Open a pull request

## Questions

- **GitHub**: [Sinnema](https://github.com/Sinnema1/github-actions-cicd)
- **Deployed App**: [Link](https://github-actions-cicd.onrender.com)
- **Email**: test@test.com

