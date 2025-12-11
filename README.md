# Recipe Vault
```markdown
# docker-php-sample

A minimal repository that demonstrates a PHP app running in Docker and wired to CI/CD using GitHub Actions.

## Files
- `index.php` - tiny PHP app (prints a message)
- `Dockerfile` - builds image from `php:8.2-apache`
- `docker-compose.yml` - run locally via `docker compose up`
- `.github/workflows/ci-cd.yml` - GitHub Actions workflow (lint, build, push to GHCR, smoke test)

## Local testing
1. Build and run with Docker:
   - docker build -t docker-php-sample:local .
   - docker run --rm -p 8080:80 docker-php-sample:local
   - Open http://localhost:8080

2. With docker compose:
   - docker compose up --build
   - Open http://localhost:8080

## CI/CD (GitHub Actions)
The workflow:
- Runs on push to `main`, PRs, and tag pushes.
- Step 1 (test): uses `shivammathur/setup-php` to run `php -v` and `php -l index.php` (syntax check).
- Step 2 (build-and-push): builds the image with `docker/build-push-action` and pushes to GitHub Container Registry (GHCR) when on `main` or on a tag.

### Required repository secrets / permissions to push to GHCR
- The workflow uses the built-in `GITHUB_TOKEN` to push to GHCR; the workflow file includes `permissions: packages: write` so Actions can publish packages. If your organization blocks writes with the default token, you may need to create a Personal Access Token (PAT) with `write:packages` and store it as `GHCR_PAT` in repository secrets and update the workflow to use it.
- If you prefer to push to Docker Hub, add:
  - `DOCKERHUB_USERNAME`
  - `DOCKERHUB_TOKEN` (or password)
  and update the workflow to login to Docker Hub instead.

## Using GitHub Copilot
GitHub Copilot is an AI assistant for editing code in your editor (VS Code, JetBrains, GitHub.dev). It is not a CI runner. Ways to use it with this repo:
- Use Copilot to help write more PHP code, tests, or expand the Dockerfile.
- Use Copilot in PRs to suggest improvements to workflows or README text.
- Install and sign into Copilot in your editor to get inline suggestions.

## Next steps / suggestions
- Add real application logic and tests.
- Add branch protection rules and require the CI workflow to pass before merging.
- For GHCR pushes from Actions, ensure `permissions: packages: write` is allowed or create a PAT with `write:packages` and store it as a secret.
- Optionally create an automated release workflow that tags and publishes images.

```