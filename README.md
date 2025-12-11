# DevOps Capstone Project - Account Microservice
![Build Status](https://github.com/iklymchuk/devops-capstone-project/actions/workflows/ci-build.yaml/badge.svg)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Python 3.9](https://img.shields.io/badge/Python-3.9-green.svg)](https://shields.io/)

This repository contains a RESTful Account microservice developed as part of the IBM DevOps and Software Engineering Professional Certificate program. The service provides full CRUD operations for managing customer accounts with a PostgreSQL backend.

## Overview

This microservice implements a complete REST API for account management, including:
- Create new accounts
- Read account details
- Update existing accounts
- Delete accounts
- List all accounts

The project demonstrates modern DevOps practices including Test Driven Development (TDD), containerization with Docker, Kubernetes deployment, and CI/CD automation with Tekton pipelines.

## Technology Stack

- **Framework**: Flask 2.1.2
- **Database**: PostgreSQL with SQLAlchemy ORM
- **Testing**: Nose, Factory Boy, Coverage
- **Containerization**: Docker
- **Orchestration**: Kubernetes (K3D for local development)
- **CI/CD**: Tekton Pipelines
- **Code Quality**: Pylint, Flake8, Black

## Development Environment

These labs are designed to be executed in the IBM Developer Skills Network Cloud IDE with OpenShift. 

Once you are in the lab environment, you can initialize it with `bin/setup.sh` by sourcing it. (*Note: DO NOT run this program as a bash script. It sets environment variables and so must be sourced*):

```bash
source bin/setup.sh
```

This will install Python 3.9, make it the default, modify the bash prompt, create a Python virtual environment and activate it.

After sourcing it, your prompt should look like this:

```bash
(venv) theia:project$
```

## Useful commands

Under normal circumstances you should not have to run these commands. They are performed automatically at setup but may be useful when things go wrong:

### Activate the Python 3.9 virtual environment

You can activate the Python 3.9 environment with:

```bash
source ~/venv/bin/activate
```

### Installing Python dependencies

These dependencies are installed as part of the setup process but should you need to install them again, first make sure that the Python 3.9 virtual environment is activated and then use the `make install` command:

```bash
make install
```

### Starting the Postgres Docker container

The labs use Postgres running in a Docker container. If for some reason the service is not available you can start it with:

```bash
make db
```

You can use the `docker ps` command to make sure that postgres is up and running.

## Project layout

The code for the microservice is contained in the `service` package. All of the test are in the `tests` folder. The code follows the **Model-View-Controller** pattern with all of the database code and business logic in the model (`models.py`), and all of the RESTful routing on the controller (`routes.py`).

```text
├── service         <- microservice package
│   ├── common/     <- common log and error handlers
│   ├── config.py   <- Flask configuration object
│   ├── models.py   <- code for the persistent model
│   └── routes.py   <- code for the REST API routes
├── setup.cfg       <- tools setup config
└── tests                       <- folder for all of the tests
    ├── factories.py            <- test factories
    ├── test_cli_commands.py    <- CLI tests
    ├── test_models.py          <- model unit tests
    └── test_routes.py          <- route unit tests
```

## Data Model

The Account model contains the following fields:

| Name | Type | Optional |
|------|------|----------|
| id | Integer| False |
| name | String(64) | False |
| email | String(64) | False |
| address | String(256) | False |
| phone_number | String(32) | True |
| date_joined | Date | False |

## API Endpoints

The microservice exposes the following REST API endpoints:

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/health` | Health check endpoint |
| GET | `/` | Service information |
| POST | `/accounts` | Create a new account |
| GET | `/accounts` | List all accounts |
| GET | `/accounts/{id}` | Read a specific account |
| PUT | `/accounts/{id}` | Update an existing account |
| DELETE | `/accounts/{id}` | Delete an account |

## Running Tests

The project maintains 95%+ code coverage through comprehensive unit tests. To run the test suite:

## Running Tests

The project maintains 95%+ code coverage through comprehensive unit tests. To run the test suite:

```bash
# Run all tests
make test

# Run tests with coverage report
nosetests --with-coverage --cover-package=service
```

## Running the Service

To start the service locally:

```bash
# Start the PostgreSQL database
make db

# Run the service
flask run
```

The service will be available at `http://localhost:5000`.

## Local Kubernetes Development

This repo can also be used for local Kubernetes development. It is not advised that you run these commands in the Cloud IDE environment. The purpose of these commands are to simulate the Cloud IDE environment locally on your computer. 

At a minimum, you will need [Docker Desktop](https://www.docker.com/products/docker-desktop) installed on your computer. For the full development environment, you will also need [Visual Studio Code](https://code.visualstudio.com) with the [Remote Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension from the Visual Studio Marketplace. All of these can be installed manually by clicking on the links above or you can use a package manager like **Homebrew** on Mac of **Chocolatey** on Windows.

Please only use these commands for working stand-alone on your own computer with the VSCode Remote Container environment provided.

1. Bring up a local K3D Kubernetes cluster

    ```bash
    $ make cluster
    ```

2. Install Tekton

    ```bash
    $ make tekton
    ```

3. Install the ClusterTasks that the Cloud IDE has

    ```bash
    $ make clustertasks
    ```

You can now perform Tekton development locally, just like in the Cloud IDE lab environment.

## CI/CD Pipeline

The project includes Tekton pipeline definitions in the `tekton/` directory for automated:
- Code linting and testing
- Building Docker images
- Deploying to Kubernetes/OpenShift

## Author

**Ivan Klymchuk** - Based on the IBM DevOps Capstone Project template by [John Rofrano](https://www.coursera.org/instructor/johnrofrano)

## License

Licensed under the Apache License. See [LICENSE](LICENSE)

## <h3 align="center"> © IBM Corporation 2022. All rights reserved. <h3/>
