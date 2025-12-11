# Lab 001: Verify Cluster Health

## Overview

This lab covers cluster node status verification, cluster operators, resource capacity, and API server connectivity using both CLI and web console.

## Prerequisites

- Completed [Lab 000: Installation and Initial Setup](../000-setup/README.md)
- OpenShift Local (CRC) running
- `oc` CLI tool installed and configured
- Logged in as `developer` or `kubeadmin`

---

## Lab Instructions

### Step 1: Verify API Server Connectivity

First, confirm you can communicate with the cluster:

```bash
oc version
```

Expected output:

```text
Client Version: 4.x.x
Kustomize Version: v5.x.x
Server Version: 4.x.x
Kubernetes Version: v1.xx.x
```

Check cluster info:

```bash
oc cluster-info
```

Expected output:

```text
Kubernetes control plane is running at https://api.crc.testing:6443

To further debug and diagnose cluster problems, use 'oc cluster-info dump'.
```

---

### Step 2: Check Node Status

Verify all nodes are in Ready state:

```bash
oc get nodes
```

Expected output:

```text
NAME   STATUS   ROLES                         AGE   VERSION
crc    Ready    control-plane,master,worker   5d    v1.xx.x
```

**Status Indicators:**

| Status | Description |
|--------|-------------|
| **Ready** | Node is healthy and accepting workloads |
| **NotReady** | Node has issues and cannot accept new workloads |
| **SchedulingDisabled** | Node is cordoned (administrative action) |

Get detailed node information:

```bash
oc describe node crc
```

This shows:

- Node conditions (Ready, MemoryPressure, DiskPressure, PIDPressure)
- Allocatable resources (CPU, memory, pods)
- System information (OS, kernel, container runtime)
- Resource usage

---

### Step 3: Check Cluster Operators

Cluster operators manage core OpenShift components. Verify all are healthy:

```bash
oc get clusteroperators
```

Expected output (sample):

```text
NAME                                       VERSION   AVAILABLE   PROGRESSING   DEGRADED   SINCE   MESSAGE
authentication                             4.x.x     True        False         False      5d
cloud-credential                           4.x.x     True        False         False      5d
cluster-autoscaler                         4.x.x     True        False         False      5d
config-operator                            4.x.x     True        False         False      5d
console                                    4.x.x     True        False         False      5d
dns                                        4.x.x     True        False         False      5d
etcd                                       4.x.x     True        False         False      5d
image-registry                             4.x.x     True        False         False      5d
ingress                                    4.x.x     True        False         False      5d
kube-apiserver                             4.x.x     True        False         False      5d
kube-controller-manager                    4.x.x     True        False         False      5d
kube-scheduler                             4.x.x     True        False         False      5d
monitoring                                 4.x.x     True        False         False      5d
network                                    4.x.x     True        False         False      5d
node-tuning                                4.x.x     True        False         False      5d
openshift-apiserver                        4.x.x     True        False         False      5d
openshift-controller-manager               4.x.x     True        False         False      5d
openshift-samples                          4.x.x     True        False         False      5d
operator-lifecycle-manager                 4.x.x     True        False         False      5d
operator-lifecycle-manager-catalog         4.x.x     True        False         False      5d
operator-lifecycle-manager-packageserver   4.x.x     True        False         False      5d
service-ca                                 4.x.x     True        False         False      5d
storage                                    4.x.x     True        False         False      5d
```

**Status Columns:**

| Column | Meaning |
|--------|---------|
| **AVAILABLE** | Operator is functional and serving requests |
| **PROGRESSING** | Operator is rolling out changes or updates |
| **DEGRADED** | Operator has errors or partial functionality |

Check for any degraded operators:

```bash
oc get clusteroperators | grep -v "True.*False.*False"
```

If all operators are healthy, only the header line appears.

Get details on a specific operator:

```bash
oc describe clusteroperator kube-apiserver
```

---

### Step 4: Verify Core Namespaces

List all namespaces to ensure core components are present:

```bash
oc get namespaces
```

Expected output includes these core namespaces:

