# Bumpify: Automated Dependency Updates

This repository uses [Bumpify](https://github.com/jbigman/bumpify) to automatically update dependencies in `package.json` to their latest versions. The process is fully automated, ensuring that the project stays up-to-date with minimal manual intervention.

## Automated Dependency Bumping Workflow

This workflow runs daily at midnight and can also be triggered manually. It updates project dependencies and creates a pull request with the changes.

```yaml
name: Bump Dependencies
on:
  schedule:
    - cron: '0 0 * * *' # Runs daily at midnight
  workflow_dispatch: # Allows manual triggering

jobs:
  bump-dependencies:
    runs-on: ubuntu-latest
    steps:
      - name: Run Dependency Bump Action
        uses: jbigman/bumpify@v1
        with:
          token: ${{ secrets.PAT_FOR_PULL_REQUEST }}
```

### How It Works
- The workflow runs automatically every day at midnight.
- It can also be triggered manually via GitHub Actions.
- `bumpify` updates dependencies and commits the changes.
- A pull request is created with the updated dependencies.
- The pull request can then be reviewed and merged after testing.

## Testing Workflow for Pull Requests

This workflow runs tests whenever a pull request is created, ensuring that the updated dependencies do not introduce breaking changes.

```yaml
name: Run Tests on Pull Request
on:
  pull_request:

jobs:
  build:
    name: Run Tests
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
      
      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18
      
      - name: Install Dependencies
        run: npm install
      
      - name: Run Tests
        run: npm test
```

### How It Works
- When a pull request is opened, this workflow is triggered.
- It checks out the latest code from the branch.
- Node.js is set up to ensure a consistent testing environment.
- Project dependencies are installed.
- Tests are executed to validate the changes.
- If all tests pass, the pull request is ready for review and merging.

With these workflows in place, dependency management becomes seamless, reducing maintenance overhead while ensuring software stability.

