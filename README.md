# pangolin



# Setting up a Github Self-Hosted Runner
1. In the Github repo, navigate to `Settings - Actions - Runners`
2. Click on `New self-hosted runner`
3. Make note of the token
4. On the target node, edit the SOPS secret object and add the token to the `.env` object as `GITHUB_RUNNER_TOKEN`
```
sops --config .sops.yaml secrets.yaml
```
5. Add the Github Runner container to the docker-compose.yml file
```

```