# Changelog

All notable changes to this project will be documented in this file.

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