```text
NAME                                               STATUS   AGE
default                                            Active   5d
kube-node-lease                                    Active   5d
kube-public                                        Active   5d
kube-system                                        Active   5d
openshift                                          Active   5d
openshift-apiserver                                Active   5d
openshift-authentication                           Active   5d
openshift-console                                  Active   5d
openshift-controller-manager                       Active   5d
openshift-dns                                      Active   5d
openshift-etcd                                     Active   5d
openshift-image-registry                           Active   5d
openshift-ingress                                  Active   5d
openshift-kube-apiserver                           Active   5d
openshift-kube-controller-manager                  Active   5d
openshift-kube-scheduler                           Active   5d
openshift-monitoring                               Active   5d
openshift-network-diagnostics                      Active   5d
openshift-operator-lifecycle-manager               Active   5d
```

Check pods in critical namespaces:

```bash
oc get pods -n openshift-kube-apiserver
oc get pods -n openshift-etcd
oc get pods -n openshift-ingress
```

All pods should be in `Running` status.

---

### Step 5: View Cluster in Web Console

1. Open the web console: `https://console-openshift-console.apps-crc.testing`
2. Log in as `kubeadmin` (use admin credentials from Lab 000)
3. Navigate to **Home** → **Overview**

   The Overview dashboard shows cluster status and health.

4. Click **Compute** → **Nodes** → **crc**

   This shows detailed node information including:

   - Running pods
   - Node conditions
   - Events

---

### Step 6: Check Cluster Events

View recent cluster events:

```bash
oc get events -A --sort-by='.lastTimestamp' | tail -20
```

This shows the last 20 events across all namespaces, useful for identifying recent issues.

Filter for warning/error events:

```bash
oc get events -A --field-selector type!=Normal
```

---

### Step 7: Verify Container Runtime

Check that the container runtime is functioning:

```bash
oc debug node/crc
```

This starts a debug pod on the node. Once inside:

```bash
chroot /host
crictl version
crictl ps | head -10
exit
exit
```

This verifies CRI-O (the container runtime) is working and shows running containers.

---

## Understanding Cluster Health

### Healthy Cluster Indicators

- All nodes show `STATUS: Ready`
- All cluster operators show `AVAILABLE: True`, `PROGRESSING: False`, `DEGRADED: False`
- Core namespace pods are `Running`
- API server responds to `oc` commands
- No persistent error events

### Unhealthy Cluster Indicators

- Nodes show `NotReady` status
- Operators show `DEGRADED: True` or `AVAILABLE: False`
- Pods in `CrashLoopBackOff`, `Error`, or `Pending` state
- Repeated error events in `oc get events`
- API server timeouts or connection failures

---

## Troubleshooting

**Operators showing DEGRADED or not AVAILABLE:**

```bash
# Get operator details
oc describe clusteroperator <operator-name>

# Check operator pods
oc get pods -n openshift-<operator-namespace>

# View operator logs
oc logs -n openshift-<operator-namespace> <pod-name>
```

**Nodes showing NotReady:**

```bash
# Describe the node to see conditions
oc describe node <node-name>

# Check node events
oc get events --field-selector involvedObject.name=<node-name>
```

For CRC specifically, this usually indicates the VM needs to be restarted:

```bash
crc stop
crc start
```

**API server not responding:**

Verify CRC is running:

```bash
crc status
```

Check authentication:

```bash
oc whoami
# If not authenticated, log in again
oc login -u developer -p developer https://api.crc.testing:6443
```

---

## Quick Reference

```bash
# Connectivity
oc version                                          # Client and server versions
oc cluster-info                                     # Cluster endpoint information

# Nodes
oc get nodes                                        # List all nodes
oc describe node <node-name>                        # Detailed node information

# Cluster Operators
oc get clusteroperators                             # List all operators
oc describe clusteroperator <operator-name>         # Operator details

# Namespaces and Pods
oc get namespaces                                   # List all namespaces
oc get pods -A                                      # List all pods

# Events
oc get events -A --sort-by='.lastTimestamp'         # Recent events
oc get events -A --field-selector type!=Normal      # Warning/error events

# Debugging
oc debug node/<node-name>                           # Start debug pod on node
```

---

## Next Steps

Continue to [Lab 002: Create New User](../002-new-user/README.md) to learn about user management in OpenShift.
