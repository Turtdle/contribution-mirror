# contribution-mirror

If you work professionally, most of your code lives in private repos on platforms like Bitbucket, GitLab, or private GitHub orgs. None of that activity shows up on your public GitHub profile.

Your contribution graph looks empty. But you've been shipping code every day.

This repo fixes that. A GitHub Action runs daily, pulls your commit history from Bitbucket via the API, and creates backdated empty commits here so your GitHub contribution graph actually reflects your work.

No code is mirrored — just the commit timestamps.

## How it works

1. The workflow scans all repos in your Bitbucket workspace
2. Finds commits matching your author identity
3. Creates empty commits with matching dates
4. Pushes to this repo, filling in your GitHub graph

## Setup

1. Create a Bitbucket App Password with repository read access
2. Base64 encode your credentials: `echo -n "you@email.com:TOKEN" | base64`
3. Store as a GitHub repo secret called `BB_AUTH_B64`
4. Update the author name/email in `.github/workflows/sync.yml` to match yours
5. Make sure the commit email matches a verified email on your GitHub account (or use your GitHub noreply email)
