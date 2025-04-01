# uv-pypi-validator

This repository contains configurations and instructions for setting up and using a local PyPI server using [pypiserver](https://pypi.org/project/pypiserver/) to test package publishing and downloading with [uv](https://docs.astral.sh/uv/).

## Features

- Run a local PyPI server directly or using Docker/Docker Compose
- Set up basic auth to support for package publishing and downloading locally or in CI.
- Brief instructions for building, publishing, and installing packages using [uv](https://docs.astral.sh/uv/).

## Quick Start

1. Create required directories:
- Clone or checkout this repository, `cd` into it, and create a local directory for packages:
```bash
mkdir -p ./local-pypi/{packages,htpasswd}
```

1. Create a password file:
```bash
export PYINDEX_USER=<your-username>
htpasswd -sc ./local-pypi/htpasswd/.htpasswd ${PYINDEX_USER}
```

1. Start the server using Docker Compose:
```bash
docker compose up -d
```

1. Access your PyPI server at:
```
http://localhost:8081/simple/
```

See [NOTES.md](NOTES.md) for detailed usage instructions.

## Future Work

- **TODO**: Create a composite GitHub Action that can be used to test package publishing and installation in CI workflows. This will allow developers to validate their uv packaging configuration before publishing to the real PyPI.

## License

MIT

