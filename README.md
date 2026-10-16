# NekroNXT image mirror to Aliyun ACR

This repository copies the official public image `ghcr.io/nekroai/nekro-nxt:latest` to the Shanghai Aliyun ACR Personal registry every six hours.

Destination: `crpi-gukwnnx8iuh9qpez.cn-shanghai.personal.cr.aliyuncs.com/hajiming/nekro-nxt:latest`

## Required Actions secrets

- `DESTINATION_CREDENTIAL`: Aliyun ACR personal registry username and fixed password in `username:password` form.
- `SOURCE_CREDENTIAL`: optional GHCR username and a GitHub token with `read:packages` permission if anonymous pulls are unavailable. Leave unset for a public package if anonymous access works.

The workflow can also be run manually from the Actions tab.
