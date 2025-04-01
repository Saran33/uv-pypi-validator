## Run Pypi server locally
- https://pypi.org/project/pypiserver/

- Install pypiserver using uv
```bash
uv venv
uv pip install pypiserver passlib
```
- Export env variables for authentication
```bash
export PYINDEX_HOST=localhost
export PYINDEX_PORT=8081
export PYINDEX_USER=<your-username>
export PYINDEX_PASS=<your-password>
```

- Create directory for packages
```bash
mkdir -p ./local-pypi/{packages,htpasswd}
# Create a password file for authentication (required for publishing)
htpasswd -sc ./local-pypi/htpasswd/.htpasswd ${PYINDEX_USER}
```
- Run pypiserver
```bash
pypi-server run -p ${PYINDEX_PORT} -P ./local-pypi/htpasswd/.htpasswd -a update,download,list ./local-pypi/packages -v
```

## Using the Docker Image
```bash
docker run -p ${PYINDEX_PORT}:8080 \
-v ./local-pypi/packages:/data/packages \
-v ./local-pypi/htpasswd:/data/auth \
pypiserver/pypiserver:latest run -P /data/auth/.htpasswd -a update,download,list /data/packages
```

## Using Docker Compose
- Create the `docker-compose.yml` file as shown in this repository.
- Create a local directory for packages
```bash
mkdir -p ./local-pypi/{packages,htpasswd}
# Create a password file for authentication (required for publishing)
htpasswd -sc ./local-pypi/htpasswd/.htpasswd saran
```
- Start the pypiserver using Docker Compose:
```bash
docker compose up -d
```
- Stop the pypiserver:
```bash
docker compose down
```

- Access the server at:
```bash
echo http://$PYINDEX_HOST:$PYINDEX_PORT/simple/
```

## Building and publishing packages with uv
### Build the package
- Build your package(s) in the project directory:
```bash
cd your-package-directory
uv build --force-pep517 --all-packages
```

### Upload the package
- Upload your package(s) to the local PyPI server:
```bash
export PYINDEX_HOST=localhost
export PYINDEX_PORT=8081
export PYINDEX_USER=<your-username>
export PYINDEX_PASS=<your-password>

uv publish \
		--publish-url http://${PYINDEX_HOST}:${PYINDEX_PORT} \
		--check-url http://${PYINDEX_HOST}:${PYINDEX_PORT}/simple/ \
		-u ${PYINDEX_USER} \
		-p ${PYINDEX_PASS} \
		dist/*
```

## Install the package

- Install packages from your local PyPI server:
```bash
uv pip install --index-url http://${PYINDEX_USER}:${PYINDEX_PASS}@${PYINDEX_HOST}:${PYINDEX_PORT}/simple/ "<your-package>" \
	# --dry-run
```

For more information on building packages with uv, see: https://docs.astral.sh/uv/guides/package/#building-your-package
