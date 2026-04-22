# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a DevWorkspaces configuration repo for launching an Ansible-focused development environment in Red Hat OpenShift Dev Spaces (or Eclipse Che). It provisions a VS Code workspace with Ansible extensions, rootless podman support, and OpenShift CLI tools. There is no application code to build or test — the repo defines the workspace itself.

## Key Files

- **devfile.yaml** — Devfile v2.2.0 defining the workspace. Two components:
  - `dev-tools`: Main container using a custom image (`ghcr.io/ansible/ansible-devspaces:v26.4.5`) with rootless podman, `/dev/fuse` and `/dev/net/tun` device mappings, unmasked `/proc`, and `hostUsers: false` for user namespace isolation.
  - `prep-workspace`: Init container that copies `oc` and `kubectl` binaries into `/projects/bin` before workspace start.
- **devspaces.code-workspace** — VS Code multi-root workspace config with recommended extensions and Ansible-specific settings (linting with `--profile production --offline`, fully qualified collection names, podman as container engine).

## Architecture Notes

- The workspace uses **nested containers via rootless podman** instead of the typical kubedock approach. This requires an OpenShift cluster with user namespace support (tech preview as of OCP 4.19).
- Pod-level annotations (`io.kubernetes.cri-o.Devices`) and container-level security context (`procMount: Unmasked`) are set via devfile attributes (`pod-overrides` / `container-overrides`).
- Storage type is `per-workspace` (not per-user or ephemeral).

## Workspace Customization

To add repositories to the workspace, modify the `devfile.yaml` — there are commented-out examples for cloning additional repos. The workspace path convention is `/projects/<repo-name>`.

## Ansible Tooling in the Workspace

The workspace image includes: `ansible`, `ansible-navigator`, `ansible-lint`, and Python 3. Linting is configured to run `ansible-lint --profile production --offline`. All `*.yml` files are associated with the `ansible` language mode by default.
