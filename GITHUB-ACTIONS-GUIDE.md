# ⚙️ GitHub Actions Workflows Guide

Comprehensive guide to automating your GitHub profile with GitHub Actions!

---

## 📋 Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Workflow 1: Blog Posts Auto-Update](#workflow-1-blog-posts-auto-update)
- [Workflow 2: WakaTime Coding Stats](#workflow-2-wakatime-coding-stats)
- [Workflow 3: Snake Contribution Animation](#workflow-3-snake-contribution-animation)
- [Workflow 4: Comprehensive Profile Update](#workflow-4-comprehensive-profile-update)
- [Workflow 5: Spotify Now Playing](#workflow-5-spotify-now-playing)
- [Workflow 6: YouTube Stats](#workflow-6-youtube-stats)
- [Workflow 7: StackOverflow Activity](#workflow-7-stackoverflow-activity)
- [Managing Secrets](#managing-secrets)
- [Testing & Debugging](#testing--debugging)
- [Common Issues & Solutions](#common-issues--solutions)
- [Workflow Status Badges](#workflow-status-badges)
- [Best Practices](#best-practices)

---

## 📖 Introduction

GitHub Actions allows you to automate tasks in your profile repository. This guide covers the most popular and useful workflows for keeping your GitHub profile dynamic and up-to-date.

### What You'll Learn

- How to set up automated blog post updates
- Integrate WakaTime coding statistics
- Generate contribution snake animations
- Display Spotify now playing
- Show YouTube and StackOverflow stats
- Manage secrets securely
- Debug workflow issues

---

## ✅ Prerequisites

Before setting up workflows:

1. ✅ GitHub profile repository created (username/username)
2. ✅ Basic understanding of YAML
3. ✅ GitHub Actions enabled (check Settings → Actions)
4. ✅ Required API keys (for specific workflows)

---

## 📝 Workflow 1: Blog Posts Auto-Update

Automatically update your GitHub profile with your latest blog posts from various platforms.

### Supported Platforms

- Dev.to
- Medium
- Hashnode
- WordPress
- RSS Feeds
- YouTube
- StackOverflow

### Setup Instructions

#### Step 1: Add Placeholder Comments to README

Add these comments where you want blog posts to appear:

```markdown
## 📝 Latest Blog Posts

<!-- BLOG-POST-LIST:START -->
<!-- BLOG-POST-LIST:END -->
```

#### Step 2: Create Workflow File

Create `.github/workflows/blog-post-workflow.yml`:

```yaml
name: Latest Blog Posts

on:
  schedule:
    # Runs every 6 hours
    - cron: '0 */6 * * *'
  workflow_dispatch: # Allows manual trigger

jobs:
  update-readme-with-blog:
    name: Update README with latest blog posts
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Pull latest blog posts
        uses: gautamkrishnar/blog-post-workflow@v1
        with:
          feed_list: "https://dev.to/feed/YOUR_USERNAME,https://medium.com/feed/@YOUR_USERNAME"
          max_post_count: 5
          template: "- [$title]($url)"
          commit_message: "Updated with latest blog posts"
          committer_username: "blog-post-bot"
          committer_email: "blog-post-bot@example.com"
```

### Configuration Options

```yaml
with:
  # Required: List of RSS feed URLs (comma-separated)
  feed_list: "https://dev.to/feed/username,https://medium.com/feed/@username"

  # Optional: Number of posts to show (default: 5)
  max_post_count: 5

  # Optional: Custom template for post format
  template: "- [$title]($url) - $newline Published on $date"

  # Optional: Custom commit message
  commit_message: "Updated with latest blog posts"

  # Optional: Filter keywords (show only posts with these words)
  filter_keywords: "javascript,react,typescript"

  # Optional: Date format
  date_format: "mmm dd, yyyy"

  # Optional: Show reading time
  show_reading_time: true

  # Optional: Custom placeholder
  comment_tag_name: "BLOG-POST-LIST"
```

### Multiple Feed Lists

You can have multiple sections with different feeds:

```markdown
## 📝 Dev.to Posts
<!-- DEV-POST-LIST:START -->
<!-- DEV-POST-LIST:END -->

## 📝 Medium Articles
<!-- MEDIUM-POST-LIST:START -->
<!-- MEDIUM-POST-LIST:END -->
```

Workflow for multiple sections:

```yaml
- name: Update Dev.to posts
  uses: gautamkrishnar/blog-post-workflow@v1
  with:
    feed_list: "https://dev.to/feed/YOUR_USERNAME"
    comment_tag_name: "DEV-POST-LIST"

- name: Update Medium posts
  uses: gautamkrishnar/blog-post-workflow@v1
  with:
    feed_list: "https://medium.com/feed/@YOUR_USERNAME"
    comment_tag_name: "MEDIUM-POST-LIST"
```

---

## ⏱️ Workflow 2: WakaTime Coding Stats

Display your coding activity statistics from WakaTime.

### Prerequisites

1. Sign up at [WakaTime](https://wakatime.com)
2. Install WakaTime plugin in your IDE
3. Get your WakaTime API key from [settings page](https://wakatime.com/settings/api-key)

### Setup Instructions

#### Step 1: Add WakaTime Secret

1. Go to your profile repository → Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Name: `WAKATIME_API_KEY`
4. Value: Your WakaTime API key
5. Click "Add secret"

#### Step 2: Add Placeholder to README

```markdown
## ⏱️ Weekly Coding Stats

<!--START_SECTION:waka-->
<!--END_SECTION:waka-->
```

#### Step 3: Create Workflow File

Create `.github/workflows/waka-readme.yml`:

```yaml
name: WakaTime Stats

on:
  schedule:
    # Runs at 00:00 UTC every day
    - cron: '0 0 * * *'
  workflow_dispatch:

jobs:
  update-readme:
    name: Update README with WakaTime stats
    runs-on: ubuntu-latest
    steps:
      - name: Update WakaTime stats
        uses: athul/waka-readme@master
        with:
          WAKATIME_API_KEY: ${{ secrets.WAKATIME_API_KEY }}
          SHOW_TITLE: true
          BLOCKS: ⣀⣄⣤⣦⣶⣷⣿
          TIME_RANGE: last_7_days
          SHOW_TIME: true
          SHOW_TOTAL: true
```

### Advanced Configuration

```yaml
with:
  WAKATIME_API_KEY: ${{ secrets.WAKATIME_API_KEY }}

  # Show chart title
  SHOW_TITLE: true

  # Custom title
  SECTION_NAME: "📊 Weekly Development Breakdown"

  # Progress blocks style
  BLOCKS: ⣀⣄⣤⣦⣶⣷⣿
  # Or: BLOCKS: ░▒▓█

  # Time range: last_7_days, last_30_days, last_6_months, last_year, all_time
  TIME_RANGE: last_7_days

  # Show time spent
  SHOW_TIME: true

  # Show total time
  SHOW_TOTAL: true

  # Show masked time (hides actual hours)
  SHOW_MASKED_TIME: false

  # Number of languages to display
  LANG_COUNT: 10
```

### Example Output

```
📊 Weekly Development Breakdown

TypeScript   8 hrs 30 mins   ████████████░░░░░   48.2%
JavaScript   4 hrs 15 mins   ██████░░░░░░░░░░░   24.1%
Python       2 hrs 45 mins   ████░░░░░░░░░░░░░   15.6%
Go           1 hr 30 mins    ██░░░░░░░░░░░░░░░    8.5%
Other        40 mins         █░░░░░░░░░░░░░░░░    3.6%
```

---

## 🐍 Workflow 3: Snake Contribution Animation

Generate an animated snake eating your GitHub contributions!

### Setup Instructions

#### Step 1: Create Workflow File

Create `.github/workflows/snake.yml`:

```yaml
name: Generate Snake Animation

on:
  # Runs every 12 hours
  schedule:
    - cron: "0 */12 * * *"

  # Allows manual trigger
  workflow_dispatch:

  # Runs on every push to main
  push:
    branches:
    - main

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10

    steps:
      - name: Generate github-contribution-grid-snake.svg
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - name: Push to output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

#### Step 2: Enable GitHub Pages (for output branch)

1. Go to Settings → Pages
2. Source: Deploy from a branch
3. Branch: `output`
4. Folder: `/ (root)`
5. Save

#### Step 3: Add to README

```markdown
## 🐍 Contribution Snake

![Snake animation](https://github.com/YOUR_USERNAME/YOUR_USERNAME/blob/output/github-contribution-grid-snake.svg)

<!-- For dark mode -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/YOUR_USERNAME/YOUR_USERNAME/blob/output/github-contribution-grid-snake-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/YOUR_USERNAME/YOUR_USERNAME/blob/output/github-contribution-grid-snake.svg">
  <img alt="github contribution grid snake animation" src="https://github.com/YOUR_USERNAME/YOUR_USERNAME/blob/output/github-contribution-grid-snake.svg">
</picture>
```

### Customization Options

```yaml
with:
  github_user_name: ${{ github.repository_owner }}

  # Custom color palette
  outputs: |
    dist/github-snake.svg?color_snake=orange&color_dots=#bfd6f6,#8dbdff,#64a1f4,#4b91f1,#3c7dd9

  # Grid size
  gif_out_path: dist/github-snake.gif?grid_size=20

  # Frame rate
  outputs: dist/github-snake.svg?frame_rate=60
```

---

## 🔄 Workflow 4: Comprehensive Profile Update

A comprehensive workflow that updates multiple sections of your profile.

Create `.github/workflows/update-profile.yml`:

```yaml
name: Update Profile

on:
  schedule:
    - cron: '0 */6 * * *'
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v3

      # Update blog posts
      - name: Update blog posts
        uses: gautamkrishnar/blog-post-workflow@v1
        with:
          feed_list: "https://dev.to/feed/YOUR_USERNAME"
          comment_tag_name: "BLOG-POST-LIST"

      # Update WakaTime stats
      - name: Update WakaTime stats
        uses: athul/waka-readme@master
        with:
          WAKATIME_API_KEY: ${{ secrets.WAKATIME_API_KEY }}

      # Update GitHub stats
      - name: Update GitHub stats
        uses: jamesgeorge007/github-activity-readme@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          COMMIT_MSG: 'Updated GitHub activity'
          MAX_LINES: 10
```

---

## 🎵 Workflow 5: Spotify Now Playing

Display your currently playing or recently played Spotify track.

### Prerequisites

1. Create Spotify App at [Spotify Dashboard](https://developer.spotify.com/dashboard)
2. Get Client ID and Client Secret
3. Deploy [novatorem](https://github.com/novatorem/novatorem) to Vercel

### Setup Instructions

#### Step 1: Deploy to Vercel

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/novatorem/novatorem)

1. Click "Deploy with Vercel"
2. Sign in with GitHub
3. Clone the repository
4. Add environment variables:
   - `SPOTIFY_CLIENT_ID`
   - `SPOTIFY_CLIENT_SECRET`
   - `SPOTIFY_REFRESH_TOKEN`
5. Deploy

#### Step 2: Get Refresh Token

1. Go to your Vercel deployment URL + `/api/login`
2. Authorize Spotify
3. Copy the refresh token
4. Add to Vercel environment variables

#### Step 3: Add to README

```markdown
## 🎵 Spotify Playing

[![Spotify](https://novatorem-YOUR_VERCEL_APP.vercel.app/api/spotify)](https://open.spotify.com/user/YOUR_SPOTIFY_USER)
```

### Alternative: Spotify GitHub Profile

```markdown
[![Spotify](https://spotify-github-profile.vercel.app/api/view?uid=YOUR_SPOTIFY_ID&cover_image=true&theme=default&show_offline=false&background_color=121212&interchange=false&bar_color=53b14f&bar_color_cover=true)](https://spotify-github-profile.vercel.app/api/view?uid=YOUR_SPOTIFY_ID&redirect=true)
```

---

## 📹 Workflow 6: YouTube Stats

Display your YouTube channel statistics.

### Setup Instructions

#### Step 1: Get YouTube API Key

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project
3. Enable YouTube Data API v3
4. Create credentials (API Key)

#### Step 2: Add Secret

Add `YOUTUBE_API_KEY` to repository secrets.

#### Step 3: Add Placeholder to README

```markdown
## 📹 YouTube Stats

<!-- YOUTUBE:START -->
<!-- YOUTUBE:END -->
```

#### Step 4: Create Workflow

Create `.github/workflows/youtube-workflow.yml`:

```yaml
name: Latest YouTube Videos

on:
  schedule:
    - cron: '0 */12 * * *'
  workflow_dispatch:

jobs:
  update-readme-with-youtube:
    name: Update README with latest YouTube videos
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - uses: gautamkrishnar/blog-post-workflow@v1
        with:
          comment_tag_name: "YOUTUBE"
          feed_list: "https://www.youtube.com/feeds/videos.xml?channel_id=YOUR_CHANNEL_ID"
          max_post_count: 5
          template: "- [$title]($url) - $newline  Views: $views | Published: $date"
```

---

## 💬 Workflow 7: StackOverflow Activity

Show your recent StackOverflow activity.

### Setup Instructions

#### Step 1: Add Placeholder

```markdown
## 💬 StackOverflow Activity

<!-- STACKOVERFLOW:START -->
<!-- STACKOVERFLOW:END -->
```

#### Step 2: Create Workflow

Create `.github/workflows/stackoverflow.yml`:

```yaml
name: Latest StackOverflow Activity

on:
  schedule:
    - cron: '0 */6 * * *'
  workflow_dispatch:

jobs:
  update-stackoverflow:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - uses: gautamkrishnar/blog-post-workflow@v1
        with:
          comment_tag_name: "STACKOVERFLOW"
          feed_list: "https://stackoverflow.com/feeds/user/YOUR_USER_ID"
          max_post_count: 5
          template: "- [$title]($url)"
```

Find your StackOverflow User ID: Go to your profile, the number in the URL is your ID.

---

## 🔐 Managing Secrets

### What Are GitHub Secrets?

GitHub Secrets are encrypted environment variables used to store sensitive information like API keys.

### How to Add Secrets

1. Go to your repository
2. Click **Settings**
3. Navigate to **Secrets and variables** → **Actions**
4. Click **New repository secret**
5. Add:
   - **Name:** SECRET_NAME
   - **Value:** Your secret value
6. Click **Add secret**

### Common Secrets

| Secret Name | Used For | Where to Get |
|-------------|----------|--------------|
| `WAKATIME_API_KEY` | WakaTime stats | [WakaTime Settings](https://wakatime.com/settings/api-key) |
| `SPOTIFY_CLIENT_ID` | Spotify integration | [Spotify Dashboard](https://developer.spotify.com/dashboard) |
| `SPOTIFY_CLIENT_SECRET` | Spotify integration | [Spotify Dashboard](https://developer.spotify.com/dashboard) |
| `SPOTIFY_REFRESH_TOKEN` | Spotify integration | OAuth flow |
| `YOUTUBE_API_KEY` | YouTube stats | [Google Cloud Console](https://console.cloud.google.com/) |
| `GITHUB_TOKEN` | GitHub API | Automatically provided |

### Using Secrets in Workflows

```yaml
env:
  WAKATIME_API_KEY: ${{ secrets.WAKATIME_API_KEY }}
```

### Security Best Practices

1. ✅ Never commit secrets to your repository
2. ✅ Use GitHub Secrets for sensitive data
3. ✅ Rotate API keys periodically
4. ✅ Use minimal permissions for tokens
5. ✅ Review workflow logs for exposed secrets
6. ❌ Don't echo secrets in workflow outputs
7. ❌ Don't use secrets in pull request workflows from forks

---

## 🧪 Testing & Debugging

### Manual Trigger

Add `workflow_dispatch` to allow manual runs:

```yaml
on:
  schedule:
    - cron: '0 */6 * * *'
  workflow_dispatch: # Enables manual trigger
```

To manually run:
1. Go to **Actions** tab
2. Select the workflow
3. Click **Run workflow**
4. Click **Run workflow** button

### Debugging Workflows

#### 1. View Workflow Logs

1. Go to **Actions** tab
2. Click on the workflow run
3. Click on the job
4. Expand steps to view logs

#### 2. Add Debug Output

```yaml
- name: Debug information
  run: |
    echo "Current directory: $(pwd)"
    echo "Files in directory:"
    ls -la
    echo "README.md preview:"
    head -n 20 README.md
```

#### 3. Enable Debug Logging

Add these secrets to enable verbose logging:
- `ACTIONS_STEP_DEBUG` = `true`
- `ACTIONS_RUNNER_DEBUG` = `true`

#### 4. Test Locally with act

Install [act](https://github.com/nektos/act) to run workflows locally:

```bash
# Install act
brew install act  # macOS
# or
curl https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo bash

# Run workflow
act -j job_name

# Run with secrets
act -j job_name -s GITHUB_TOKEN=your_token
```

### Common Debugging Commands

```yaml
# Print environment variables
- run: env | sort

# Show git status
- run: git status

# Show git diff
- run: git diff

# Validate YAML
- run: yamllint .github/workflows/*.yml
```

---

## 🔧 Common Issues & Solutions

### Issue 1: Workflow Not Running on Schedule

**Problem:** Scheduled workflows don't run.

**Solutions:**
1. Ensure repository is active (push something)
2. Workflows may be delayed during high load
3. Check if Actions are enabled in repository settings
4. Inactive repos have workflows automatically disabled after 60 days

### Issue 2: Secrets Not Working

**Problem:** Workflow can't access secrets.

**Solutions:**
1. Verify secret name matches exactly (case-sensitive)
2. Check secret is added to correct repository
3. Ensure using `${{ secrets.SECRET_NAME }}` syntax
4. Secrets aren't available in forked repositories

### Issue 3: README Not Updating

**Problem:** README.md isn't being updated.

**Solutions:**
1. Verify placeholder comments exist: `<!-- TAG:START -->` and `<!-- TAG:END -->`
2. Check commit permissions:
   ```yaml
   permissions:
     contents: write
   ```
3. Ensure workflow has write access to repository
4. Check for errors in Actions logs

### Issue 4: Rate Limiting

**Problem:** API rate limits exceeded.

**Solutions:**
1. Reduce workflow frequency (increase cron interval)
2. Use `GITHUB_TOKEN` for authenticated requests
3. Cache API responses when possible
4. Implement exponential backoff

### Issue 5: Snake Animation Not Generating

**Problem:** Snake SVG not created.

**Solutions:**
1. Verify `output` branch exists
2. Check GitHub Pages is enabled for `output` branch
3. Ensure `GITHUB_TOKEN` has proper permissions
4. Wait for workflow to complete (check Actions tab)
5. Verify the SVG URL is correct (username, branch name)

### Issue 6: WakaTime Stats Not Showing

**Problem:** WakaTime section is empty.

**Solutions:**
1. Verify API key is correct
2. Ensure WakaTime plugin is installed and active in IDE
3. Check if you have logged time in the selected time range
4. Wait 24 hours after installing WakaTime for data to populate

---

## 📛 Workflow Status Badges

Add status badges to show if workflows are working:

```markdown
![Blog Post Workflow](https://github.com/YOUR_USERNAME/YOUR_USERNAME/actions/workflows/blog-post-workflow.yml/badge.svg)
![WakaTime](https://github.com/YOUR_USERNAME/YOUR_USERNAME/actions/workflows/waka-readme.yml/badge.svg)
![Snake](https://github.com/YOUR_USERNAME/YOUR_USERNAME/actions/workflows/snake.yml/badge.svg)
```

### Customized Status Badges

```markdown
![Status](https://github.com/YOUR_USERNAME/YOUR_USERNAME/actions/workflows/workflow.yml/badge.svg?branch=main&event=push)
```

---

## ✨ Best Practices

### 1. Workflow Optimization

```yaml
# Good: Specific schedule
on:
  schedule:
    - cron: '0 */6 * * *'  # Every 6 hours

# Better: Include manual trigger
on:
  schedule:
    - cron: '0 */6 * * *'
  workflow_dispatch:  # Allows testing

# Best: Include conditions
on:
  schedule:
    - cron: '0 */6 * * *'
  workflow_dispatch:
  push:
    branches:
      - main
    paths:
      - '.github/workflows/**'
```

### 2. Permissions

Use minimal required permissions:

```yaml
permissions:
  contents: write  # Only allow writing to repository

# Instead of:
permissions: write-all  # Too permissive
```

### 3. Caching

Cache dependencies to speed up workflows:

```yaml
- name: Cache dependencies
  uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

### 4. Error Handling

```yaml
- name: Update stats
  continue-on-error: true  # Don't fail entire workflow
  uses: some/action@v1

- name: Notify on failure
  if: failure()
  run: echo "Workflow failed!"
```

### 5. Workflow Organization

```
.github/
└── workflows/
    ├── blog-posts.yml          # Blog post updates
    ├── waka-readme.yml         # WakaTime stats
    ├── snake.yml               # Snake animation
    ├── profile-update.yml      # Combined updates
    └── test.yml                # Testing workflow
```

### 6. Documentation

Add comments to workflows:

```yaml
name: Update Profile

# Runs every 6 hours to keep profile fresh
on:
  schedule:
    - cron: '0 */6 * * *'

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      # Checkout repository with full history
      - name: Checkout
        uses: actions/checkout@v3
        with:
          fetch-depth: 0
```

### 7. Monitoring

Set up notifications for workflow failures:

```yaml
- name: Slack Notification
  if: failure()
  uses: 8398a7/action-slack@v3
  with:
    status: ${{ job.status }}
    webhook_url: ${{ secrets.SLACK_WEBHOOK }}
```

### 8. Version Pinning

Pin action versions for stability:

```yaml
# Good: Specific version
uses: actions/checkout@v3

# Better: Commit SHA (most secure)
uses: actions/checkout@8e5e7e5ab8b370d6c329ec480221332ada57f0ab

# Avoid: Latest (unpredictable)
uses: actions/checkout@latest
```

---

## 📊 Workflow Performance Tips

### 1. Parallel Jobs

Run independent tasks in parallel:

```yaml
jobs:
  update-blog:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Update blog posts
        uses: gautamkrishnar/blog-post-workflow@v1

  update-waka:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Update WakaTime
        uses: athul/waka-readme@master
```

### 2. Conditional Execution

Only run when needed:

```yaml
- name: Update stats
  if: github.event_name == 'schedule'
  uses: some/action@v1

- name: Test workflow
  if: github.event_name == 'workflow_dispatch'
  run: echo "Manual test run"
```

### 3. Timeout Settings

Prevent hanging workflows:

```yaml
jobs:
  update:
    runs-on: ubuntu-latest
    timeout-minutes: 10  # Fail after 10 minutes
```

---

## 🎓 Learning Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Awesome Actions](https://github.com/sdras/awesome-actions)
- [Actions Marketplace](https://github.com/marketplace?type=actions)
- [GitHub Actions Community](https://github.community/c/actions/41)

---

## 📝 Checklist

Use this checklist to verify your workflows are set up correctly:

- [ ] GitHub Actions enabled in repository settings
- [ ] Workflow files created in `.github/workflows/`
- [ ] Placeholders added to README.md
- [ ] Required secrets added to repository
- [ ] Workflows tested with manual trigger
- [ ] Status badges added to README (optional)
- [ ] Workflow permissions configured correctly
- [ ] Error handling implemented
- [ ] Scheduled runs set to reasonable intervals
- [ ] Documentation added for custom workflows

---

**Happy automating! 🤖**

_If you found this helpful, give it a ⭐ and share with others!_
