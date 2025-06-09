# `ak export_blueprint` Command Reference

[![authentik][authentik-shield]][authentik-url]
[![Documentation][docs-shield]][docs-url]
[![Discord][discord-shield]][discord-url]
[![GitHub Release][github-release-shield]][github-release-url]

> **Export your Authentik configuration as code with the `ak export_blueprint` management command**

The `ak export_blueprint` command enables you to export your entire Authentik instance configuration as [Blueprint](https://docs.goauthentik.io/docs/customize/blueprints/) YAML files, supporting infrastructure-as-code workflows, configuration backups, and multi-environment deployments.

---

## 🚀 Quick Start

```bash
# Docker Compose
docker-compose exec worker ak export_blueprint > my-config.yaml

# Kubernetes
kubectl exec -it deployment/authentik-worker -- ak export_blueprint > my-config.yaml

# Docker
docker exec authentik-worker ak export_blueprint > my-config.yaml
```

## 📖 Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Usage](#usage)
- [Export Scope](#export-scope)
- [Output Format](#output-format)
- [Security Considerations](#security-considerations)
- [Troubleshooting](#troubleshooting)
- [Examples](#examples)
- [Best Practices](#best-practices)
- [Contributing](#contributing)

## Overview

Blueprints are Authentik's infrastructure-as-code solution that allows you to:

- **🔄 Version Control**: Track configuration changes over time
- **🚀 Automated Deployments**: Bootstrap new instances with existing configurations
- **🔧 Environment Consistency**: Maintain identical setups across dev/staging/prod
- **💾 Backup & Recovery**: Create point-in-time configuration snapshots
- **📦 Distribution**: Share common configurations across teams

## Prerequisites

| Requirement     | Details                                           |
| --------------- | ------------------------------------------------- |
| **Access**      | Shell access to an Authentik worker container     |
| **Permissions** | Container user must have database read access     |
| **Version**     | Authentik 2022.8+ (Blueprint support)             |
| **Resources**   | Sufficient memory for large configuration exports |

## Usage

### Basic Export

The command must be executed from within an Authentik worker container:

```bash
ak export_blueprint
```

### Container Access Methods

<details>
<summary><strong>Docker Compose</strong></summary>

```bash
# Interactive shell
docker-compose exec worker bash
ak export_blueprint

# Direct execution
docker-compose exec worker ak export_blueprint
```

</details>

<details>
<summary><strong>Kubernetes</strong></summary>

```bash
# List worker pods
kubectl get pods -l app.kubernetes.io/name=authentik-worker

# Execute command
kubectl exec -it <worker-pod-name> -- ak export_blueprint

# Save to file
kubectl exec -it <worker-pod-name> -- ak export_blueprint > blueprint.yaml
```

</details>

<details>
<summary><strong>Docker</strong></summary>

```bash
# Find worker container
docker ps | grep authentik-worker

# Execute command
docker exec <container-name> ak export_blueprint

# Save to file
docker exec <container-name> ak export_blueprint > blueprint.yaml
```

</details>

### Output Redirection

```bash
# Save to file
ak export_blueprint > my-authentik-backup.yaml

# Add timestamp
ak export_blueprint > "backup-$(date +%Y%m%d-%H%M%S).yaml"

# Compress output
ak export_blueprint | gzip > backup.yaml.gz
```

## Export Scope

### ✅ Included Objects

The export includes most configuration objects:

| Category              | Objects                                         |
| --------------------- | ----------------------------------------------- |
| **Identity**          | Users, Groups, Tokens                           |
| **Applications**      | Applications, Providers (OAuth2, SAML, SCIM)    |
| **Flows**             | Authentication, Authorization, Enrollment flows |
| **Stages**            | All stage types and configurations              |
| **Policies**          | Expression, Group, Reputation policies          |
| **Sources**           | OAuth, SAML, LDAP sources                       |
| **Property Mappings** | Scope, SAML, SCIM mappings                      |
| **System**            | Tenants, Brands, Certificates                   |

### ❌ Excluded Objects

| Object Type          | Reason                                                 |
| -------------------- | ------------------------------------------------------ |
| **Secrets**          | Write-only fields (OAuth client secrets, private keys) |
| **Temporary Data**   | Sessions, audit logs, cache entries                    |
| **Dependencies**     | Objects with unresolvable external dependencies        |
| **System Generated** | Auto-created objects that shouldn't be replicated      |

### ⚠️ Limitations

- **Hardcoded Primary Keys**: Exported objects contain database-specific IDs
- **No Templating**: Exports don't include YAML tags or variable substitution
- **Default Values**: Omitted from output to reduce file size
- **Cross-References**: May need manual resolution between related objects

## Output Format

### Structure

```yaml
# Example export structure
version: 1
metadata:
  name: "authentik-export"
entries:
  - model: authentik_core.application
    pk: 1
    attrs:
      name: "My Application"
      slug: "my-app"
      provider: 2

  - model: authentik_providers_oauth2.oauth2provider
    pk: 2
    attrs:
      name: "OAuth Provider"
      client_id: "my-client-id"
      # client_secret omitted (write-only)
```

### File Characteristics

- **Format**: YAML with specific Authentik schema
- **Size**: Varies by configuration complexity (typically 10KB-10MB)
- **Encoding**: UTF-8
- **Compatibility**: Authentik Blueprint v1 format

## Security Considerations

### 🔒 Protected Information

The export **automatically excludes** sensitive fields:

```yaml
# ❌ NOT exported
client_secret: "secret-value"
private_key: "-----BEGIN PRIVATE KEY-----"
password: "hashed-password"

# ✅ Exported
client_id: "public-client-id"
public_key: "-----BEGIN PUBLIC KEY-----"
username: "admin"
```

### 🛡️ Best Practices

- **Storage**: Store exported files in secure, access-controlled locations
- **Transmission**: Use encrypted channels for file transfer
- **Versioning**: Commit to private repositories only
- **Cleanup**: Remove old exports regularly
- **Access**: Limit export permissions to necessary personnel

> **⚠️ Warning**: While sensitive fields are excluded, exported configurations may still contain PII or organizational structure information.

## Troubleshooting

### Common Issues

<details>
<summary><strong>Permission Denied</strong></summary>

```bash
# Error: permission denied
# Solution: Check container user permissions
docker-compose exec --user root worker whoami
```

</details>

<details>
<summary><strong>Database Connection Errors</strong></summary>

```bash
# Error: database connection failed
# Solution: Verify database connectivity
docker-compose exec worker ak check --database
```

</details>

<details>
<summary><strong>Export Failures</strong></summary>

```bash
# Error: KeyError: 'serializer'
# Common in versions 2024.x-2025.x
# Workaround: Try exporting specific flows via UI
```

**Known Issues by Version:**

- `2022.8.2`: [Serialization bugs](https://github.com/goauthentik/authentik/issues/3482)
- `2024.x`: [Certificate handling issues](https://github.com/goauthentik/authentik/issues/8684)
- `2025.2.0`: [Serializer KeyError](https://github.com/goauthentik/authentik/issues/13294)

</details>

<details>
<summary><strong>Memory Issues</strong></summary>

```bash
# Error: Out of memory during export
# Solution: Increase container memory limits
# Docker Compose
services:
  worker:
    mem_limit: 2g
```

</details>

### Debug Mode

Enable verbose logging for troubleshooting:

```bash
# Set log level
export AUTHENTIK_LOG_LEVEL=debug
ak export_blueprint
```

### Alternative Export Methods

If `ak export_blueprint` fails, try these alternatives:

1. **Flow-specific exports**: Use the web UI to export individual flows
2. **API exports**: Use the REST API for specific object types
3. **Partial exports**: Export specific models using Django management commands

## Examples

### Basic Workflow

```bash
# 1. Create backup
docker-compose exec worker ak export_blueprint > backup.yaml

# 2. Version control
git add backup.yaml
git commit -m "feat: authentik configuration backup $(date)"

# 3. Deploy to new instance
docker-compose -f staging.yml exec worker ak import_blueprint < backup.yaml
```

### Automated Backup Script

```bash
#!/bin/bash
# authentik-backup.sh

BACKUP_DIR="/backups/authentik"
TIMESTAMP=$(date +%Y%m%d-%H%M%S)
FILENAME="authentik-backup-${TIMESTAMP}.yaml"

# Create backup
docker-compose exec -T worker ak export_blueprint > "${BACKUP_DIR}/${FILENAME}"

# Compress
gzip "${BACKUP_DIR}/${FILENAME}"

# Cleanup old backups (keep 30 days)
find "${BACKUP_DIR}" -name "*.yaml.gz" -mtime +30 -delete

echo "Backup created: ${FILENAME}.gz"
```

### Configuration Diff

```bash
# Compare configurations between instances
docker-compose exec worker ak export_blueprint > current.yaml
kubectl exec deployment/authentik-worker -- ak export_blueprint > staging.yaml

# Show differences
diff -u current.yaml staging.yaml
```

## Best Practices

### 🔄 Regular Backups

```bash
# Cron job for daily backups
0 2 * * * /scripts/authentik-backup.sh
```

### 📝 Documentation

Document your exports:

```yaml
# Add metadata comments
# Exported: 2025-06-08T10:30:00Z
# Instance: production-authentik
# Version: 2025.6.1
# Purpose: Pre-upgrade backup

version: 1
metadata:
  name: "production-backup-20250608"
entries:
  # ... configuration objects
```

### 🔧 Environment-Specific Configs

```bash
# Separate configs per environment
ak export_blueprint > configs/development.yaml
ak export_blueprint > configs/staging.yaml
ak export_blueprint > configs/production.yaml
```

### ✅ Validation

Always validate exported blueprints:

```bash
# Test import on development instance
ak import_blueprint --dry-run < backup.yaml
```

## Contributing

Found an issue or want to improve this documentation?

- 📚 [Authentik Documentation](https://docs.goauthentik.io/)
- 🐛 [Report Issues](https://github.com/goauthentik/authentik/issues)
- 💬 [Discord Community](https://discord.gg/jg33eMhnj6)
- 🤝 [Contribution Guide](https://docs.goauthentik.io/docs/developer-docs/)

---

<div align="center">

**Made with ❤️ by the [Authentik Security](https://github.com/goauthentik) team**

[⭐ Star us on GitHub](https://github.com/goauthentik/authentik) | [📖 Read the Docs](https://docs.goauthentik.io/) | [💬 Join Discord](https://discord.gg/jg33eMhnj6)

</div>

<!-- Links -->

[authentik-shield]: https://img.shields.io/badge/authentik-000000?style=for-the-badge&logo=authentik&logoColor=white
[authentik-url]: https://goauthentik.io/
[docs-shield]: https://img.shields.io/badge/docs-available-blue?style=for-the-badge
[docs-url]: https://docs.goauthentik.io/
[discord-shield]: https://img.shields.io/discord/809154715984199690?style=for-the-badge&logo=discord&logoColor=white
[discord-url]: https://discord.gg/jg33eMhnj6
[github-release-shield]: https://img.shields.io/github/v/release/goauthentik/authentik?style=for-the-badge
[github-release-url]: https://github.com/goauthentik/authentik/releases
