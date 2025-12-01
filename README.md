# Docker Build Action with Cache and Slack Notifications

A custom GitHub Action for building Docker images with caching and Slack notifications.

## Features

- Docker image building with BuildKit
- Layer caching using GitHub Actions cache
- Push to any Docker registry
- Slack notifications (success/failure/always)
- Custom pre/post build scripts
- Multi-tag support

## Usage

```yaml
- uses: jepcloud/docker-build-action@v1
  with:
    image-name: myapp
    tags: latest,${{ github.sha }}
    registry: ghcr.io
    registry-username: ${{ github.actor }}
    registry-password: ${{ secrets.GITHUB_TOKEN }}
    push: "true"
    slack-webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}
```

## Inputs

| Input               | Description                             | Required | Default        |
| ------------------- | --------------------------------------- | -------- | -------------- |
| `dockerfile`        | Path to Dockerfile                      | No       | `./Dockerfile` |
| `context`           | Build context path                      | No       | `.`            |
| `image-name`        | Docker image name                       | Yes      | -              |
| `tags`              | Comma-separated tags                    | No       | `latest`       |
| `build-args`        | Build arguments (KEY=VALUE)             | No       | -              |
| `registry`          | Docker registry                         | No       | `docker.io`    |
| `registry-username` | Registry username                       | No       | -              |
| `registry-password` | Registry password                       | No       | -              |
| `push`              | Push to registry                        | No       | `false`        |
| `pre-build-script`  | Script before build                     | No       | -              |
| `post-build-script` | Script after build                      | No       | -              |

## Outputs

| Output      | Description          |
| ----------- | -------------------- |
| `image-tag` | Full image tag built |
| `digest`    | Image digest         |

## Examples

### Basic Build

```yaml
- uses: jepcloud/docker-build-action@v1
  with:
    image-name: myapp
```

### Build and Push to GHCR

```yaml
- uses: jepcloud/docker-build-action@v1
  with:
    image-name: myapp
    tags: latest,${{ github.sha }}
    registry: ghcr.io
    registry-username: ${{ github.actor }}
    registry-password: ${{ secrets.GITHUB_TOKEN }}
    push: "true"
```

### With Custom Scripts

```yaml
- uses: jepcloud/docker-build-action@v1
  with:
    image-name: myapp
    pre-build-script: |
      npm run test
      npm run lint
    post-build-script: |
      echo \"Build completed!\"
      curl -X POST https://api.example.com/notify
```

### With Build Arguments

```yaml
- uses: jepcloud/docker-build-action@v1
  with:
    image-name: myapp
    build-args: |
      NODE_ENV=production,
      VERSION=${{ github.sha }},
      BUILD_DATE=$(date -u +'%Y-%m-%dT%H:%M:%SZ')
```
