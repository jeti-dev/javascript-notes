---
title: Github actions
layout: default
---
# Github actions

| Feature / Aspect         | Description                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Purpose**              | Automate workflows for CI/CD, testing, deployment, and project management. |
| **Trigger Types**        | Events like `push`, `pull_request`, `schedule`, `workflow_dispatch`, etc.  |
| **Runner Types**         | GitHub-hosted (Linux, Windows, macOS) or self-hosted runners.              |
| **Matrix Builds**        | Run jobs in parallel across different configurations (e.g., Node versions). |
| **Secrets Management**   | Securely store and access sensitive values (tokens, API keys).             |
| **Artifact Storage**     | Save and share files between jobs using `upload-artifact` and `download-artifact`. |
| **Marketplace**          | Thousands of reusable community-contributed actions.                       |
| **Environment Support**  | Supports environment-specific variables, approvals, and deployment gates.  |
| **Container Support**    | Supports Docker containers and container-based builds.                     |
| **Live Logs**            | Real-time logs with highlighting and search functionality.                 |
| **Integration**          | Deep integration with GitHub (pull requests, issues, releases, etc.).      |
| **Pricing**              | Free for public repos, limited free minutes for private repos (more with GitHub Pro/Team). |
| **Limitations**          | Limited concurrency, runtime limits (6h/job), storage limits.              |
| **Use Cases**            | CI/CD, testing, linting, deployment, auto-labeling, bot automation.        |


``` yml
# Name of the GitHub Actions workflow
name: Node.js CI

# Trigger the workflow on push or pull request to the default branch
on:
  push:
    branches: [ $default-branch ]  # Replace with actual branch name or use GitHub defaults
  pull_request:
    branches: [ $default-branch ]

jobs:
  build:
    # Define the operating system the job will run on
    runs-on: ubuntu-latest

    # Define a matrix strategy to test multiple Node.js versions
    strategy:
      matrix:
        node-version: [18.x, 20.x, 22.x]  # Test across multiple Node.js versions

    steps:
      # Checkout the repository code
      - uses: actions/checkout@v4

      # Set up the Node.js environment for the current matrix version
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}  # Dynamically use Node version from matrix
          cache: 'npm'  # Enable automatic caching of npm dependencies

      # Install dependencies using a clean and deterministic install
      - run: npm ci

      # Run build script if it's defined in package.json
      - run: npm run build --if-present

      # Run tests defined in package.json
      - run: npm test

```