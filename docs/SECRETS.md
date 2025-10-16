# Required Secrets and Configuration

This document describes all the GitHub secrets required for the automation workflows to function properly.

## GitHub Actions Secrets

To set up these secrets, navigate to your repository:
`Settings → Secrets and variables → Actions → New repository secret`

### Required for Release Automation

#### `BOT_GITHUB_PAT`
- **Purpose**: Personal Access Token for automated git operations (commit, push, tag creation)
- **Used by**: 
  - `update_version.yml` - Version updates and tagging
  - `docker-publish.yml` - Triggering deployment workflows
  - `keep-alive.yml` - Keep-alive commits
- **Required Permissions**:
  - `repo` (Full control of repositories)
  - `workflow` (Update GitHub Action workflows)
- **How to create**: See [Troubleshooting Guide](./TROUBLESHOOTING.md#step-1-generate-a-new-personal-access-token)
- **Expiration**: Set to 90 days or 1 year, with calendar reminders before expiration

### Required for Docker Build & Publish

#### `DOCKER_HUB_USERNAME`
- **Purpose**: Docker Hub username for authentication
- **Used by**: `docker-publish.yml`
- **Value**: Your Docker Hub username (e.g., `klastic`)

#### `DOCKER_HUB_ACCESS_TOKEN`
- **Purpose**: Docker Hub access token for secure authentication
- **Used by**: `docker-publish.yml`
- **How to create**:
  1. Log in to Docker Hub
  2. Go to Account Settings → Security → New Access Token
  3. Give it a descriptive name and appropriate permissions
  4. Copy the token and save it as this secret

### Required for Docker Swarm Deployment

#### `SWARM_MANAGER`
- **Purpose**: Hostname or IP address of the Docker Swarm manager node
- **Used by**: `deploy-to-swarm.yml`
- **Example**: `swarm.example.com` or `192.168.1.100`

#### `SWARM_USER`
- **Purpose**: SSH username for connecting to the swarm manager
- **Used by**: `deploy-to-swarm.yml`
- **Example**: `deployer` or `ubuntu`

#### `SWARM_KEY`
- **Purpose**: SSH private key for authentication to swarm manager
- **Used by**: `deploy-to-swarm.yml`
- **Format**: Complete private key including headers
```
-----BEGIN OPENSSH PRIVATE KEY-----
[key content]
-----END OPENSSH PRIVATE KEY-----
```

#### `SWARM_PORT`
- **Purpose**: SSH port for connecting to the swarm manager
- **Used by**: `deploy-to-swarm.yml`
- **Default**: `22` (or your custom SSH port)

#### `DOCKER_CONFIGS_DEPLOY_KEY`
- **Purpose**: SSH deploy key for accessing the private docker-swarm-configs repository
- **Used by**: `deploy-to-swarm.yml`
- **Repository**: `LostKode/docker-swarm-configs`
- **How to create**:
  1. Generate an SSH key pair: `ssh-keygen -t ed25519 -C "deploy-key"`
  2. Add the public key as a deploy key in the config repository
  3. Store the private key in this secret
- **Format**: Complete private key including headers

## Secret Validation Checklist

Use this checklist to verify all secrets are properly configured:

- [ ] `BOT_GITHUB_PAT` - Valid GitHub PAT with repo and workflow permissions
- [ ] `DOCKER_HUB_USERNAME` - Valid Docker Hub username
- [ ] `DOCKER_HUB_ACCESS_TOKEN` - Valid Docker Hub access token
- [ ] `SWARM_MANAGER` - Correct hostname/IP of swarm manager
- [ ] `SWARM_USER` - Valid SSH username for swarm manager
- [ ] `SWARM_KEY` - Valid SSH private key with newlines preserved
- [ ] `SWARM_PORT` - Correct SSH port number
- [ ] `DOCKER_CONFIGS_DEPLOY_KEY` - Valid deploy key for config repository

## Testing Secrets

### Test GitHub PAT
```bash
curl -H "Authorization: token YOUR_TOKEN_HERE" https://api.github.com/user
```

### Test Docker Hub Token
```bash
echo "YOUR_TOKEN_HERE" | docker login -u YOUR_USERNAME --password-stdin
```

### Test SSH Access to Swarm
```bash
ssh -i /path/to/private_key -p PORT USER@SWARM_MANAGER
```

## Secret Rotation Schedule

Regular rotation of secrets improves security:

| Secret | Recommended Rotation | Notes |
|--------|---------------------|-------|
| `BOT_GITHUB_PAT` | Every 6-12 months | Set calendar reminder before expiration |
| `DOCKER_HUB_ACCESS_TOKEN` | Every 12 months | Coordinate with Docker Hub access review |
| `SWARM_KEY` | Every 12 months | Update both private key and authorized_keys |
| `DOCKER_CONFIGS_DEPLOY_KEY` | Every 12 months | Update both secret and repo deploy key |

## Troubleshooting

If you encounter authentication or permission issues, see the [Troubleshooting Guide](./TROUBLESHOOTING.md).

## Security Best Practices

1. **Never commit secrets to the repository**
2. **Use separate tokens for different purposes** (don't reuse tokens)
3. **Grant minimum required permissions** for each token
4. **Regularly audit and rotate secrets**
5. **Monitor workflow runs** for authentication failures
6. **Use GitHub's secret scanning** to detect leaked secrets
7. **Consider using GitHub Apps** instead of PATs for better security
