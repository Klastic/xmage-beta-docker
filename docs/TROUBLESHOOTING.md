# Troubleshooting Guide

## Release Automation Issues

### Problem: Workflow Authentication Failures

If you see errors like:
```
fatal: could not read Username for 'https://github.com': terminal prompts disabled
```

This indicates that the `BOT_GITHUB_PAT` (Personal Access Token) secret is either:
- Expired
- Invalid
- Not set
- Has insufficient permissions

### Solution: Update the BOT_GITHUB_PAT Secret

#### Step 1: Generate a New Personal Access Token

1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
   - Direct link: https://github.com/settings/tokens
2. Click "Generate new token" → "Generate new token (classic)"
3. Give your token a descriptive name (e.g., "xmage-beta-docker-automation")
4. Set an expiration date (recommended: 90 days or 1 year)
5. Select the following scopes:
   - ✅ `repo` (Full control of private repositories)
     - This includes: repo:status, repo_deployment, public_repo, repo:invite, security_events
   - ✅ `workflow` (Update GitHub Action workflows)
6. Click "Generate token"
7. **Important**: Copy the token immediately - you won't be able to see it again!

#### Step 2: Update the Repository Secret

1. Go to your repository settings
   - Navigate to: `https://github.com/Klastic/xmage-beta-docker/settings/secrets/actions`
2. Find `BOT_GITHUB_PAT` in the list of secrets
3. Click "Update" (or "New repository secret" if it doesn't exist)
4. Paste your new token
5. Click "Update secret"

#### Step 3: Verify the Fix

1. Go to the Actions tab
2. Select the "Update XMage Version" workflow
3. Click "Run workflow" → "Run workflow" button
4. Wait for the workflow to complete
5. Check that it runs successfully without authentication errors

### Affected Workflows

The `BOT_GITHUB_PAT` secret is used by the following workflows:

1. **Update XMage Version** (`.github/workflows/update_version.yml`)
   - Checks out the repository with write access
   - Commits version updates and creates tags
   - Pushes changes to the main branch

2. **Build Docker Image** (`.github/workflows/docker-publish.yml`)
   - Triggers the deployment workflow via repository dispatch
   - Requires workflow permissions

3. **Keep Actions Alive** (`.github/workflows/keep-alive.yml`)
   - Checks out the repository with write access
   - Creates keep-alive commits to prevent workflow deactivation
   - Pushes changes to the main branch

### Token Expiration Prevention

GitHub Personal Access Tokens can expire. To avoid future issues:

1. **Set Calendar Reminders**: Add a reminder 1 week before your token expires
2. **Use Longer Expiration**: Consider using 1-year expiration for production tokens
3. **Monitor Workflow Failures**: Set up notifications for failed workflow runs
4. **Documentation**: Keep this troubleshooting guide updated

### Additional Security Considerations

- **Never commit tokens directly to the repository**
- Use GitHub Secrets for all sensitive credentials
- Regularly rotate tokens (every 6-12 months recommended)
- Use the minimum required permissions for tokens
- Consider using GitHub Apps instead of PATs for better security and granular permissions

### Other Common Issues

#### Issue: Workflows Disabled Due to Inactivity

If workflows stop running after 60 days of repository inactivity:
- The "Keep Actions Alive" workflow should prevent this
- If it fails (due to PAT issues), manually re-enable workflows in repository settings

#### Issue: Docker Hub Authentication Failures

If Docker builds fail with authentication errors:
- Check `DOCKER_HUB_USERNAME` and `DOCKER_HUB_ACCESS_TOKEN` secrets
- Verify Docker Hub access token is still valid
- Update tokens in repository settings

#### Issue: Deployment Failures

If deployment to Docker Swarm fails:
- Verify `SWARM_MANAGER`, `SWARM_USER`, `SWARM_KEY`, `SWARM_PORT` secrets
- Check `DOCKER_CONFIGS_DEPLOY_KEY` for SSH access to private configuration repository
- Test SSH connectivity to the swarm manager manually

## Getting Help

If issues persist after following this guide:
1. Check the Actions tab for detailed error logs
2. Verify all required secrets are set correctly
3. Create an issue in the repository with:
   - The workflow that's failing
   - Complete error messages
   - Steps you've already tried
