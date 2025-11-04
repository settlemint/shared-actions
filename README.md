# Shared GitHub Actions

This repository contains reusable GitHub Actions for common CI/CD workflows. These actions are designed to be used across multiple repositories to maintain consistency and reduce duplication.

**Repository:** [settlemint/shared-actions](https://github.com/settlemint/shared-actions)

## Actions

### 🔄 [auto-merge](./.github/actions/auto-merge)

**Auto-Merge Management** - Automatically manages auto-merge functionality for pull requests based on approval status, QA status, and draft state.

**Inputs:**
- `pr_number` (required): Pull request number
- `pr_author` (required): Pull request author
- `pr_author_type` (optional): Pull request author type (User or Bot), default: "User"
- `has_approval` (required): Whether PR has approval
- `qa_status` (required): QA status (success, failed, running, pending)
- `is_draft` (required): Whether PR is a draft
- `merge_method` (optional): Merge method (merge, squash, rebase), default: "squash"
- `log_level` (optional): Log verbosity (error, warn, info, debug), default: "error"

**Usage:**
```yaml
- uses: settlemint/shared-actions/.github/actions/auto-merge@main
  with:
    pr_number: ${{ github.event.pull_request.number }}
    pr_author: ${{ github.event.pull_request.user.login }}
    has_approval: ${{ steps.check.outputs.has_approval }}
    qa_status: ${{ steps.check.outputs.qa_status }}
    is_draft: ${{ github.event.pull_request.draft }}
```

---

### 🏷️ [build-status-labeler](./.github/actions/build-status-labeler)

**Build Status Labeler** - Updates build status labels on pull requests based on workflow status.

**Inputs:**
- `pr_number` (required): Pull request number
- `workflow_status` (required): Status of the workflow (pending, running, success, failure)
- `log_level` (optional): Log verbosity (error, warn, info, debug), default: "error"

**Usage:**
```yaml
- uses: settlemint/shared-actions/.github/actions/build-status-labeler@main
  with:
    pr_number: ${{ github.event.pull_request.number }}
    workflow_status: ${{ job.status }}
```

---

### 🔒 [codeql](./.github/actions/codeql)

**CodeQL Analysis** - Runs CodeQL security analysis on the codebase.

**Inputs:**
- `language` (required): Language to analyze (e.g., javascript-typescript, python, java)
- `build_mode` (optional): Build mode for CodeQL (none, autobuild, manual), default: "autobuild"
- `queries` (optional): CodeQL query suites to use, default: "security-extended,security-and-quality"

**Usage:**
```yaml
- uses: settlemint/shared-actions/.github/actions/codeql@main
  with:
    language: javascript-typescript
    build_mode: autobuild
```

---

### 🏷️ [pr-labeler](./.github/actions/pr-labeler)

**PR Labeler** - Automatically labels pull requests based on conventional commit format in the PR title and body.

**Inputs:**
- `pr_number` (required): Pull request number
- `pr_title` (required): Pull request title
- `pr_body` (optional): Pull request body, default: ""
- `log_level` (optional): Log verbosity (error, warn, info, debug), default: "error"

**Usage:**
```yaml
- uses: settlemint/shared-actions/.github/actions/pr-labeler@main
  with:
    pr_number: ${{ github.event.pull_request.number }}
    pr_title: ${{ github.event.pull_request.title }}
    pr_body: ${{ github.event.pull_request.body }}
```

---

### ✅ [pr-review-check](./.github/actions/pr-review-check)

**PR Review Check** - Checks PR approval status and determines QA status based on workflow results.

**Inputs:**
- `pr_number` (required): Pull request number
- `pr_author` (required): Pull request author login
- `event_name` (required): GitHub event name
- `qa_result` (optional): QA job result (for pull_request events)
- `secret_scanning_result` (optional): Secret scanning job result (for pull_request events)
- `log_level` (optional): Log verbosity (error, warn, info, debug), default: "error"

**Outputs:**
- `has_approval`: Whether PR has approval
- `qa_status`: QA status (success, failed, running, pending)

**Usage:**
```yaml
- uses: settlemint/shared-actions/.github/actions/pr-review-check@main
  id: review_check
  with:
    pr_number: ${{ github.event.pull_request.number }}
    pr_author: ${{ github.event.pull_request.user.login }}
    event_name: ${{ github.event_name }}
    qa_result: ${{ steps.qa.outcome }}
    secret_scanning_result: ${{ steps.secret_scan.outcome }}
- run: echo "Has approval: ${{ steps.review_check.outputs.has_approval }}"
```

---

### 📊 [pr-status-labeler](./.github/actions/pr-status-labeler)

**PR Status Labeler** - Updates PR status labels based on approval status, QA status, draft state, and merge status.

**Inputs:**
- `pr_number` (required): Pull request number
- `is_draft` (required): Whether the PR is a draft
- `has_approval` (optional): Whether the PR has been approved, default: "false"
- `qa_status` (optional): The QA status (pending, running, success, failed), default: ""
- `is_merged` (optional): Whether the PR has been merged, default: "false"
- `is_abandoned` (optional): Whether the PR has been abandoned (closed without merging), default: "false"
- `log_level` (optional): Log verbosity (error, warn, info, debug), default: "error"

**Usage:**
```yaml
- uses: settlemint/shared-actions/.github/actions/pr-status-labeler@main
  with:
    pr_number: ${{ github.event.pull_request.number }}
    is_draft: ${{ github.event.pull_request.draft }}
    has_approval: ${{ steps.check.outputs.has_approval }}
    qa_status: ${{ steps.check.outputs.qa_status }}
```

---

### 🔐 [secret-scanner](./.github/actions/secret-scanner)

**Secret Scanner** - Scans the codebase for exposed secrets using Trivy.

**Inputs:**
- `trivy_config` (optional): Path to Trivy configuration file, default: ""
- `severity` (optional): Severity levels to check (comma-separated), default: "HIGH,CRITICAL"
- `exit_code` (optional): Exit code when secrets are found, default: "1"
- `log_level` (optional): Log verbosity (error, warn, info, debug), default: "error"

**Usage:**
```yaml
- uses: settlemint/shared-actions/.github/actions/secret-scanner@main
  with:
    severity: HIGH,CRITICAL
    exit_code: 1
```

---

### 🛠️ [setup-dependencies](./.github/actions/setup-dependencies)

**Setup Dependencies** - Sets up all common dependencies and tools for the project including Node.js, Bun, Foundry, Helm, Python, and chart-testing.

**Inputs:**
- `github_token` (required): GitHub token
- `npm_token` (required): NPM registry token
- `disable_node` (optional): Disable Node.js installation, default: "false"

**Usage:**
```yaml
- uses: settlemint/shared-actions/.github/actions/setup-dependencies@main
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    npm_token: ${{ secrets.NPM_TOKEN }}
```

---

### 💬 [slack-pr-notifier](./.github/actions/slack-pr-notifier)

**Slack PR Notifier** - Sends or updates Slack notifications for PR events with labels and status information.

**Inputs:**
- `pr_number` (required): Pull request number
- `pr_title` (required): Pull request title
- `pr_url` (required): Pull request URL
- `pr_author` (required): Pull request author
- `pr_author_type` (optional): Pull request author type (User or Bot), default: "User"
- `pr_author_avatar` (required): Pull request author avatar URL
- `slack_bot_token` (required): Slack bot token
- `slack_channel_id` (required): Slack channel ID
- `update_message` (optional): Whether to update the message format, default: "false"
- `is_abandoned` (optional): Whether the PR was abandoned (closed without merging), default: "false"
- `wait_time` (optional): Time to wait for label propagation in milliseconds, default: "500"
- `reaction_delay_ms` (optional): Delay between Slack reaction operations in milliseconds, default: "50"
- `verification_delay_ms` (optional): Delay before verifying Slack state in milliseconds, default: "300"

**Usage:**
```yaml
- uses: settlemint/shared-actions/.github/actions/slack-pr-notifier@main
  with:
    pr_number: ${{ github.event.pull_request.number }}
    pr_title: ${{ github.event.pull_request.title }}
    pr_url: ${{ github.event.pull_request.html_url }}
    pr_author: ${{ github.event.pull_request.user.login }}
    pr_author_avatar: ${{ github.event.pull_request.user.avatar_url }}
    slack_bot_token: ${{ secrets.SLACK_BOT_TOKEN }}
    slack_channel_id: ${{ secrets.SLACK_CHANNEL_ID }}
```

---

## Usage

To use these actions in your workflows, reference them from the shared repository:

```yaml
- uses: settlemint/shared-actions/.github/actions/action-name@main
  with:
    # action-specific inputs
```

You can also pin to a specific version or branch:
- `@main` - Latest from main branch
- `@v1` - Specific version tag (recommended for production)
- `@sha:abc123` - Specific commit SHA

## Contributing

When adding or modifying actions, ensure:
1. Actions follow the composite action pattern
2. All inputs are properly documented in `action.yml`
3. Actions include appropriate error handling and logging
4. Actions are tested before being merged

