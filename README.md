# Christy-Khaddaa-docker
## Run with Docker

Clone this repository and open its folder:

```bash
git clone https://github.com/khaddaachristy/Christy-Khaddaa-docker.git
cd Christy-Khaddaa-docker
```

Create a `.env` file beside `manage.py` containing your own Gemini API key:

```text
GEMINI_API_KEY=your_key_here
```

Build and run:

```bash
docker build -t inventory-docker .
docker run --rm -p 8000:8000 --env-file .env inventory-docker
```

Open http://localhost:8000 locally. In GitHub Codespaces, open port 8000 from the Ports tab.

The image includes Python, the required packages, and the application. Database migrations run automatically at startup. The `.env` file is excluded from Git and the Docker image.

## Validation and known limitation

The Docker image built successfully, the container started, and the inventory page loaded in GitHub Codespaces.

Database changes inside the container are not retained after the co
cat >> README.md <<'EOF'

## Run with Docker

Clone this repository and open its folder:

```bash
git clone https://github.com/khaddaachristy/Christy-Khaddaa-docker.git
cd Christy-Khaddaa-docker
```

Create a `.env` file beside `manage.py` containing your own Gemini API key:

```text
GEMINI_API_KEY=your_key_here
```

Build and run:

```bash
docker build -t inventory-docker .
docker run --rm -p 8000:8000 --env-file .env inventory-docker
```

Open http://localhost:8000 locally. In GitHub Codespaces, open port 8000 from the Ports tab.

The image includes Python, the required packages, and the application. Database migrations run automatically at startup. The `.env` file is excluded from Git and the Docker image.

## Validation and known limitation

The Docker image built successfully, the container started, and the inventory page loaded in GitHub Codespaces.

Database changes inside the container are not retained after the container is removed.
