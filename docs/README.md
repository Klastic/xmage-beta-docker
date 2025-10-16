# Documentation Index

This directory contains documentation for troubleshooting and configuring the xmage-beta-docker automation workflows.

## Quick Links

### 🚨 Fix Issues Now

- **[QUICK_FIX.md](./QUICK_FIX.md)** - 5-minute fix for authentication/automation failures
  - Start here if workflows are currently failing
  - Step-by-step instructions with direct links

### 🔧 Troubleshooting

- **[TROUBLESHOOTING.md](./TROUBLESHOOTING.md)** - Comprehensive troubleshooting guide
  - Authentication failures
  - Workflow errors
  - Token expiration prevention
  - Security best practices

### 🔐 Configuration

- **[SECRETS.md](./SECRETS.md)** - Complete secrets and configuration reference
  - All required GitHub secrets
  - How to create and configure each secret
  - Secret rotation schedule
  - Validation checklist

## Common Issues

### ❌ Workflows Failing with Authentication Errors

**Symptom**: Errors like "fatal: could not read Username for 'https://github.com'"

**Solution**: See [QUICK_FIX.md](./QUICK_FIX.md)

**Root Cause**: Expired or invalid `BOT_GITHUB_PAT` secret

### ❌ Docker Build Failures

**Symptom**: Build workflow fails during Docker Hub push

**Solution**: Check `DOCKER_HUB_USERNAME` and `DOCKER_HUB_ACCESS_TOKEN` in [SECRETS.md](./SECRETS.md)

### ❌ Deployment Not Working

**Symptom**: Deployment workflow fails to connect to Docker Swarm

**Solution**: Verify all swarm-related secrets in [SECRETS.md](./SECRETS.md)

## Workflow Overview

The repository uses 4 main workflows:

1. **Update XMage Version** (`.github/workflows/update_version.yml`)
   - Runs daily at 7 PM UTC
   - Checks for new XMage beta versions
   - Creates version tags automatically
   - Triggers Docker build

2. **Build Docker Image** (`.github/workflows/docker-publish.yml`)
   - Runs when new tags are pushed
   - Builds Docker images
   - Publishes to Docker Hub
   - Triggers deployment

3. **Deploy to Docker Swarm** (`.github/workflows/deploy-to-swarm.yml`)
   - Triggered after successful build
   - Updates running Docker Swarm stack
   - Can be manually triggered

4. **Keep Actions Alive** (`.github/workflows/keep-alive.yml`)
   - Runs weekly on Sundays
   - Prevents workflow deactivation due to inactivity
   - Creates keep-alive commits if needed

## Getting Started

### First Time Setup

1. Review [SECRETS.md](./SECRETS.md) for all required secrets
2. Configure each secret in GitHub repository settings
3. Test workflows by manually triggering them
4. Monitor first runs to ensure everything works

### Maintenance

1. Set calendar reminders for token expiration
2. Monitor workflow run status regularly
3. Follow security best practices in [TROUBLESHOOTING.md](./TROUBLESHOOTING.md)
4. Rotate secrets according to schedule in [SECRETS.md](./SECRETS.md)

## Need Help?

1. Check the relevant documentation file above
2. Review workflow logs in the Actions tab
3. Verify all secrets are properly configured
4. Create an issue with:
   - The workflow that's failing
   - Complete error messages from logs
   - Steps you've already tried

## Contributing

When adding new workflows or modifying automation:

1. Update relevant documentation files
2. Document any new secrets required
3. Add troubleshooting entries for common issues
4. Update this index if adding new documentation files
