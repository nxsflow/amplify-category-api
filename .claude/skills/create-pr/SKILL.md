---
name: create-pr
description: Create pull requests following conventions. Use when opening PRs, writing PR descriptions, or preparing changes for review. Determines target repo (aws-amplify upstream or nxsflow) and follows PR template.
---

# Create Pull Request

Create pull requests following engineering practices.

**Requires**: GitHub CLI (`gh`) authenticated and available.

## Prerequisites

Before creating a PR, ensure all changes are committed. If there are uncommitted changes, run the `committing` skill first.

```bash
git status --porcelain
```

## Step 1: Determine PR Target

Check which branch this work is based on to determine the PR target:

```bash
# Show current branch and its merge base
git branch --show-current
git merge-base --is-ancestor upstream HEAD && echo "Based on upstream" || echo "Based on main"
```

| Based on | PR target | Remote |
|----------|-----------|--------|
| `upstream` | `aws-amplify/amplify-category-api` main | Open PR against upstream repo |
| `main` | `nxsflow/amplify-category-api` main | Open PR against origin |

If unclear, use the AskUserQuestion tool to ask:
> "Should this PR target **upstream** (aws-amplify/amplify-category-api) or **nxsflow** (your fork)?"

**For upstream PRs**: Verify no nxsflow-specific content is included (CLAUDE.md, .claude/, nxsflow packages).

## Step 2: Verify Branch State

```bash
git status
git log main..HEAD --oneline
```

Ensure:
- All changes are committed
- Branch is up to date with remote
- Changes are rebased on the base branch if needed

## Step 3: Analyze Changes

```bash
git log main..HEAD
git diff main...HEAD
```

Understand the scope and purpose of all changes before writing the description.

## Step 4: Write the PR Description

This repo has a PR template with these sections:
- **Description of changes**
- **CDK / CloudFormation Parameters Changed** — list any CFN param changes with docs links
- **Issue #** — link related issues and doc PRs
- **Description of how you validated changes**
- **Checklist**

Follow the template structure.

## Step 5: Create the PR

### For nxsflow PRs (origin):

```bash
gh pr create --title "<type>(<scope>): <description>" --body "$(cat <<'EOF'
<description body here>
EOF
)"
```

### For upstream PRs (aws-amplify):

```bash
# Push to your fork first
git push origin <branch-name>

# Create PR against upstream
gh pr create --repo aws-amplify/amplify-category-api \
  --title "<type>(<scope>): <description>" \
  --body "$(cat <<'EOF'
<description body here>
EOF
)"
```

**Title format** follows commit conventions:
- `feat(scope): add new feature`
- `fix(scope): fix the bug`
- `chore: maintenance task`

## Issue References

| Syntax | Effect |
|--------|--------|
| `Fixes #1234` | Closes GitHub issue on merge |
| `Refs #1234` | Links without closing |

## Guidelines

- **One PR per feature/fix** — don't bundle unrelated changes
- **Keep PRs reviewable** — smaller PRs get faster, better reviews
- **Explain the why** — code shows what; description explains why
- **Mark WIP early** — use draft PRs for early feedback
