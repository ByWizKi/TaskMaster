# Git Hooks in TaskMaster

In this project, we use **Git hooks** to automate tasks and enforce consistency throughout the development process. Git hooks are scripts that run at key points in the Git lifecycle, such as before a commit or before pushing code to the repository.

This document explains the hooks we have in place and how they work.

## Table of Contents
1. [Pre-commit Hook](#pre-commit-hook)
2. [Commit-msg Hook](#commit-msg-hook)
3. [Pre-push Hook](#pre-push-hook)
4. [Post-merge Hook](#post-merge-hook)
5. [Pre-rebase Hook](#pre-rebase-hook)
6. [How to Set Up Hooks Locally](#how-to-set-up-hooks-locally)

---

## Pre-commit Hook

### What it does:
The **pre-commit** hook is triggered before a commit is created. It ensures that the code is properly formatted and linted before it can be committed.

- **Black** is used to automatically format the Python code.
- **Flake8** is used to check for linting errors in Python code.

### How it works:
Before each commit, Black will attempt to format the code, and if it finds issues, it will fix them automatically. Flake8 will then check for linting issues, and if there are any, the commit will be aborted until the issues are fixed.

### Configuration:

```bash
#!/bin/sh

# Run Black to autoformat the code
echo "Running Black to format Python code..."
docker-compose exec -T web black .

if [ $? -ne 0 ]; then
    echo "Black failed to format the code. Commit aborted."
    exit 1
fi

# Run Flake8 to check for linting errors
echo "Running Flake8 to check for linting issues..."
docker-compose exec -T web flake8 .

if [ $? -ne 0 ]; then
    echo "Flake8 detected issues. Commit aborted."
    exit 1
fi

echo "Code formatted and linting passed. Proceeding with commit."
```

This script is located in the `.git/hooks/pre-commit` file. It ensures that only properly formatted and lint-free code is committed to the repository.

---

## Commit-msg Hook

### What it does:
The **commit-msg** hook ensures that every commit message follows a consistent format. This helps keep commit messages clear, traceable, and linked to issues or features.

- The commit message must follow **Conventional Commits** format (e.g., `feat:`, `fix:`, `chore:`).
- The commit message must include an **issue number** (e.g., `#42`).

### Configuration:

```bash
#!/bin/sh

COMMIT_MSG_FILE=$1
COMMIT_MSG=$(cat $COMMIT_MSG_FILE)

# Regex for Conventional Commit format
CONVENTIONAL_REGEX="^(feat|fix|chore|docs|style|refactor|test|perf|build|ci|revert|config|wip): [A-Z].*"

# Check for proper format
if ! echo "$COMMIT_MSG" | grep -qE "$CONVENTIONAL_REGEX"; then
    echo "ERROR: Commit message does not follow the Conventional Commit format."
    echo "Example: 'feat: Add new authentication service'"
    exit 1
fi

# Check for issue number
if ! echo "$COMMIT_MSG" | grep -qE "#[0-9]+"; then
    echo "ERROR: Commit message must include an issue number (e.g., #42)."
    exit 1
fi

exit 0
```

This hook is located in `.git/hooks/commit-msg`. It prevents commits that don't follow the correct format.

---

## Pre-push Hook

### What it does:
The **pre-push** hook runs automatically before you push code to the repository. It is configured to run the Django test suite to ensure no breaking changes are pushed.

### Configuration:

```bash
#!/bin/sh

echo "Running Django tests before pushing..."

docker-compose exec -T web python manage.py test

if [ $? -ne 0 ]; then
    echo "Tests failed. Push aborted."
    exit 1
fi

echo "All tests passed. Proceeding with push."
```

If any tests fail, the push is aborted, preventing broken code from being pushed to the repository.

---

## Post-merge Hook

### What it does:
The **post-merge** hook runs after a branch has been successfully merged. It is used to automatically update dependencies and run database migrations when necessary.

### Configuration:

```bash
#!/bin/sh

echo "Installing any new dependencies..."

docker-compose exec -T web pip install -r requirements.txt

echo "Running Django migrations..."
docker-compose exec -T web python manage.py migrate

echo "Post-merge tasks completed."
```

This hook is useful for keeping your local environment up to date after pulling or merging code.

---

## Pre-rebase Hook

### What it does:
The **pre-rebase** hook is triggered before starting a rebase. It checks if your branch is up to date with the remote branch, ensuring that you’re rebasing with the latest code.

### Configuration:

```bash
#!/bin/sh

echo "Checking if branch is up to date..."

git fetch origin

LOCAL=$(git rev-parse @)
REMOTE=$(git rev-parse @{u})

if [ $LOCAL != $REMOTE ]; then
    echo "Your branch is not up to date with the remote branch."
    echo "Please pull the latest changes before rebasing."
    exit 1
fi

echo "Branch is up to date. Proceeding with rebase."
```

This hook ensures that you're not rebasing on an outdated branch, preventing potential merge conflicts.

---

## How to Set Up Hooks Locally

Hooks are stored locally in each developer's repository under the `.git/hooks/` directory. These scripts are not shared across the team by default, so every developer needs to copy the hooks to their local repository.

### Steps to set up hooks:

1. Copy the provided hook scripts into your local `.git/hooks/` directory.
2. Make sure the scripts are executable:

   ```bash
   chmod +x .git/hooks/pre-commit
   chmod +x .git/hooks/commit-msg
   chmod +x .git/hooks/pre-push
   chmod +x .git/hooks/post-merge
   chmod +x .git/hooks/pre-rebase
   ```

3. The hooks will now run automatically at the specified points in the Git lifecycle.

### Optional: Automating Hook Sharing

To ensure that all team members use the same hooks, you can consider using tools like **`pre-commit`** or include the hooks in a shared folder and add a setup script that copies them to the appropriate location.

---

By following these guidelines, you ensure that the team maintains a consistent development workflow with automated checks at key points during the Git lifecycle. If you have any questions or need further assistance with setting up hooks, don't hesitate to ask!

---

This documentation provides a clear explanation of each hook and how it works. Feel free to include it in a **`docs/hooks.md`** file or directly in your **README**. If you'd like to adjust or add more details, let me know!
