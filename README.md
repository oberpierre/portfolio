# Portfolio

A modern, responsive portfolio website built with [Hugo](https://gohugo.io/) and the [Blowfish](https://blowfish.page/) theme.

## Getting Started

### Prerequisites

- Install [Node.js](https://nodejs.org/en/download) ≥ 22.0.0 (recommended via [NVM](https://github.com/nvm-sh/nvm#installing-and-updating))
- Install [Hugo](https://gohugo.io/installation/)
- Optionally: Install [Docker](https://www.docker.com/)
  - Enables containerized development that closely resembles the production environment, including 404 redirects and other webserver (nginx) features

### Local Development

1. **Install dependencies**

   ```bash
   npm install
   ```

2. **Start the development server**

   ```bash
   npm run dev
   ```

   The site will be available at `http://localhost:1313` with live reload enabled.

### Containerized Development

For a containerized development environment that mirrors production:

1. **Install dependencies**

   ```bash
   npm install
   ```

2. **Start Hugo in watch mode** (in one terminal):

   ```bash
   npm run dev
   ```

3. **Start the containerized server** (in another terminal):
   ```bash
   npm run dev:docker
   ```

This setup provides a production-like environment for the development workflow:

- Hugo (`npm run dev`) continuously builds the site and updates the `public/` directory
- Docker Compose (`npm run dev:docker`) serves the site through nginx at `http://localhost:$PORT` (configurable via `.build/.env`)
- The container automatically syncs changes from the `public/` directory, providing a production-like environment with proper error pages, redirects, and live changes from development.

### Code Formatting

This project uses [Prettier](https://prettier.io/) for code formatting to maintain consistent code style across all files.

A pre-commit hook is configured using [Husky](https://typicode.github.io/husky/) and [lint-staged](https://github.com/lint-staged/lint-staged) that automatically formats staged files before each commit. This ensures all committed code follows the project's formatting standards.

The hook will automatically run `prettier --write` on all staged files when you commit changes.

To manually run formatting during development you can use:

- `npm run format` to format all files
- `npm run format:check`: to check formatting of all files

## Building

### Local Build

```bash
npm run build
```

The generated static site will be available in the `public/` directory.

### Container Build

```bash
npm run build:docker
```

This builds a production-ready Docker image using nginx.

## Content Management

This portfolio uses Hugo's content management system with the Blowfish theme. Content is written in Markdown and stored in the `content/` directory.

- **Hugo Content Management**: [gohugo.io/content-management/](https://gohugo.io/content-management/)
- **Blowfish Theme Documentation**: [blowfish.page/docs/](https://blowfish.page/docs/)

When running `npm run dev`, you can preview changes live as you edit content files.

## Deployment

### Static Hosting

The built site in the `public/` directory can be deployed to any static hosting service:

- GitHub Pages
- Netlify
- Vercel
- AWS S3
- Your own server
- etc.

### Container Deployment

For containerized deployments:

1. **Build the container**:

   ```bash
   npm run build:docker
   ```

   You may alternatively use `docker buildx` or `docker build` or some other container build.

2. **Tag and push to registry**:

   ```bash
   docker image tag portfolio:latest {registry}/{image}:{version}
   docker image push {registry}/{image}:{version}
   ```

   You may publish the container under multiple tags by repeating above commands, see [Build, tag and publish an image](https://docs.docker.com/get-started/docker-concepts/building-images/build-tag-and-publish-an-image/)

3. **Deploy the container**:
   ```bash
   docker run -p 8080:80 {registry}/{image}:{version}
   ```

The container serves the site using nginx on port 80.

## CI/CD Pipeline

This project includes automated GitHub Actions workflows for building and releasing:

### Hugo Build Workflow

- **Trigger**: Push to `main` branch, pull requests, manual dispatch
- **Actions**:
  - Installs Hugo CLI and Dart Sass
  - Installs Node.js dependencies
  - Builds the site using `npm run build`
  - Uploads the generated site as an artifact
- **File**: `.github/workflows/hugo.yml`

### Release Workflow

- **Trigger**: Git tags matching `v*.*.*` (semantic versioning)
- **Actions**:
  - Builds the site using the Hugo workflow
  - Creates multi-platform Docker images (linux/amd64, linux/arm64)
  - Pushes images to GitHub Container Registry
  - Creates a GitHub release with the site archive
- **File**: `.github/workflows/release.yml`

### Creating a Release

To create a new release:

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0
```

This will automatically trigger the release workflow, building and publishing both the static site and Docker container.

## Useful Links

- **Hugo Documentation**: [gohugo.io/documentation/](https://gohugo.io/documentation/)
- **Blowfish Theme**: [blowfish.page/](https://blowfish.page/)
- **Prettier**: [prettier.io/](https://prettier.io/)
