# Docker Push GitHub Action

This GitHub Action, named `Docker Push`, is designed to build and push a Docker image. It provides a variety of inputs to customize the build and push process, and outputs the version of the image that was built.

## Inputs

- `github_token`: GitHub Token. Only required for GCR deployments.
- `dockerhub_user`: DockerHub Username. Required for both Dockerhub and GCR deployments.
- `dockerhub_password`: DockerHub Password. Required for both Dockerhub and GCR deployments.
- `image`: Image Name. Required.
- `context`: Build context directory. Defaults to the repository root.
- `build_args`: Build Args. Not required.
- `dockerfile`: Dockerfile. Not required, default is 'Dockerfile'
- `tag_latest`: Tag as latest. Not required, by default will tag only on the default branch.
- `registry`: Registry (docker, github). Not required, default is 'github'
- `version`: Version to tag for the image. Not required, by default will tag with 'edge'
- `allow_vulnerabilities`: Push the image even if vulnerabilities are found. Not required, by default will fail if vulnerabilities are found.
- `slack_webhook_url`: Slack Webhook URL to send notifications. Not required.

## Outputs

- `version`: The version of the image that was built.

## Steps

1. Generate Docker Tags: This step generates Docker tags based on the inputs.
2. Configuration: This step sets the output image version.
3. Login to Container Registry: This step logs into the specified container registry.
4. Build and push to the specified registry: This step builds and pushes the Docker image to the specified registry.
5. Check CVEs: This step checks for Common Vulnerabilities and Exposures (CVEs) in the Docker image.
6. Send notification to slack: This step sends a notification to Slack if the previous step fails and a Slack webhook URL is provided.

## Usage

To use this action, include it in your workflow file with the necessary inputs. Here's an example:

```yaml
- name: Docker Push
  uses: enosix/ghac-docker-github@stable
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    dockerhub_user: ${{ secrets.DOCKERHUB_USER }}
    dockerhub_password: ${{ secrets.DOCKERHUB_PASSWORD }}
    image: 'your-image-name'
    context: '.'
    build_args: 'ARG1=value1,ARG2=value2'
    dockerfile: 'Dockerfile'
    tag_latest: 'true'
    registry: 'docker'
    version: '1.0.0'
    allow_vulnerabilities: 'false'
    slack_webhook_url: ${{ secrets.SLACK_WEBHOOK_URL }}
```

Replace `your-image-name`, `ARG1=value1`, `ARG2=value2`, and `1.0.0` with your own values. Make sure to set appropriate secrets in your repository settings.
