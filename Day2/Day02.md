# Day 2 - Advanced Git & GitHub Management

## 1. Git Reset

`git reset` moves the current branch reference to another commit. Depending on the reset mode, it can also change the staging area and working directory.

### Types of Git Reset

#### A. Soft Reset

A soft reset moves the branch pointer to an earlier commit but keeps the changes staged and available for committing again.

**Command:**

```bash
git reset --soft HEAD~1
```

**Explanation:**
- Moves the current branch back by one commit.
- Keeps changes in the staging area.
- Keeps changes in the working directory.
- Useful when you want to modify or combine recent commits.

**Example:**

```bash
git log --oneline
git reset --soft HEAD~1
git status
```

#### B. Hard Reset

A hard reset moves the branch pointer to another commit and resets both the staging area and working directory to match that commit.

**Command:**

```bash
git reset --hard HEAD~1
```

**Explanation:**
- Moves the current branch back by one commit.
- Removes staged changes and tracked working-directory changes that differ from the target commit.
- Can permanently discard uncommitted work.
- Use carefully because discarded changes may be difficult to recover.

**Example:**

```bash
git log --oneline
git reset --hard HEAD~1
git status
```

**Warning:** Do not use `git reset --hard` unless you are sure you no longer need the affected changes.

---

## 2. Git Revert

`git revert` creates a new commit that reverses the changes introduced by an earlier commit.

Unlike reset, revert preserves the existing commit history.

**Command:**

```bash
git revert <commit-id>
```

**Example:**

```bash
git log --oneline
git revert abc1234
```

Replace `abc1234` with the actual commit ID.

**Explanation:**
- Creates a new commit that undoes the selected commit's changes.
- Preserves the previous commits in the history.
- Suitable for undoing changes in shared branches.
- May require conflict resolution if later changes depend on the reverted changes.

To revert the latest commit:

```bash
git revert HEAD
```

---

## 3. Git Reset vs Git Revert

| Feature | Git Reset | Git Revert |
|---|---|---|
| Purpose | Moves the branch to an earlier commit | Creates a commit that undoes changes |
| History | Rewrites the current branch history | Preserves existing history |
| New commit | Does not create a new commit | Creates a new commit |
| Shared branches | Use cautiously | Generally safer |
| Common use | Undoing local commits | Undoing changes in shared branches |

### When to Use Reset

Use reset when you want to modify recent local commits or remove commits that have not been shared with others.

```bash
git reset --soft HEAD~1
```

### When to Use Revert

Use revert when you want to undo a commit without rewriting shared history.

```bash
git revert <commit-id>
```

---

## 4. Squash Commit

Squashing combines multiple commits into a single commit.

It helps maintain a clean and understandable Git history.

### Interactive Rebase Example

First, check the commit history:

```bash
git log --oneline
```

Start an interactive rebase for the last three commits:

```bash
git rebase -i HEAD~3
```

An editor opens with entries similar to:

```text
pick a111111 First commit
pick b222222 Second commit
pick c333333 Third commit
```

Change the entries to:

```text
pick a111111 First commit
squash b222222 Second commit
squash c333333 Third commit
```

Save and close the editor. Git will let you combine or edit the commit messages.

**Explanation:**
- `pick` keeps a commit.
- `squash` combines a commit with the previous commit and allows you to edit the resulting commit message.
- `fixup` combines a commit with the previous commit without keeping its commit message.

**Note:** Interactive rebase rewrites commit history. Avoid rewriting commits already shared with others unless the team agrees.

---

## 5. Git Cherry-Pick

`git cherry-pick` applies the changes introduced by a specific commit from another branch to the current branch.

### Example

Suppose a commit exists on the `feature` branch and you want to apply it to `main`.

First, find the commit ID:

```bash
git log --oneline feature
```

Switch to the target branch:

```bash
git switch main
```

Apply the selected commit:

```bash
git cherry-pick <commit-id>
```

**Explanation:**
- Applies the changes from the selected commit.
- Creates a new commit on the current branch.
- Does not merge the entire source branch.

If conflicts occur, resolve them and stage the corrected files:

```bash
git add <file-name>
git cherry-pick --continue
```

To cancel the cherry-pick operation:

```bash
git cherry-pick --abort
```

