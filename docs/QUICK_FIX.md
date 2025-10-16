# Quick Fix: Release Automation Not Working

## Problem Identified

Your release automation is failing because the `BOT_GITHUB_PAT` (Personal Access Token) secret has expired or is invalid.

**Error seen in workflow logs:**
```
fatal: could not read Username for 'https://github.com': terminal prompts disabled
```

## Solution (5 minutes)

### 1. Create a New GitHub Personal Access Token

Visit: https://github.com/settings/tokens/new

Configure:
- **Name**: `xmage-beta-docker-automation`
- **Expiration**: 1 year
- **Scopes**: 
  - ✅ `repo` (all checkboxes under repo)
  - ✅ `workflow`

Click "Generate token" and **copy it immediately** (you won't see it again!)

### 2. Update the Repository Secret

1. Go to: https://github.com/Klastic/xmage-beta-docker/settings/secrets/actions
2. Find `BOT_GITHUB_PAT`
3. Click "Update"
4. Paste your new token
5. Click "Update secret"

### 3. Test the Fix

1. Go to: https://github.com/Klastic/xmage-beta-docker/actions/workflows/update_version.yml
2. Click "Run workflow" button (top right)
3. Select "main" branch
4. Click "Run workflow"
5. Wait ~30 seconds and verify it completes successfully (green checkmark)

## Why This Happened

GitHub Personal Access Tokens expire for security. The workflows couldn't authenticate to:
- Check out code
- Commit version updates
- Push tags
- Trigger deployments

## Prevent Future Issues

- **Set a calendar reminder** 1 week before your token expires (check expiration date in GitHub settings)
- **Monitor workflow failures** - enable email notifications for failed Actions
- **Documentation**: Refer to [TROUBLESHOOTING.md](./TROUBLESHOOTING.md) for detailed information

## What Gets Fixed

Once the token is updated, these workflows will work again:

✅ **Update XMage Version** - Daily checks for new versions, auto-tags releases  
✅ **Build Docker Image** - Builds and publishes to Docker Hub on new tags  
✅ **Keep Actions Alive** - Prevents GitHub from disabling workflows  
✅ **Deploy to Swarm** - Deploys updates to your Docker Swarm (triggered after build)

## Need More Help?

See the complete [Troubleshooting Guide](./TROUBLESHOOTING.md) or [Secrets Configuration](./SECRETS.md) for detailed information.
