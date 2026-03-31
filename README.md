# Ansible Development in Red Hat OpenShift Dev Spaces

This repository provides an Ansible-focused development workspace for Red Hat OpenShift Dev Spaces (3.25+) on OpenShift 4.20+. It uses the upstream [ansible-devspaces](https://github.com/ansible/ansible-dev-tools) container image with nested container support via rootless podman and user namespaces.

## What's Included

### Container Image

The workspace uses `ghcr.io/ansible/ansible-devspaces:latest`, which includes:

- **Ansible tooling**: ansible-core, ansible-lint, ansible-navigator, molecule, ansible-creator, tox-ansible
- **Container tools**: rootless podman, buildah, skopeo (nested container-in-container via user namespaces)
- **CLI tools**: `oc` (OpenShift CLI), `kubectl`, `git`, `zsh` with oh-my-zsh
- **Python 3.12** with pip
- **Pre-installed Ansible collections**: `ansible.netcommon`, `ansible.posix`, `ansible.scm`, `ansible.utils`
- **Dynamic UID mapping** at runtime for portable rootless podman across any OpenShift UID assignment

### VS Code Workspace Configuration

The `devspaces.code-workspace` file pre-configures the editor with:

- Ansible extension with FQCN enforcement and validation
- ansible-lint with production profile (offline mode)
- File associations for all Ansible directory patterns (`.yml` and `.yaml`)
- Podman as the container engine
- Black formatter for Python
- EditorConfig support for consistent formatting

### Linting and Quality

- `.ansible-lint` — production profile with proper exclusions
- `.yamllint` — YAML formatting rules aligned with Ansible conventions
- `.ansible-navigator.yml` — navigator defaults (stdout mode, no execution environment)
- `.editorconfig` — cross-editor formatting (2-space YAML indent, LF line endings)

## Getting Started

### Prerequisites

- Red Hat OpenShift 4.20+ cluster with Dev Spaces 3.25+ installed
- User namespace support enabled (GA in OCP 4.20)

### Launching the Workspace

1. Log in to your OpenShift console
2. Access Dev Spaces via the application launcher (grid icon, top right) and select "Red Hat OpenShift Dev Spaces"
3. If this is your first time, authorize Dev Spaces with OpenShift OAuth
4. Create a workspace from this repository URL — see [Starting a workspace from a Git repository URL](https://docs.redhat.com/en/documentation/red_hat_openshift_dev_spaces/3.23/html/user_guide/getting-started-with-devspaces#starting-a-workspace-from-a-git-repository-url)

## Git Authentication

To authenticate with GitHub, GitLab, or Bitbucket across all your workspaces:

1. Log in to the Dev Spaces dashboard
2. Click the dropdown next to your username (top right) and select "User Preferences"
3. Go to "Personal Access Tokens" and add a Personal Access Token (PAT)

See the official Red Hat documentation for [Authenticating to a Git server from a workspace](https://docs.redhat.com/en/documentation/red_hat_openshift_dev_spaces/3.23/html/user_guide/getting-started-with-devspaces#authenticating-to-a-git-server-from-a-workspace).

## Adding Repositories

Clone additional repositories into your workspace via the terminal or VS Code source control. You can also fork this repository and modify the `devfile.yaml` to add repositories automatically.

## Customizing the Dashboard

To modify or add sample tiles in the Dev Spaces dashboard, see [Configuring getting started samples](https://docs.redhat.com/en/documentation/red_hat_openshift_dev_spaces/3.23/html/administration_guide/configuring-devspaces#configuring-getting-started-samples).

## Attribution

This workspace setup builds on:

- [ansible/ansible-dev-tools](https://github.com/ansible/ansible-dev-tools) — upstream DevSpaces image and devfile
- [redhat-developer-demos/ansible-devspaces-demo](https://github.com/redhat-developer-demos/ansible-devspaces-demo) — reference demo workspace
- [jeffcpullen/devspaces-example](https://github.com/jeffcpullen/devspaces-example) — DevSpaces example workspace with nested podman
- [Enable nested containers in OpenShift Dev Spaces with user namespaces](https://developers.redhat.com/articles/2024/12/02/enable-nested-containers-openshift-dev-spaces-user-namespaces) — nested container setup guide
