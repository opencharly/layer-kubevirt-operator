# AGENTS.md — layer-kubevirt-operator

Standalone candy repo for the `kubevirt-operator` layer — installs **KubeVirt +
CDI** onto a Kubernetes cluster by downloading the pinned release manifests and
applying them **in-venue** with the venue's own `kubectl`, then waiting for the
KubeVirt CR to report `Deployed`. The candy lives in `charly.yml` at the repo
root. It carries **no `skill:` entity**, so no owning `/charly-<family>:<name>`
skill is projected into the marketplace corpus.

Canonical files:

- `charly.yml` — the `kubevirt-operator:` candy entity (no `skill:` entity
  present).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-kubernetes:kubernetes` — the closest owning skill for the Kubernetes
  cluster/venue this operator install targets.
- `/charly-internals:plugin` — the plugin authoring reference; the KubeVirt
  platform this layer installs is probed by `candy/plugin-kubevirt`'s
  `kubevirt:` verb.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `run:`/`check:`, per-distro arms). Load before editing
  any entity field or plan step.
- `/charly-internals:git-workflow` — before any git/PR action.

There is no dedicated `/charly-*:layer-kubevirt-operator` owning skill — this
repo's candy carries no `skill:` entity. The gap is recorded against
`opencharly/opencharly#291`; when one is authored, add it here.

## Build / validate / test

- The candy's `plan:` steps are the functional evidence: the bounded in-venue
  apiserver/node-ready wait, the manifest `download:`/`run:` applies, and the
  `check:` that the KubeVirt CR reports `Deployed`.
- `charly box validate` at the repo root — the structural check (the candy + the
  `run:`/`check:` steps).
- The live acceptance runs against the `k3s-server` guest venue (the
  `check-kubevirt-operator` R10 bed), whose bundled `/usr/local/bin/kubectl` and
  `curl` the in-venue steps use.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate and ships only `.github/workflows/tag-on-merge.yml`.

## Modify this repo

- Edit the `kubevirt-operator:` candy entity in `charly.yml`.
- Bump `KUBEVIRT_VERSION` / `CDI_VERSION` in the candy's `var:` block to move the
  pinned release manifests.
- Keep the applies **in-venue** against the venue's own kubeconfig
  (`KUBECONFIG_IN_VENUE`, default `/etc/rancher/k3s/k3s.yaml`), exactly like the
  helm-chart precedent — this candy installs no cluster and no client; both come
  from the composing venue.

## Landing

Every change lands through a pull request gated by the org-required
`charly/pr-validator`. The landing mechanics — the `feat/` branch, the PR-only
rule, `CHANGELOG/` history, and the tag-on-merge CalVer — are owned by
`/charly-internals:git-workflow` and the umbrella `AGENTS.md` /
`charly/AGENTS.md`; this signpost points at them and does not restate them.
