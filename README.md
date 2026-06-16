# contribution-mirror

Mirror your Bitbucket commit activity to your GitHub contribution graph.

If you work professionally, most of your code lives in private repos on Bitbucket, GitLab, or private GitHub orgs. None of that shows up on your public GitHub profile. Your contribution graph looks empty, but you've been shipping code every day.

This repo fixes that. A GitHub Action runs daily, pulls your commit timestamps from Bitbucket, and creates backdated empty commits so your GitHub graph reflects your actual work.

No code is mirrored. No repo names are exposed. Just timestamps.

## Setup

1. **Fork this repo** (or use it as a template)

2. **Create a Bitbucket App Password**
   - Go to Bitbucket > Personal Settings > App Passwords
   - Create one with **Repositories: Read** permission

3. **Add these secrets** to your fork (Settings > Secrets and variables > Actions):

   | Secret | Value | Example |
   |--------|-------|---------|
   | `BB_AUTH_B64` | Base64 of `email:token` | See below |
   | `BB_WORKSPACE` | Your Bitbucket workspace slug | `mycompany` |
   | `BB_AUTHOR_MATCH` | Pipe-separated strings to match your commits | `Jane Doe\|jane.doe` |
   | `GIT_USER_NAME` | Your GitHub username | `janedoe` |
   | `GIT_USER_EMAIL` | Email that matches your GitHub account | `12345+janedoe@users.noreply.github.com` |

   To generate `BB_AUTH_B64`:
   ```bash
   echo -n "you@email.com:YOUR_APP_PASSWORD" | base64
   ```

4. **Find your GitHub noreply email** for `GIT_USER_EMAIL`:
   - Go to https://github.com/settings/emails
   - Look for `ID+USERNAME@users.noreply.github.com`
   - Or add your work email as a verified email on GitHub

5. **Run it** — go to Actions > "Sync Contributions" > "Run workflow", or wait for the daily run at 6am UTC

## How it works

1. Authenticates with the Bitbucket API
2. Lists all repos in your workspace
3. Scans each repo for commits matching your author identity
4. Compares against commits already in this repo (deduplication)
5. Creates empty commits with matching timestamps
6. Pushes to main

GitHub sees commits on the default branch from a verified email and counts them on your contribution graph.

## FAQ

**Will this expose my company's code or repo names?**
No. The workflow runs inside GitHub Actions. Repo names appear only in the action logs, which are private to your fork. The commits themselves are empty with a generic message.

**How long does the daily sync take?**
Depends on how many repos are in your workspace. ~15-20 minutes for ~300 repos.

**Can I use this with GitLab instead?**
Not yet, but the pattern is the same. PRs welcome.

**Why empty commits?**
GitHub counts any commit on the default branch by a verified email. Empty commits (`--allow-empty`) have no content, so there's nothing to leak.
