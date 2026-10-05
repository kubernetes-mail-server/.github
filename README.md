# kubernetes-mail-server/.github

`.github/workflows/service.yml` is the pipeline every service repository calls: build on pull
requests, build, push and deploy on `master`, and wait until the rollout is ready. See the inputs at
the top of the file; a caller looks like this:

```yaml
name: build and deploy
on:
  pull_request:
  push:
    branches: [master]
  workflow_dispatch:
jobs:
  service:
    uses: kubernetes-mail-server/.github/.github/workflows/service.yml@main
    secrets: inherit
    with:
      name: opendkim
      helm-args: --set port=$(kubectl get cm -n mail-server services-info -o=jsonpath="{.data.OPENDKIM_PORT}")
```
