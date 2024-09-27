# Contributing to TaskMaster

Thank you for considering contributing to **TaskMaster**! Here are some guidelines to ensure your contributions are effective and easy to integrate.

## Git Flow Rules

We follow Git Flow for managing our branches. Here are the basic rules:

- The **main** branch contains the production-ready code and should not be modified directly.
- The **develop** branch is the main branch for integrating new features.
- For any new feature, create a branch named `feature/[feature-name]` based on **develop**.
- To fix a critical issue in production, create a `hotfix/[bug-name]` branch.

## Pull Request Process

1. Create a **pull request** from your **feature** or **hotfix** branch to the **develop** branch (or **main** for hotfixes).
2. Ensure that all tests pass before submitting the PR.
3. Make sure your code follows style guidelines using **Flake8**.
4. Include detailed descriptions of your changes and reference the corresponding issue.

## Rules for Issues

1. Use the **Bug Report** template to report bugs.
2. Use the **Feature Request** template to propose new features.
3. Follow the instructions in the templates to provide all necessary information.

## Linting and Testing

- Before submitting any code, run **Flake8** to check that the code follows our style guide.
- Add unit tests for any new feature or bug fix.
- Make sure all tests pass before submitting your PR.

Thank you again for your contribution!
