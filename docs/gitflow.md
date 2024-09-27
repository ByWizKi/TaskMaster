# Git Flow Guide for TaskMaster

**Git Flow** is a branching model that helps structure and manage the development workflow. It separates development and production code and defines clear roles for different branches and merges. This guide will explain how to use Git Flow in the **TaskMaster** project.

## Table of Contents

1. [Installation](#installation)
2. [Main Concepts](#main-concepts)
   - [Main Branches](#main-branches)
   - [Supporting Branches](#supporting-branches)
3. [How to Use Git Flow](#how-to-use-git-flow)
   - [Start a Feature](#start-a-feature)
   - [Finish a Feature](#finish-a-feature)
   - [Release Branch](#release-branch)
   - [Hotfix Branch](#hotfix-branch)
4. [Git Flow Command Summary](#git-flow-command-summary)

---

## 1. Installation

### a. Install Git Flow

To get started with Git Flow, you need to install it on your machine.

- **On macOS** (via Homebrew):

  ```bash
  brew install git-flow
  ```

- **On Linux** (Debian/Ubuntu):

  ```bash
  sudo apt-get install git-flow
  ```

- **On Windows**: Install it via [Git for Windows SDK](https://github.com/git-for-windows/build-extra/releases) or use **Git Flow AVH** in Git Bash.

---

## 2. Main Concepts

Git Flow defines a strict branching model with two main types of branches: **main branches** and **supporting branches**.

### Main Branches

1. **`main`**:
   - Contains the production-ready code.
   - No direct commits should be made to this branch except for merges from **release** or **hotfix** branches.

2. **`develop`**:
   - This is where the ongoing development happens.
   - All features should be merged into `develop`.
   - When `develop` reaches a stable state, it can be merged into `main`.

### Supporting Branches

1. **Feature branches**:
   - Used for developing new features.
   - These branches always branch off from `develop` and merge back into `develop`.

   Naming convention: `feature/feature-name`

2. **Release branches**:
   - Used to prepare a new production release.
   - These branches are created from `develop` and merged into both `main` and `develop` when ready.

   Naming convention: `release/x.y.z` (where `x.y.z` is the version number)

3. **Hotfix branches**:
   - Used to fix critical issues in the production code.
   - Hotfix branches are created from `main` and are merged back into both `main` and `develop`.

   Naming convention: `hotfix/hotfix-description`

---

## 3. How to Use Git Flow

Below are the typical commands you'll use in Git Flow for **TaskMaster**.

### a. Initialize Git Flow in your project

Before using Git Flow for the first time, you need to initialize it in your repository:

```bash
git flow init
```

You will be prompted to confirm the default branch names. Typically:
- Main branch: `main`
- Development branch: `develop`

Press **Enter** to confirm the default names for other branches.

### b. Start a Feature

When working on a new feature, create a **feature branch**. This isolates the new work from the rest of the project until it's ready to be merged.

1. Create a new feature branch:

   ```bash
   git flow feature start feature-name
   ```

   This command creates and switches you to a new branch called `feature/feature-name` off the `develop` branch.

2. Work on the feature and commit changes as usual:

   ```bash
   git add .
   git commit -m "feat: Add new feature"
   ```

3. Push the feature branch to the remote repository (optional):

   ```bash
   git push origin feature/feature-name
   ```

### c. Finish a Feature

Once the feature is completed and tested, you need to merge it back into `develop`.

1. Finish the feature:

   ```bash
   git flow feature finish feature-name
   ```

   This command:
   - Merges the feature into `develop`.
   - Deletes the feature branch locally.

2. Push the `develop` branch to the remote repository:

   ```bash
   git push origin develop
   ```

---

### d. Release Branch

Once enough features have been completed and you're ready to prepare a new release, you create a **release branch**.

1. Start a new release branch:

   ```bash
   git flow release start x.y.z
   ```

   Replace `x.y.z` with the new version number (e.g., `1.0.0`).

2. Finalize the release (test it, make any necessary final changes, update version numbers, etc.), then finish the release:

   ```bash
   git flow release finish x.y.z
   ```

   This command:
   - Merges the release branch into both `main` and `develop`.
   - Tags the new release in `main`.
   - Deletes the release branch locally.

3. Push the changes to both `main` and `develop`, and push the new tag:

   ```bash
   git push origin main
   git push origin develop
   git push --tags
   ```

---

### e. Hotfix Branch

If a critical issue is found in production, you'll need to create a **hotfix branch** off `main` to fix the issue without waiting for the next release cycle.

1. Start a hotfix branch:

   ```bash
   git flow hotfix start hotfix-description
   ```

   This command creates a new branch `hotfix/hotfix-description` from `main`.

2. After fixing the issue, finish the hotfix:

   ```bash
   git flow hotfix finish hotfix-description
   ```

   This command:
   - Merges the hotfix into both `main` and `develop`.
   - Tags the new release in `main`.
   - Deletes the hotfix branch locally.

3. Push the changes to both `main` and `develop`, and push the new tag:

   ```bash
   git push origin main
   git push origin develop
   git push --tags
   ```

---

## 4. Git Flow Command Summary

Here’s a quick reference for the key Git Flow commands:

| Command                                      | Description                                           |
|----------------------------------------------|-------------------------------------------------------|
| `git flow init`                              | Initialize Git Flow in the repository                 |
| `git flow feature start <feature-name>`      | Start a new feature branch                            |
| `git flow feature finish <feature-name>`     | Finish and merge a feature branch                     |
| `git flow release start <version>`           | Start a release branch                                |
| `git flow release finish <version>`          | Finish and merge a release branch                     |
| `git flow hotfix start <hotfix-name>`        | Start a hotfix branch                                 |
| `git flow hotfix finish <hotfix-name>`       | Finish and merge a hotfix branch                      |

---

### Best Practices

1. **Branch Naming**:
   - Use clear and concise names for features, releases, and hotfixes.
   - Example: `feature/user-auth`, `release/1.0.0`, `hotfix/security-patch`.

2. **Regular Pushes**: Push your feature branches to the remote repository regularly to ensure your work is backed up and available for collaboration.

3. **Pull Requests**: Even when using Git Flow, you can still use **pull requests** to review code before merging features into `develop`.

4. **Frequent Merges**: Regularly merge `develop` into your feature branches to ensure that your work is always up to date with the latest changes.

---

This guide should provide you with a solid understanding of how to use **Git Flow** in the TaskMaster project. By following this workflow, you'll be able to manage multiple features, releases, and hotfixes effectively.

If you have any questions or run into issues, feel free to reach out to the team for help!
