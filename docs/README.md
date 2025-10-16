# Documentation Index

This directory contains documentation for troubleshooting and configuring the xmage-beta-docker automation workflows.

## Quick Links

### [!] Fix Issues Now

- **[QUICK_FIX.md](./QUICK_FIX.md)** - 5-minute fix for authentication/automation failures
  - Start here if workflows are currently failing
  - Step-by-step instructions with direct links

### [?] Troubleshooting

- **[TROUBLESHOOTING.md](./TROUBLESHOOTING.md)** - Comprehensive troubleshooting guide
  - Authentication failures
  - Workflow errors
  - Token expiration prevention
  - Security best practices

### [*] Configuration

- **[SECRETS.md](./SECRETS.md)** - Complete secrets and configuration reference
  - All required GitHub secrets
  - How to create and configure each secret
  - Secret rotation schedule
  - Validation checklist

## Common Issues

- **Authentication Errors** - See [QUICK_FIX.md](./QUICK_FIX.md) for expired `BOT_GITHUB_PAT`
- **Docker Build Failures** - Check Docker Hub credentials in [SECRETS.md](./SECRETS.md)
- **Deployment Issues** - Verify swarm secrets in [TROUBLESHOOTING.md](./TROUBLESHOOTING.md)
- **Other Problems** - Full troubleshooting in [TROUBLESHOOTING.md](./TROUBLESHOOTING.md)

## Workflow Overview

The repository uses 4 main workflows:

1. **Update XMage Version** (`.github/workflows/update_version.yml`)
   - Schedule: Daily at 19:00 UTC (cron: "0 19 * * *")
   - Checks for new XMage beta versions
   - Creates version tags automatically
   - Triggers Docker build

2. **Build Docker Image** (`.github/workflows/docker-publish.yml`)
   - Trigger: When new tags are pushed
   - Builds Docker images
   - Publishes to Docker Hub
   - Triggers deployment

3. **Deploy to Docker Swarm** (`.github/workflows/deploy-to-swarm.yml`)
   - Trigger: After successful build or manual dispatch
   - Updates running Docker Swarm stack
   - Can be manually triggered for specific tags

4. **Keep Actions Alive** (`.github/workflows/keep-alive.yml`)
   - Schedule: Weekly on Sundays at 12:00 UTC (cron: "0 12 * * 0")
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
