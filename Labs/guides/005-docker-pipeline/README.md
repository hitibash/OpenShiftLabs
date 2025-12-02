# 005-Docker Pipeline — CI pipeline basics; building from source to image

This module covers creating a CI pipeline that builds a container image from source and promotes it to the cluster for deployment.

## Objectives

- Create a simple pipeline that builds, tests, and pushes an image.
- Integrate the pipeline with Git (webhooks) and OpenShift BuildConfigs if applicable.
- Validate pipeline success and automate rollback scenarios.

## Tasks

1. Configure a pipeline (Jenkins, Tekton, or OpenShift Pipelines) to build from Git and produce a container image.
2. Create a BuildConfig if using OpenShift-native builds.
3. Verify the pipeline's build logs and resulting ImageStream tags.

Estimated time: 60–90 minutes

