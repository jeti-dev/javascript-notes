---
title: Git
layout: default
---


# Git

## Branching Strategies

| **Strategy** | **Description** | **Main Branches** | **Role of Branches** | **Use Cases** | **Pros** | **Cons** |
|-------------|-----------------|------------------|----------------------|---------------|----------|----------|
| **Feature Branching** | Create a new branch for each feature, merge back to `main` or `develop` when complete | `main`, `feature` | **Feature Branch:** Used to develop a single feature<br>**Main Branch:** Used to store stable production code | New feature development | Isolated development, easy to revert, clear focus | Can lead to long-lived branches, merge conflicts |
| **Git Flow** | Complex strategy with dedicated branches for development, releases, and hotfixes | `main`, `develop`, `feature`, `release`, `hotfix` | **Main Branch:** Stable production code<br>**Develop Branch:** Latest development state<br>**Feature Branch:** Isolated development of new features<br>**Release Branch:** Prepares code for release<br>**Hotfix Branch:** Fixes for production issues | Complex release cycles, large teams | Clear process, structured release management, easy rollback | Overhead for small projects, more complex |
| **GitHub Flow** | Simple strategy based on feature branching and pull requests | `main`, `feature` | **Feature Branch:** Isolated work for a specific feature or fix<br>**Main Branch:** Always in a deployable state | Continuous integration, simple workflow | Fast, simple, encourages small changes | No dedicated release management |
| **GitLab Flow** | Similar to GitHub Flow but includes environment-specific branches | `main`, `staging`, `production`, `feature` | **Main Branch:** Latest development state<br>**Staging Branch:** Prepares code for deployment<br>**Production Branch:** Code running in production<br>**Feature Branch:** Isolated development of features | CI/CD with multiple environments | Clear deployment stages, good for CI/CD | More complex than GitHub Flow |
| **Trunk-Based Development** | Work directly on `main`, create short-lived branches for fixes or features | `main`, `feature` | **Main Branch:** Latest production-ready code<br>**Feature Branch:** Short-lived branches for small features or fixes | Fast delivery, continuous deployment | Fast feedback, simple branching model | Risk of conflicts, requires strict discipline |
| **Release Branching** | Create a release branch from `develop` for final testing and fixes | `main`, `develop`, `release` | **Main Branch:** Stable production code<br>**Develop Branch:** Latest development state<br>**Release Branch:** Final testing and minor fixes | Stable releases, LTS support | Isolate release preparation, easy hotfixing | Increases branch management complexity |
