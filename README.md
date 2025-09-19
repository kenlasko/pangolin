# Introduction
This is the repo for my [Pangolin](https://github.com/fosrl/pangolin) self-hosted secure web services tunnel running in Docker. Any publically facing web services are proxied through Pangolin to either my [home or cloud-based Kubernetes cluster](https://github.com/kenlasko/k8s). 

It is hosted on a small VPS on Oracle cloud, alongside my cloud Kubernetes cluster. 

# Prerequisites
- Docker
- SOPS and age (for secrets management)

# Docker Secrets
I want to ensure that all secrets are properly encrypted at rest so that I can store the repo on Github. This is accomplished via a few scripts:
- [load-sops-secrets.sh](load-sops-secrets.sh)
- [setup-sops-secret-builder.sh](setup-sops-secret-builder.sh)

Secrets are encrypted via SOPS/age into [secrets.yaml](secrets.yaml)

[load-sops-secrets.sh](load-sops-secrets.sh) will parse [secrets.yaml](secrets.yaml) and will create files at the specified location or individual secrets under `/run/secrets`. An example `secrets.yaml` is below:
```
STANDALONE_SECRET: mysecretvalue
/docker/.env: |
  SECRET1: mysecretvalue1
  SECRET2: mysecretvalue2
```
`STANDALONE_SECRET` will be placed in a file at `/run/secrets/STANDALONE_SECRET`
`/docker/.env` will create a secret in a file located at `/docker/.env`

[setup-sops-secret-builder.sh](setup-sops-secret-builder.sh) creates a series of `systemd` services that will watch for changes in `secrets.yaml` and will trigger the [load-sops-secrets.sh](load-sops-secrets.sh)


# Updates
Docker container updates are managed via [Renovate](https://github.com/renovatebot/renovate). When an update is found, Renovate updates the version number in `docker-compose.yml`.  A [Github Self-Hosted Action](https://github.com/kenlasko/docker-rpi2/actions/runners?tab=self-hosted) runs locally in a Docker container to pull the latest repo changes and restart the affected containers.


# References
- [Installing/Configuring SOPS and age](https://github.com/kenlasko/docker-rpi1/blob/main/docs/SOPS-CONFIG.md)
- [Setting up a Github Self-Hosted Runner](https://github.com/kenlasko/docker-rpi1/blob/main/docs/GITHUB-RUNNER.md)

# Related Repositories
Links to my other repositories mentioned or used in this repo:
- [K8s Cluster Configuration](https://github.com/kenlasko/k8s): Manages Kubernetes cluster manifests and workloads.
- [NixOS](https://github.com/kenlasko/nixos-wsl): A declarative OS modified to support my Kubernetes cluster
- [Omni](https://github.com/kenlasko/omni): Creates and manages the Kubernetes clusters.