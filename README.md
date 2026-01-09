# voting-app-DevOps

DevOps/bootstrap repository for the Example Voting App source code.

## What this repo contains

- Application source code only:
  - `vote/` (Python/Flask)
  - `result/` (Node.js)
  - `worker/` (.NET)
  - `healthchecks/` (helper scripts)
- Infrastructure automation (Ansible): `ansible/`

## Ansible bootstrap

The Ansible playbook provisions a local Ubuntu VM with the tooling needed to run the project.

### Structure

- `ansible/playbook.yml` – main playbook
- `ansible/inventory/hosts.ini` – inventory (defaults to `localhost`)
- `ansible/inventory/groupvars/all.yml` – variables (e.g. `minikube_user`, `aws_user`)
- `ansible/roles/` – roles:
  - `docker_and_git` – installs Docker + Git and configures Docker daemon
  - `k8s_tools` – installs `kubectl` + `minikube`, configures the minikube driver, and validates the local cluster
  - `aws_tools` – installs AWS CLI v2 and creates `~/.aws/` for the target user
  - `common` – baseline OS setup (packages, user, timezone, directories)

### Run

From the `ansible/` directory:

```bash
ansible-playbook playbook.yml
```

## AWS CLI configuration (manual)

The `aws_tools` role **installs** the AWS CLI, but it deliberately does **not** store or write AWS credentials.

After provisioning, configure AWS credentials yourself on the VM:

```bash
aws configure
# or
aws configure --profile <profile-name>
```

Verify authentication:

```bash
aws sts get-caller-identity
# or
aws sts get-caller-identity --profile <profile-name>
```