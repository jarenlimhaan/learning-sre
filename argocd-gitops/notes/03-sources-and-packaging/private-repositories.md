# Private repositories

## 1. Connecting to private Git repositories
1. Overview of the mechanism: Creating Kubernetes Secrets with specific labels and data schemas.
2. The two primary authentication methods: HTTPS (username + PAT) and SSH (private key).

## 2. Authentication via HTTPS
1. Generating Fine-Grained Personal Access Tokens in GitHub.
2. Configuring repository credentials via the Argo CD UI and the CLI.

## 3. Authentication via SSH
1. Generating SSH key pairs (`ssh-keygen`) and configuring Deploy Keys in the Git provider.
2. Creating the necessary Kubernetes Secrets for SSH authentication.


## How Argo CD stores repository credentials

Argo CD stores repository access in Secrets in its control-plane namespace. A repository Secret uses the label `argocd.argoproj.io/secret-type: repository` and fields such as `url`, `username`, and `password`, or `sshPrivateKey`.

Grant read-only access wherever possible. Avoid committing credentials to Git or putting a token directly on a shell command line, where it can enter shell history. Prefer the Argo CD CLI/UI credential flow, an external secret manager, or an interactive/environment-based process appropriate to your platform. Rotate any credential that is exposed.

![Connecting a private repository](../../images/connect-to-private-repo.png)

In the UI, go to **Settings → Repositories → Connect Repo**, choose HTTPS or SSH, and enter the repository URL and corresponding credential. Verify that the connection succeeds before creating the Application.

For several repositories sharing a URL prefix, use a repository credential template (`repo-creds`) instead of duplicating credentials. Keep the URL prefix precise so credentials are not applied more broadly than intended.
