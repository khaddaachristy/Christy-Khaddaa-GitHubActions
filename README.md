# Christy-Khaddaa-GitHubActions

EECE 430 — GitHub Actions CI/CD for the Inventory Agent application.

The application uses Django and the Gemini API to handle inventory requests. This repository contains the application, its Dockerfile, and the workflow used to build, publish, and deploy it.

## CI/CD workflow

The workflow is defined in [.github/workflows/ci.yml](.github/workflows/ci.yml). It runs on pushes to `main` and can also be started manually from the Actions tab.

The `test` job runs on GitHub's Ubuntu runner. It installs the Python dependencies, runs `python manage.py check`, builds the Docker image, and pushes it to GitHub Container Registry:

```text
ghcr.io/khaddaachristy/christy-khaddaa-githubactions:latest
```

The Django check uses a placeholder API key and does not send Gemini requests.

After that job succeeds, the `deploy` job runs on a self-hosted Linux runner with the `portfolio-deploy` label. It pulls the published image, replaces the `portfolio-app` container, and starts the application on port 8000. Registry authentication uses the workflow's `GITHUB_TOKEN`.

## Run locally with Docker

Docker must be installed and running.

```bash
git clone https://github.com/khaddaachristy/Christy-Khaddaa-GitHubActions.git
cd Christy-Khaddaa-GitHubActions
```

Create a `.env` file beside `manage.py`:

```text
GEMINI_API_KEY=your_key_here
```

Replace the placeholder with a valid Gemini API key. Keep this file out of Git.

Build and run:

```bash
docker build -t inventory-docker .
docker run --rm -p 8000:8000 --env-file .env inventory-docker
```

Open http://localhost:8000. In Codespaces, use the Ports tab to open port 8000 in the browser. Database migrations run automatically when the container starts.

Stop this manually started container before using the deployment workflow on the same machine, so port 8000 is available.

## Deployment setup

The deployment machine needs Docker and a GitHub Actions runner registered with this repository. The runner must have the labels `self-hosted`, `Linux`, and `portfolio-deploy`.

Store the Gemini API key on that machine in `~/ai-agent.env`:

```text
GEMINI_API_KEY=your_key_here
```

Restrict access to the file:

```bash
chmod 600 ~/ai-agent.env
```

For the Codespaces demonstration, the runner is installed in `~/actions-runner`. Start it in a terminal and leave it running:

```bash
cd ~/actions-runner
./run.sh
```

In GitHub, open **Actions → CI → Run workflow**, select `main`, and start the workflow. Both jobs should finish successfully.

The deployed container maps `127.0.0.1:8000` to the application's port 8000. Open it through the Codespaces Ports tab.

## Verify the deployment

In a separate terminal:

```bash
docker ps --filter name=portfolio-app
docker logs --tail 50 portfolio-app
curl -I http://127.0.0.1:8000/
```

Open the inventory page in the browser. To test the Gemini integration, submit an inventory question using a valid API key.

The workflow build, image publication, and deployment completed successfully during setup, and the deployed page loaded in the browser. A successful inventory response from Gemini has not yet been verified.

## Lab demonstration

1. Resume the Codespace used for deployment.
2. Start the runner with `cd ~/actions-runner` followed by `./run.sh`.
3. Run the workflow from the repository's Actions tab.
4. Show the successful build and deployment jobs.
5. Open port 8000 and show the deployed application.

The runner is available only while its process and Codespace are running. The application uses Django's development server for this demonstration. Database changes inside the container are lost when the container is replaced because no persistent database volume is configured.
