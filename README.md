# Docker Push GitHub Action

This GitHub Action, named `Docker Push`, is designed to build and push a Docker image. It provides a variety of inputs to customize the build and push process, and outputs the version of the image that was built.

## Inputs

- `github_token`: GitHub Token. Not required. Used for Docker Scout CVE scanning on pull requests.
- `username`: Username or `github.actor`. Not required.
- `password`: Password or `github.token`. Not required.
- `dockerhub_user`: DockerHub Username. Required. Used for Docker Scout authentication.
- `dockerhub_password`: DockerHub Password. Required. Used for Docker Scout authentication.
- `image`: Image Name. Required.
- `context`: Build context directory. Not required, defaults to the repository root.
- `build_args`: Build Args. Not required.
- `secrets`: Secrets passed to the build (format: `MY_SECRET=MY_ENV_VAR`). Not required.
- `dockerfile`: Dockerfile path. Not required.
- `tag_latest`: Tag as latest. Not required, defaults to `auto`.
- `tag_sha`: Tag with the commit SHA. Not required.
- `version`: Version to tag the image with. Not required, defaults to `edge` tag.
- `allow_vulnerabilities`: Push the image even if vulnerabilities are found. Not required, defaults to `false`.
- `slack_webhook_url`: Slack Webhook URL to send notifications on failure. Not required.

## Outputs

- `version`: The version of the image that was built.

## Steps

1. Set up Docker Buildx: Configures Docker Buildx for multi-platform builds.
2. Generate Docker Tags: Generates Docker tags based on the inputs.
3. Configuration: Determines the target registry and sets the output image version.
4. Build image (local load for scanning): Builds the Docker image locally for CVE scanning.
5. Docker Scout - CVE scan: Scans the image for critical and high severity vulnerabilities.
6. Login to registry: Logs into the target container registry if `username` and `password` are provided.
7. ACR Login: Logs into Azure Container Registry if the target registry is an ACR endpoint.
8. Push image with attestations: Pushes the Docker image with SBOM and provenance attestations.
9. Send Slack notification: Sends a notification to Slack if a previous step fails and a Slack webhook URL is provided.

## Usage

To use this action, include it in your workflow file with the necessary inputs. Here's an example:

```yaml
- name: Docker Push
  uses: enosix/ghac-docker-github@stable
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    username: ${{ github.actor }}
    password: ${{ secrets.GITHUB_TOKEN }}
    dockerhub_user: ${{ secrets.DOCKERHUB_USER }}
    dockerhub_password: ${{ secrets.DOCKERHUB_PASSWORD }}
    image: 'your-image-name'
    context: '.'
    build_args: 'ARG1=value1,ARG2=value2'
    secrets: 'MY_SECRET=MY_ENV_VAR'
    dockerfile: 'Dockerfile'
    tag_latest: 'auto'
    tag_sha: 'true'
    version: '1.0.0'
    allow_vulnerabilities: 'false'
    slack_webhook_url: ${{ secrets.SLACK_WEBHOOK_URL }}
```

Replace `your-image-name`, `ARG1=value1`, `ARG2=value2`, and `1.0.0` with your own values. Make sure to set appropriate secrets in your repository settings.