---

## 6. Git Rebase

`git rebase` reapplies commits from one branch on top of another commit or branch.

It helps create a linear commit history.

### Example

Suppose you have a `feature` branch and want to update it with the latest changes from `main`.

```bash
git switch main
git pull origin main
git switch feature
git rebase main
```

**Explanation:**
- Updates the feature branch by replaying its commits on top of `main`.
- Produces a linear history when successful.
- May require conflict resolution.

After resolving conflicts:

```bash
git add <file-name>
git rebase --continue
```

To cancel the rebase:

```bash
git rebase --abort
```

**Important:** Rebase changes commit IDs. Avoid rebasing commits that others are already using.

---

## 7. Git Merge

`git merge` combines changes from one branch into the current branch.

### Example

Merge the `feature` branch into `main`.

```bash
git switch main
git merge feature
```

**Explanation:**
- Integrates changes from the feature branch.
- May create a merge commit when a fast-forward merge is not possible.
- Preserves the existing branch history.
- Can require conflict resolution.

If conflicts occur, resolve them and run:

```bash
git add <file-name>
git commit
```

To cancel an in-progress merge before completing it:

```bash
git merge --abort
```

---

## 8. Git Rebase vs Git Merge

| Feature | Git Rebase | Git Merge |
|---|---|---|
| Purpose | Replays commits on a new base | Integrates branch changes |
| History | Usually linear | May contain merge commits |
| Existing commit IDs | Rewritten for replayed commits | Existing commits are preserved |
| Conflict handling | Conflicts may arise during replay | Conflicts may arise during integration |
| Shared branches | Avoid rewriting shared history | Generally safer for shared branches |

### When to Use Rebase

Use rebase to update your local feature branch with the latest changes from the base branch.

```bash
git switch feature
git rebase main
```

### When to Use Merge

Use merge to integrate completed feature work into another branch while preserving the branch history.

```bash
git switch main
git merge feature
```

**Best practice:** Follow your team's branching and code-review policy before choosing rebase or merge.

---

# GitHub Repository Management

## 9. Repository Creation

A GitHub repository stores source code, documentation, and project history.

### Steps

1. Sign in to GitHub.
2. Select **New repository**.
3. Enter a repository name.
4. Add an optional description.
5. Choose Public or Private.
6. Select repository initialization options.
7. Click **Create repository**.

### Clone a Repository

```bash
git clone <repository-url>
```

Navigate into the repository:

```bash
cd <repository-name>
```

Check the remote repository:

```bash
git remote -v
```

---

## 10. Teams Access and Permission Management

GitHub organizations allow repository access to be managed through teams.

### Common Repository Roles

- **Read:** View and clone repository content.
- **Triage:** Manage issues and pull requests without write access.
- **Write:** Push changes and contribute to the project.
- **Maintain:** Manage repository tasks without full administrative control.
- **Admin:** Manage repository settings and permissions.

### Steps to Manage Team Access

1. Open the GitHub organization.
2. Navigate to the organization settings.
3. Open Teams and create or select a team.
4. Add the required members.
5. Open the team's repository access settings.
6. Select the repository.
7. Assign the appropriate permission level.

**Best practice:** Give users only the permissions they need.

---

## 11. Branch Protection Rules

Branch protection rules help prevent accidental changes to important branches, such as `main`.

### Common Protection Settings

- Require pull requests before merging.
- Require approvals from reviewers.
- Require status checks to pass.
- Require conversation resolution.
- Restrict direct pushes when appropriate.
- Require signed commits when required by organizational policy.

### Steps

1. Open the GitHub repository.
2. Go to **Settings**.
3. Select **Branches** or the repository's applicable rules settings.
4. Create a branch protection rule or ruleset.
5. Specify the target branch, such as `main`.
6. Enable the required protections.
7. Save the configuration.

**Benefits:**
- Protects important branches.
- Encourages code reviews.
- Helps prevent untested changes from being merged.
- Supports consistent development practices.

---

## 12. CODEOWNERS File

A `CODEOWNERS` file specifies which users or teams should review changes to particular files or directories.

### Example

Create a file named `CODEOWNERS` in the `.github` directory.

Example file path:

```text
.github/CODEOWNERS
```

