# Shared Security Workflows  

This repository contains reusable GitHub Actions workflows designed to improve code quality and security across projects in your organisation.  

## security-pipeline.yml  

The `security-pipeline.yml` workflow runs on pushes and pull requests to the `main` branch. It performs the following jobs:

- **Lint Python** – Installs flake8 and runs it against your Python code to catch style issues and basic errors.  
- **Lint JS/TS** – If a `package.json` exists, installs dependencies via `npm ci` and runs ESLint to enforce consistent JavaScript/TypeScript code style.  
- **Bandit Scan** – Runs Bandit on your Python code using `--severity-level high` to report only high‑severity security issues ([Bandit documentation - Read the Docs](https://bandit.readthedocs.io/en/latest/man/bandit.html#:~:text=B
```
yaml
name: Reuse security pipeline

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  security:
    uses: ginesthoii/security-pipeline/.github/workflows/security-pipeline.yml@main
```

Replace `ginesthoii/security-pipeline` with the owner and repository path where this workflow file resides if your organisation or repository name differs.

2. Commit and push that workflow file to your repository. From then on, GitHub Actions will run the security pipeline on every push and pull request to the specified branches.

### Customisation

You can modify `security-pipeline.yml` in this repository to suit your needs—adjust Python or Node versions, change severity thresholds, add or remove scanning tools, or tweak when the workflow runs. Updates to this workflow will propagate to all repositories that reference it via `uses:` when you update the version (branch or tag).
andit%20documentation%20,EXAMPLES%EF%83%81)). Any high‑severity finding will cause this job to fail.  
- **Semgrep Scan** – Uses the Semgrep recommended CI ruleset (`p/ci`) to detect common security and correctness issues.  
- **Trivy Scan** – Scans the repository’s file system (source files and dependencies) with Trivy. The scan fails when high or critical vulnerabilities are found thanks to the `severity` and `exit-code` settings ([Others](https://trivy.dev/v0.15.0/examples/others/#:~:text=Exit%20Code)).  
- **Secrets Scan** – Uses gitleaks to detect hard‑coded secrets and fails if any are present.  

If any job reports a high‑severity issue, the workflow exits with a non‑zero status. When branch protections are configured, these failures will block merges, helping ensure only secure code is merged.  

### How to reuse this workflow  

To run this security pipeline in another repository:

1. In the target repository, create a new workflow file—for example `.github/workflows/security.yml`—and add the following content:

   ```yaml
   name: Reuse security pipeline

   on:
     push:
       branches: [ "main" ]
     pull_request:
       branches: [ "main" ]

   jobs:
     security:
       uses: ginesthoii/security-pipeline/.github/workflows/security-pipeline.yml@main
   ```

   Replace `ginesthoii/security-pipeline` with the owner and repository path where this workflow file resides if your organisation or repository name differs.

2. Commit and push that workflow file to your repository. From then on, GitHub Actions will run the security pipeline on every push and pull request to the specified branches.  

### Customisation  

You can modify `security-pipeline.yml` in this repository to suit your needs—adjust Python or Node versions, change severity thresholds, add or remove scanning tools, or tweak when the workflow runs. Updates to this workflow will propagate to all repositories that reference it via `uses:` when you update the version (branch or tag).
