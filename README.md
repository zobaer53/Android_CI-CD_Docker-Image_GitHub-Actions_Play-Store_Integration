![zeDMoviesApp](https://github.com/zobaer53/Android_CI-CD_Docker-Image_GitHub-Actions_Play-Store_Integration/blob/master/Add%20a%20heading.png)
# Setting Up GitHub Actions CI/CD for Android Apps with Google Play Store Integration Using GitHub Container Registry (GHCR) - Free Tier Guide


This repository contains an Android Calculator App with automated CI/CD using GitHub Actions and GitHub Container Registry (GHCR).

## Features

- Automated Docker image building and publishing to GitHub Container Registry
- Automated Debug APK building on each commit
- Automated release creation with APK attachment when tags are pushed
- Authentication using GitHub repository secrets

## Setup Instructions

### 1. Set up Repository Secrets

Before using this workflow, you need to set up the following repository secrets:

1. Go to your repository settings
2. Navigate to "Secrets and variables" → "Actions"
3. Add the following secrets:
   - `GHCR_PAT`: A GitHub Personal Access Token with `read:packages`, `write:packages`, and `delete:packages` scopes
   - `REGISTRY_URL` (Optional): The registry URL, defaults to `ghcr.io` if not specified

### 2. Trigger a Build

The workflow is triggered automatically on:
- Push to the `main` branch
- Pull requests to the `main` branch
- Push of a tag starting with `v` (e.g., `v1.0.0`)
- Manual trigger from the Actions tab

### 3. Create a Release

To create a release with the APK attached:

1. Create and push a tag:
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

2. The workflow will automatically:
   - Build the Docker image and push it to GHCR
   - Build the Debug APK
   - Create a GitHub Release with the APK attached

## Docker Images

The Docker images are published to GitHub Container Registry and are available at:
```
ghcr.io/[username]/calculatorapp:latest
ghcr.io/[username]/calculatorapp:[commit-sha]
```

## Using the Docker Image

You can pull and use the Docker image with:

```bash
# Pull the image
docker pull ghcr.io/[username]/calculatorapp:latest

# Build a debug APK
docker run --rm -v $(pwd)/output:/output ghcr.io/[username]/calculatorapp:latest -c "/app/build-apk.sh Debug && cp -r /app/app/build/outputs/apk/debug/* /output/"

# Build a release APK
docker run --rm -v $(pwd)/output:/output ghcr.io/[username]/calculatorapp:latest -c "/app/build-apk.sh Release && cp -r /app/app/build/outputs/apk/release/* /output/"
```

## Workflow Details

The GitHub Actions workflow:
1. Builds a Docker image using the Dockerfile
2. Pushes the image to GitHub Container Registry
3. Uses the Docker image to build a Debug APK
4. Creates a GitHub Release if a tag is pushed
5. Uploads the APK as an artifact

For more details, see [medium article](https://medium.com/@zobaer53/setting-up-github-actions-ci-cd-for-android-apps-using-github-container-registry-ghcr-f0f5fbea512b).

