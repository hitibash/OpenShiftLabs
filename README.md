# OpenShiftLabs — Documentation and Hands-on Labs for OpenShift

This repository curates documentation and hands-on labs for learning OpenShift and containerized application lifecycle management.
The objective is to provide structured modules suitable for classroom instruction, self-study, and practical exercises used by developers and operators.

## Project Vision

We want to create clear, accessible, and repeatable labs that teach practical OpenShift concepts: from provisioning and setup to CI/CD pipelines and monitoring.
Each lab should include objectives, prerequisites, a step-by-step walkthrough, and test assertions so students can safely practice in disposable clusters.


## 📁 Repository Layout

Key directories and files:

- `Labs/` — Markdown content for guides, tutorials, and hands-on labs. Modules are organized by subject (e.g., `guides/`, `tutorials/`, `reference/`).
- `mkdocs/` — modular MkDocs configuration used to build site.
- `init_site.sh` — setup helper script to initialize local dev environment.
- `requirements.txt` — Python (MkDocs) requirements for local build.
- `.github/workflows/` — Contains CI workflows that validate docs and run build tests.



---
## How To Use This Repo (Quick Start)

### Prerequisites

- Access to a local or cloud OpenShift cluster (CRC/Minishift/minikube+OKD, or using a hosted OpenShift cluster)
- Git and Docker / Podman or Buildah installed for building images
- MkDocs + mkdocs-material installed for local previews (optional but helpful)

Recommended quick-setup commands for local preview (works in bash):

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```
# Authoring Workflow

Follow these steps when creating or maintaining labs:

1. **Clone and branch** — Clone this repository and create a feature branch for your changes.
2. **Choose or create a module** — Work within an existing module under `Labs/guides/NNN-module-name/` or create a new numbered module following the naming convention (e.g., `013-new-topic`).
3. **Add content** — Each module should include:
   - `README.md` — Module overview with learning objectives, tasks, and estimated duration
   - `_demo.sh` — CI-friendly demo script that checks tooling and performs smoke tests (use existing modules as templates (temporarely))
   - Additional lab files as needed (step-by-step guides, manifests, scripts)
4. **Update navigation** — Add your module to `mkdocs/06-mkdocs-nav.yml` in the appropriate order under "How-to Guides"
5. **Test locally** — Run `mkdocs serve` to preview the docs site and test your `_demo.sh` script
6. **Submit PR** — Create a pull request with a clear description of your changes


### Tips for Maintainers


- Include validation steps so students can verify successful completion
- Use consistent formatting and terminology across modules

---
## Course Modules (Labs/guides)

A structured lab modules under `Labs/guides/` following the proposed course workplan. Each module includes:

- **README.md** — Short description, learning objectives, and tasks
- **_demo.sh** — CI-friendly demo script that checks for required tooling and performs basic smoke tests

Modules are numbered for sequential learning:

- **000-setup** — Accessing OpenShift web console.
- **001-verify-cluster** — Verifying OpenShift cluster health and checking component status
- **002-new-user** — Creating and managing users and roles
- **003-new-project** — Creating namespaces/projects with quotas and limits
- **004-docker-lifecycle** — Building, tagging, and managing local Docker images
- **005-docker-pipeline** — CI pipeline basics; building from source to image
- **006-hooks-setup** — Git hooks, build hooks, and webhooks in pipelines
- **007-images-imagestream** — Using BuildConfigs, ImageStreams, and registry interactions
- **008-deploying** — DeploymentConfig vs K8s Deployment, rolling strategies, and scaling
- **009-services-routes** — Exposing services using routes, Ingress, and load balancing
- **010-monitoring** — Prometheus, Grafana, and alerts; instrumenting applications
- **011-logging** — Centralized logging with EFK/ELK stack for cluster and application logs
- **012-scaling** — Horizontal Pod Autoscaling and manual scaling strategies


## Recent Changes

See `CHANGELOG.md` for a summary of recent updates to the repository structure and content.

---

## Detailed Module Checklist

Every lab should include the following items.

- Title and short description
- Learning Objectives (3–5 bullets)
- Prerequisites (tools, cluster, credentials)
- Scenario / Storyline (what they will build or fix)
- Steps with commands and commands outputs where applicable
- Validation example (how to know the lab succeeded)
- Estimated duration

Example validation for Docker pipeline:

- Build image via pipeline and confirm image tag exists in ImageStream/registry.
- Deploy from the pipeline's image and confirm the application responds on its route.
- Check that pipeline artifacts are recorded in OpenShift build logs.

---

**Happy teaching and building!** 🌟

