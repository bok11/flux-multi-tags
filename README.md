# flux-multi-tags

Docker image tags for the [`flux-multi-base`](https://github.com/bok11/flux-multi-base)
GitOps demo, kept in a **separate repo** (one tag per file).

`flux-multi-base` pulls this repo in as a remote kustomize base and injects the tag into the
nginx Deployment via a `replacements:` block.

## Files

- `nginx` — the image tag for the nginx deployment (e.g. `1.27-alpine`).
- `kustomization.yaml` — generates the `image-tags` ConfigMap (`data.nginx`) from the tag files.

## Experiment

To test whether Flux re-deploys when **only** this repo changes (the manifest repo stays
untouched), edit `nginx` to a new tag, commit, and push:

```bash
echo -n 1.29-alpine > nginx
git commit -am "bump nginx to 1.29-alpine" && git push
```

Then watch the cluster — see the root `README.md` of the experiment for the full procedure.

> Note: edit the `nginx` file with no trailing newline (`echo -n`), otherwise the tag becomes
> `1.29-alpine\n` and produces an invalid image reference.
