# Bytewax Platform Website Content

This repository stores the content shown on the Bytewax Platform website.

- Staging: https://staging.platform.bytewax.io
- Production: https://platform.bytewax.io

The website looks for releases of this repository and shows the content of the selected release by the user. By default, the latest release will be shown.

## Reference Content

The content of these files is intended to be generated automatically. This section describes the steps for each.

### Dataflow Custom Resource Definition

Steps:

1. Define these variables:

```bash
export waxctl_repo_path=<ABSOLUTE PATH OF WAXCTL REPOSITORY DIRECTORY>
export content_path=<ABSOLUTE PATH OF PLATFORM CONTENT REPOSITORY DIRECTORY>
```

For example:

```bash
export waxctl_repo_path=/home/jdoe/repos/waxctl
export content_path=/home/jdoe/repos/platform-docs-content
```

2. Run this command to update the file `reference/dataflow-crd.md`

```bash
docker run -u $(id -u):$(id -g) --rm \
  -v $waxctl_repo_path:/workdir \
  -v $content_path/reference:/reference \
  ghcr.io/fybrik/crdoc:latest \
  --resources /workdir/operator/config/crd/bases \
  --output /reference/dataflow-crd.md \
  --template /reference/markdown.tmpl
```

### Helm Chart

Steps:

1. Open a terminal in the `deployment/chart` directory of the `waxctl` repository.

2. Run this command to output a table to the std output:

```bash
docker run --rm --volume "$(pwd):/helm-docs" -u $(id -u) jnorwood/helm-docs:latest --dry-run
```

3. Replace the md table in `reference\helm-chart.md` 

### WaxAPI

To update the `resources/openapi.json` file, follow the instructions of the `OpenAPI` section in the WaxAPI [README.md](https://github.com/bytewax/waxctl/blob/main/cmd/web/README.md#OpenAPI)