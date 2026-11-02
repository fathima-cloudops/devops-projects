# Production-Ready DevOps Platform

A hands-on DevOps portfolio project demonstrating containerization, automated testing, health checks, CI/CD foundations, monitoring, and deployment recovery practices.

> **Project status:** Initial local Docker workflow and automated tests are included. Jenkins deployment, registry publishing, Terraform provisioning, and production rollback must be configured and tested before describing them as implemented.

## Architecture

Developer → GitHub → Jenkins (planned CI pipeline) → Tests → Docker Image → Deployment → Health Check

Prometheus scrapes application metrics. Docker Compose runs the application and Prometheus locally.

## Technology

- Python / Flask
- Docker and Docker Compose
- Jenkins (pipeline included)
- Prometheus
- Terraform (planned extension)
- Linux, Git, and GitHub

## Requirements

- Docker Engine
- Docker Compose plugin

## Run locally

```bash
docker compose up -d --build
docker compose ps
curl http://localhost:5000/
curl http://localhost:5000/health
curl http://localhost:5000/metrics
```

Prometheus is available at `http://localhost:9090` on the machine running Docker. The published ports bind to localhost by default.

View logs and stop the stack:

```bash
docker compose logs -f app
docker compose down
```

## Run tests without Docker

Python 3.12 or a compatible Python 3 version is required.

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r app/requirements.txt
pytest -q
```

On Windows PowerShell, activate the virtual environment with:

```powershell
.\.venv\Scripts\Activate.ps1
```

## CI/CD

The `Jenkinsfile` checks out the repository, installs dependencies, runs tests, and builds a versioned Docker image. It does **not** yet publish the image to a registry or deploy to a remote host. Add credentials through Jenkins Credentials and configure deployment only after testing the pipeline.

## Health checks and monitoring

- `/health` returns HTTP 200 while the application process responds.
- `/metrics` exposes a basic Prometheus uptime metric.
- Docker Compose health checks monitor the local application container.
- Prometheus scrapes the application and its own metrics.

This is a learning/portfolio implementation, not a complete production monitoring design. Add meaningful service-level metrics, alert rules, dashboards, authentication, TLS, and operational testing before production use.

## Security practices

- Application container runs as a non-root user.
- Application and monitoring ports bind to `127.0.0.1` by default.
- Do not commit secrets, private keys, `.env` files, Terraform state, or production configuration.
- Use least-privilege IAM roles and restricted security-group rules for any AWS deployment.
- Review Terraform plans and cloud costs before provisioning resources.

## Rollback approach

Use immutable image tags (for example, Git commit SHA or build number) and retain the last known-good image. A rollback script should only be added after the deployment method and image naming convention are defined and tested. Never claim rollback is tested until you have verified recovery from a failed deployment.

## Suggested next steps

1. Configure a Jenkins agent with Docker access and run the pipeline.
2. Add a registry push stage using credentials stored in Jenkins.
3. Implement and test deployment plus rollback on a disposable test host.
4. Add Terraform for a small AWS environment, review `terraform plan`, and destroy temporary resources when finished.
5. Add Grafana dashboards and actionable alert rules.
6. Add screenshots of actual successful tests and pipeline runs.

## Author

Fathima A. — DevOps portfolio project
