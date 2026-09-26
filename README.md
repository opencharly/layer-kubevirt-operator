# layer-kubevirt-operator

The `layer-kubevirt-operator` candy of the [opencharly/charly](https://github.com/opencharly/charly)
candy library, as a standalone repo (the candy de-submodule cutover, kind-prefixed naming).
It installs **KubeVirt + CDI** onto a Kubernetes cluster by downloading the pinned release
manifests and applying them with the `kube:` verb, then waiting for the KubeVirt CR to report
`Deployed` via the `kubevirt:` verb.
