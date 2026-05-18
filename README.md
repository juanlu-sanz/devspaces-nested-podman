# Dev Spaces: Nested Podman for Molecule Testing

A ready-to-use Dev Spaces workspace with nested container support, enabling Molecule to use the **podman driver** for Ansible role testing.

## Quick Start

1. Ensure your cluster has the [nested-podman-scc](https://github.com/juanlu-sanz/workshop-devspaces-ansible-molecule/blob/main/01-devspaces/setup/nested-podman-scc.yaml) applied and the CheCluster patched to use it.
2. Open the Dev Spaces dashboard and click **Create Workspace**.
3. Paste this repository URL and launch.

The workspace starts with podman, molecule, ansible-core, and ansible-lint pre-installed.

## What's Included

- **Image:** `ghcr.io/juanlu-sanz/nested-podman-devspaces:latest` (public, based on UDI RHEL9)
- **Podman nesting:** `container-overrides` with `securityContext.privileged: true`
- **Tools:** podman, buildah, fuse-overlayfs, slirp4netns, molecule, ansible-core, ansible-lint

## Usage

From the workspace terminal:

```bash
unset CONTAINER_HOST
podman run --rm registry.access.redhat.com/ubi9/ubi-minimal echo "It works!"
```

## Cluster Prerequisites

A cluster admin must apply the privileged SCC before this workspace can function:

```bash
oc apply -f https://raw.githubusercontent.com/juanlu-sanz/workshop-devspaces-ansible-molecule/main/01-devspaces/setup/nested-podman-scc.yaml

oc patch checluster devspaces -n openshift-devspaces --type merge -p '
spec:
  devEnvironments:
    containerBuildConfiguration:
      openShiftSecurityContextConstraint: nested-podman-scc
'
```

## References

- [Workshop: Ansible Development with Molecule on Dev Spaces](https://github.com/juanlu-sanz/workshop-devspaces-ansible-molecule)
- [Nested Podman in Dev Spaces (upstream)](https://github.com/cgruver/devspaces-nested-podman-privileged)
