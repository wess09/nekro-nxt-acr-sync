# NekroNXT Image Sync

Automated container-image mirroring from GitHub Container Registry (GHCR) to Alibaba Cloud Container Registry (ACR), powered by GitHub Actions and Skopeo.

This repository contains the synchronization workflow only. It does not build or modify the upstream application image.

## How it works

- **Scheduled synchronization:** Every six hours, at minute 23 (UTC).
- **Manual synchronization:** Run the workflow from the **Actions** tab.
- **Image copying:** The configured image tag is copied from the source registry to the destination registry.
- **Multi-platform support:** The current sync action uses `skopeo copy -a`, which copies all architectures in the source image index.

Scheduled runs are best-effort and may start later than their configured time.

## Configuration

The workflow is defined in [`.github/workflows/sync.yml`](.github/workflows/sync.yml). Update its `source`, `destination`, and `copy` settings for your own registries and image names.

Configure credentials in **Settings → Secrets and variables → Actions**:

| Secret | Purpose |
| --- | --- |
| `DESTINATION_CREDENTIAL` | Destination registry credentials in `<username>:<password>` format. |
| `SOURCE_CREDENTIAL` | Optional source registry credentials. For private GHCR images, use a token with the appropriate package-read permission. |

Never commit passwords, access tokens, or other credentials to the repository.

## Run manually

1. Open the repository's **Actions** tab.
2. Select **Sync NekroNXT image to Aliyun ACR**.
3. Choose **Run workflow**.
4. Open the resulting run to inspect synchronization logs.

## Troubleshooting

- **Long-running synchronization:** Multi-architecture images can require copying many layers. Check which image and layer is still transferring.
- **`unexpected EOF` while reading a blob:** The source transfer ended unexpectedly. Retrying may help; repeated failures at the same offset warrant investigating the registry connection.
- **Authentication errors:** Check the applicable Actions secrets and registry permissions.
- **Overlapping runs:** Scheduled or manual runs may overlap unless concurrency controls are added to the workflow.

## Privacy and security

This README deliberately omits deployment-specific registry hosts, instance identifiers, namespaces, account names, and credential values.

**Important:** Removing those details from the README does not remove them from workflow configuration, Git history, or Actions logs. Review those locations separately before publishing or sharing a deployment.
