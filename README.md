# Bytewax Platform Website Content

This repository stores the content shown on the Bytewax Platform website.

- Staging: https://staging.platform.bytewax.io
- Production: https://platform.bytewax.io

The website looks for releases of this repository and shows the content of the selected release by the user. By default, the latest release will be shown.

## Reference Content

The content of these files is intended to be generated automatically. This section describes the steps for each.

### Helm Chart

Steps:

1. Open a terminal in the `deployment/chart` directory of the `waxctl` repository.

2. Run this command to output a table to the std output:

```bash
docker run --rm --volume "$(pwd):/helm-docs" -u $(id -u) jnorwood/helm-docs:latest --dry-run
```

3. Replace the md table in `reference\helm-chart.md` 

