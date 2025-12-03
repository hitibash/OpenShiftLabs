# 001-Verify-Cluster — Verifying OpenShift Cluster Health

This module covers verifying the OpenShift cluster health and checking that all components are running properly.

## Objectives

- Verify cluster node status and health.
- Check cluster operators status.
- Validate cluster resources and capacity.

## Tasks

1. Use `oc get nodes` to verify all nodes are in Ready state.
2. Check cluster operators with `oc get clusteroperators` and ensure all are Available.
3. Review cluster resource usage using `oc adm top nodes`.
4. Verify API server connectivity and version with `oc version`.

Estimated time: 20–30 minutes