Example content:

```text
# Default owners for the repository
* @your-github-username

# DevOps-related files
/scripts/ @your-github-username

# Documentation files
*.md @your-github-username
```

Replace `your-github-username` with the actual GitHub username.

For an organization team, you can use a pattern such as:

```text
* @your-organization/devops-team
```

Replace `your-organization` and `devops-team` with the actual organization and team names.

**Benefits:**
- Identifies the appropriate reviewers.
- Improves code review ownership.
- Helps teams maintain code quality.

**Note:** CODEOWNERS can request reviews, but automatically required approval depends on the repository's branch protection or ruleset configuration. The file must be in a supported location and accessible on the target branch.

---

# SSH Key Setup

## 13. Generate an SSH Key

SSH allows secure authentication between your computer and GitHub without entering your account password for every Git operation.

### Step 1: Check for Existing SSH Keys

```bash
ls -al ~/.ssh
```

Check whether you already have a key before generating a new one.

### Step 2: Generate a New SSH Key

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Replace the example email with your GitHub email address.

Press Enter to accept the default file location. Set a passphrase when prompted for additional protection.

### Step 3: Start the SSH Agent

```bash
eval "$(ssh-agent -s)"
```

### Step 4: Add the Private Key to the Agent

```bash
ssh-add ~/.ssh/id_ed25519
```

### Step 5: Display the Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the complete public key.

**Security warning:** Never share your private key, `~/.ssh/id_ed25519`. Only copy the public key ending in `.pub`.

### Step 6: Add the Key to GitHub

1. Sign in to GitHub.
2. Open **Settings**.
3. Select **SSH and GPG keys**.
4. Click **New SSH key**.
5. Enter a descriptive title.
6. Paste your public key.
7. Click **Add SSH key**.

### Step 7: Test the Connection

```bash
ssh -T git@github.com
```

If the setup is successful, GitHub should display a message confirming authentication.

### Step 8: Use the SSH Repository URL

Check your remote URL:

```bash
git remote -v
```

If necessary, change the remote to the SSH URL:

```bash
git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
```

Replace `USERNAME` and `REPOSITORY` with the actual values.

---

# Webhooks

## 14. What Are Webhooks?

A webhook is an HTTP callback that sends information to another application when a specified event occurs.

Instead of repeatedly checking for updates, the receiving application can get notified when an event happens.

### How Webhooks Work

1. An event occurs in an application.
2. The application sends an HTTP request to the configured webhook URL.
3. The receiving service processes the request.
4. The service performs the required action.

### Example

When a developer pushes code to GitHub, a webhook can notify a CI/CD server to start a build.

### Uses of Webhooks

#### A. CI/CD Automation

- Trigger automated builds.
- Run automated tests.
- Start deployment pipelines.

#### B. Notifications

- Send notifications when a pull request is created.
- Notify teams when a build fails.
- Send alerts when a deployment completes.

#### C. Integrations

- Connect GitHub with external applications.
- Send events to project management tools.
- Integrate repositories with monitoring and automation systems.

### GitHub Webhook Setup

1. Open the GitHub repository.
2. Go to **Settings**.
3. Select **Webhooks**.
4. Click **Add webhook**.
5. Enter the receiving service's payload URL.
6. Select the content type, such as `application/json`.
7. Select the events that should trigger the webhook.
8. Configure a secret for signature verification.
9. Add the webhook and test its delivery.

**Security best practices:**
- Use HTTPS.
- Keep webhook secrets confidential.
- Verify webhook signatures.
- Subscribe only to necessary events.
- Validate incoming requests before processing them.

---

# Day 2 Summary

In this lesson, I learned:

- Git reset and its soft and hard modes.
- Git revert and how it preserves history.
- Differences between reset and revert.
- How to squash commits using interactive rebase.
- How to apply individual commits using cherry-pick.
- How rebase and merge integrate branch changes.
- Differences between rebase and merge.
- How to manage GitHub repositories, teams, and permissions.
- How to configure branch protection and CODEOWNERS.
- How to generate and configure SSH keys.
- How webhooks support automation, notifications, and integrations.

## Conclusion

Advanced Git and GitHub management help developers maintain clean version histories, collaborate effectively, protect important branches, and automate development workflows.
