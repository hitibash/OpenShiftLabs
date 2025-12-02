# Changelog

All notable changes to this project will be documented in this file.

## [2025-12-02] - Lab Structure Enhancement

### Added
- Created `001-verify-cluster` module for cluster health verification between setup and user management
- Created `011-logging` module for centralized logging with EFK/ELK stack
- Created `012-scaling` module for HPA and manual scaling strategies

### Changed
- Renumbered all modules from 001-009 to 002-010 to accommodate new verify-cluster module
- Updated `mkdocs/06-mkdocs-nav.yml` with new module order and paths
- Updated root `README.md` to reflect all 13 modules (000-012) with accurate descriptions

### Technical Details
- All new modules follow the established template with README.md and _demo.sh scripts
- Module numbering now accommodates logical flow: setup → verify → user management → projects

## [2025-11-30] - Dynamic CI Workflow

### Added
- Created `.github/workflows/test-labs.yaml` with dynamic matrix testing
- Workflow auto-detects changed lab modules and runs their `_demo.sh` scripts in parallel
- Falls back to testing all modules when workflow itself changes

### Technical Details
- Uses git diff to identify modified modules (Labs/guides/NNN-*)
- Builds dynamic JSON matrix at runtime for parallel execution
- Includes comprehensive workflow documentation in file header
- CI-friendly with proper GitHub Actions conditionals (success/failure)

## [2025-11-29] - Guides Structure & Demo Scripts

### Added
- Created 11 lab modules under `Labs/guides/` (000-setup through 010-monitoring)
- Each module includes:
  - `README.md` with learning objectives and tasks
  - `_demo.sh` CI-friendly demo script for smoke testing
- Updated `mkdocs/06-mkdocs-nav.yml` to include only the new module READMEs in order
- Updated `README.md` with Course Modules section and Authoring Workflow

### Changed
- Renamed `001-setup` to `000-setup` and renumbered subsequent modules
- All demo scripts are now non-destructive and skip gracefully when tooling (oc, kubectl, docker) is missing or not authenticated
- Simplified navigation structure to focus on the course workplan modules

### Technical Details
- Demo scripts use consistent template with `info()` and `warn()` functions
- Scripts return exit code 0 even when tools are missing, making them CI-friendly
- Each module follows the proposed course workplan from the main README

