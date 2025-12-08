# Lab 000: OpenShift Installation and Initial Setup

## Overview

Welcome to OpenShiftLabs! This lab guides you through the complete setup process for OpenShift, from installation to creating your first project. You'll learn about different installation options, set up a local OpenShift cluster using OpenShift Local (formerly CodeReady Containers), install the `oc` command-line tool, and access the web console to verify your environment is ready for the upcoming labs.

## Learning Objectives

By completing this lab, you will:

- Understand OpenShift 4 installation options
- Install OpenShift Local (CRC) for local development
- Install and configure the `oc` CLI tool
- Start and access your OpenShift cluster
- Log into the OpenShift web console
- Understand the difference between Administrator and Developer perspectives
- Create your first project in OpenShift
- Verify cluster health and connectivity

## Prerequisites

**System Requirements:**

- **Operating System**: Linux (Ubuntu, Fedora, RHEL, CentOS, etc.)
- **RAM**: Minimum 9 GB of available RAM (16 GB recommended)
- **CPU**: 4 virtual CPUs minimum
- **Disk Space**: 35 GB of free disk space
- **Virtualization**: Hardware virtualization enabled in BIOS with KVM support

**Software Prerequisites:**

- Administrator/root access on your machine
- A Red Hat account (free to create at https://www.redhat.com)
- Internet connection for downloads
- Web browser (Chrome or Firefox)

**Note**: If you already have access to an existing OpenShift cluster (cloud-hosted or organization-provided), you can skip the installation steps and proceed directly to **Step 3: Install the oc CLI Tool**.

---

## Background: OpenShift Installation Options

OpenShift 4 offers several installation options depending on your needs:

### 1. OpenShift Local (Recommended for Learning)

**Formerly known as CodeReady Containers (CRC)**

- Single-node OpenShift cluster running on your laptop
- Official Red Hat solution for local development
- Pre-configured with all OpenShift components
- Acts as both control plane and worker node
- Perfect for learning, testing, and development
- **This is what we'll use in this lab**

### 2. OKD (Upstream Community Version)

- Free, open-source community distribution
- Upstream version of OpenShift
- No Red Hat subscription required
- Less stable than OpenShift Local
- Good for contributing to the project or testing bleeding-edge features

### 3. OpenShift Container Platform (Production)

- Full enterprise installation on bare metal or VMs
- Multi-node clusters with high availability
- Requires Red Hat subscription
- Supports on-premises, AWS, Azure, GCP, IBM Cloud, and more
- Best for production environments

### 4. Managed OpenShift Services

- **Red Hat OpenShift on AWS (ROSA)**
- **Azure Red Hat OpenShift (ARO)**
- **OpenShift Dedicated**
- Fully managed by Red Hat/Cloud Provider
- Pay-as-you-go pricing

**For this lab series, we'll use OpenShift Local**, which provides a full-featured OpenShift environment without the complexity of a production installation.

---

## Lab Instructions

### Step 1: Create a Red Hat Account and Get Pull Secret

Before installing OpenShift Local, you need a Red Hat account and pull secret to authenticate.

1. **Create a Red Hat Account** (if you don't have one):
   - Visit: https://www.redhat.com
   - Click **Register** or **Create an account**
   - Fill in your details and verify your email
   - This is completely free for developers

2. **Download Your Pull Secret**:
   - Navigate to: https://console.redhat.com/openshift/create/local
   - Log in with your Red Hat account
   - You'll see the **OpenShift Local** download page
   - Click **Download pull secret** - save this file (it's JSON format)
   - Keep this file safe - you'll need it during CRC setup

---

### Step 2: Install OpenShift Local (CRC)

Now let's install OpenShift Local on your Linux system.

1. **Download OpenShift Local**:
   - Visit: https://console.redhat.com/openshift/create/local
   - Download the **Linux** version (`.tar.xz` file)
   - Current version will be something like `crc-linux-amd64.tar.xz`

2. **Extract and Install**:
   ```bash
   cd ~/Downloads
   tar -xvf crc-linux-*.tar.xz
   cd crc-linux-*-amd64
   sudo cp crc /usr/local/bin/
   ```

3. **Verify Installation**:
   ```bash
   crc version
   ```

   Expected output:
   ```
   CRC version: 2.x.x+<commit>
   OpenShift version: 4.x.x
   Podman version: 4.x.x
   ```

4. **Set Up CRC**:
   ```bash
   crc setup
   ```

   This command will:
   - Download necessary dependencies
   - Set up virtualization (libvirt/KVM)
   - Configure networking
   - Takes 5-10 minutes

---

### Step 3: Start OpenShift Local

Now let's start your local OpenShift cluster.

1. **Start CRC** (this will take 10-15 minutes on first run):
   ```bash
   crc start
   ```

2. **When prompted**, provide your pull secret:
   - Copy the contents of the pull secret file you downloaded
   - Paste it when prompted
   - Or specify the file directly:
     ```bash
     crc start --pull-secret-file ~/Downloads/pull-secret.txt
     ```

3. **Wait for startup**. You'll see output like:
   ```
   INFO Checking if running as non-root
   INFO Checking if crc-admin-helper executable is cached
   INFO Checking for obsolete admin-helper executable
   INFO Checking if running on a supported CPU architecture
   INFO Checking if crc executable symlink exists
   INFO Checking minimum RAM requirements
   INFO Check if Podman binary exists in: /usr/local/bin/podman
   INFO Checking if running emulated on Apple silicon
   INFO Starting CRC VM for OpenShift 4.x.x...
   INFO CRC instance is running with IP 192.168.130.11
   INFO CRC VM is running
   INFO Updating authorized keys...
   INFO Configuring shared directories
   INFO Check internal and public DNS query...
   INFO Check DNS query from host...
   INFO Verifying validity of the kubelet certificates...
   INFO Starting OpenShift kubelet service
   INFO Waiting for kube-apiserver availability... [takes around 2min]
   INFO Waiting for user's pull secret part of instance disk...
   INFO Operator openshift-controller-manager is progressing
   INFO All operators are available. Ensuring stability...
   INFO Operators are stable (2/3)...
   INFO Operators are stable (3/3)...
   INFO Adding crc-admin and crc-developer contexts to kubeconfig...
   Started the OpenShift cluster.

   The server is accessible via web console at:
     https://console-openshift-console.apps-crc.testing

   Log in as administrator:
     Username: kubeadmin
     Password: <password-will-be-shown-here>

   Log in as user:
     Username: developer
     Password: developer

   Use the 'oc' command line interface:
     $ eval $(crc oc-env)
     $ oc login -u developer https://api.crc.testing:6443
   ```

4. **Save the credentials** displayed at the end:
   - **Admin username**: `kubeadmin`
   - **Admin password**: (shown in output - copy it!)
   - **Developer username**: `developer`
   - **Developer password**: `developer`
   - **Web console URL**: `https://console-openshift-console.apps-crc.testing`

5. **Verify CRC is running**:
   ```bash
   crc status
   ```

   Expected output:
   ```
   CRC VM:          Running
   OpenShift:       Running (v4.x.x)
   Podman:          Running
   RAM Usage:       4.5GB of 9GB
   Disk Usage:      15GB of 35GB
   ```

---

### Step 4: Install the oc CLI Tool

The `oc` command-line tool is essential for interacting with OpenShift from your terminal. OpenShift Local includes `oc`, but you need to add it to your PATH.

#### Option 1: Use oc from CRC (Easiest)

1. **Add CRC's oc to your PATH**:
   ```bash
   eval $(crc oc-env)
   ```

   To make this permanent, add to your shell profile (`~/.bashrc`, `~/.zshrc`):
   ```bash
   echo 'eval $(crc oc-env)' >> ~/.bashrc
   source ~/.bashrc
   ```

2. **Verify oc is available**:
   ```bash
   oc version
   ```

   Expected output:
   ```
   Client Version: 4.x.x
   Kubernetes Version: v1.xx.x
   ```

#### Option 2: Download oc Separately

If you prefer to install `oc` independently or need a specific version:

**Method 1: Download from Mirror Site**

1. Visit: https://mirror.openshift.com/pub/openshift-v4/clients/oc/latest/

2. **Download and install**:
   ```bash
   cd ~/Downloads
   wget https://mirror.openshift.com/pub/openshift-v4/clients/oc/latest/linux/oc.tar.gz
   tar -xzf oc.tar.gz
   sudo mv oc /usr/local/bin/
   sudo chmod +x /usr/local/bin/oc
   ```

**Method 2: From OpenShift Web Console** (after Step 6)

Once you access the web console, you can download the `oc` binary directly from OpenShift:

1. Click the **?** (help) icon in the top-right corner
2. Select **Command Line Tools**
3. Download the Linux binary

3. **Verify Installation**:
   ```bash
   oc version
   ```

---

### Step 5: Log in to OpenShift via CLI

Now let's authenticate with your OpenShift cluster using the `oc` CLI.

1. **Log in as developer** (good for learning):
   ```bash
   oc login -u developer -p developer https://api.crc.testing:6443
   ```

   Expected output:
   ```
   Login successful.

   You don't have any projects. You can try to create a new project, by running

       oc new-project <projectname>
   ```

2. **Alternatively, log in as admin** (kubeadmin):
   ```bash
   oc login -u kubeadmin -p <password-from-crc-start> https://api.crc.testing:6443
   ```

3. **Check your current user**:
   ```bash
   oc whoami
   ```

   Output: `developer` or `kubeadmin`

4. **View cluster information**:
   ```bash
   oc cluster-info
   ```

   Expected output:
   ```
   Kubernetes control plane is running at https://api.crc.testing:6443
   ```

5. **Check cluster nodes**:
   ```bash
   oc get nodes
   ```

   Expected output:
   ```
   NAME                 STATUS   ROLES                         AGE   VERSION
   crc-xxxxx-master-0   Ready    control-plane,master,worker   5m    v1.xx.x+xxxxx
   ```

   You should see one node in **Ready** status (CRC is single-node).

---

### Step 6: Access the Web Console

The OpenShift Container Platform web console is a user-friendly graphical interface accessible from any web browser. It provides two distinct perspectives designed for different user roles:

**Administrator Perspective:**

- Designed for cluster administrators and operations teams
- Provides infrastructure-level access to cluster resources
- Manage nodes, storage, networking, and cluster-wide settings
- Monitor overall cluster health and performance
- Default view for users with cluster-admin privileges

**Developer Perspective:**

- Designed for application developers
- Focuses on application deployment and development workflows
- Simplified interface for building, deploying, and managing applications
- Topology view for visualizing application components
- Default view for non-administrative users

Both perspectives access the same cluster but present information in ways that match different workflows and responsibilities.

**Now let's access the web console:**

1. **Open your web browser** (Chrome or Firefox recommended)

2. **Navigate to the web console URL**:

   **For OpenShift Local (CRC)**:
   ```
   https://console-openshift-console.apps-crc.testing
   ```

   **For other clusters**, find the URL:
   ```bash
   oc whoami --show-console
   ```

3. **Accept the SSL Certificate Warning**:
   - OpenShift Local uses self-signed certificates
   - You'll see a security warning in your browser
   - Click **Advanced** → **Proceed** (or similar for your browser)
   - This is safe for local development clusters

4. **You'll see the OpenShift Container Platform login page**

5. **Log in using CRC credentials**:

   **As developer** (recommended for learning):
   - Username: `developer`
   - Password: `developer`
   - Click **Log in**

   **Or as admin** (kubeadmin):
   - Username: `kubeadmin`
   - Password: (use the password from `crc start` output)
   - Click **Log in**

6. After successful login, you're redirected to the OpenShift web console

![alt text](image.png)

---

### Step 7: Understand the Two Perspectives

After logging in, you'll see the web console interface.

1. **Locate the Perspective Switcher:**
   - Look at the top-left corner of the console
   - You'll see either **Administrator** or **Developer** displayed
   - This is the perspective switcher dropdown

this is not correct anymore
![alt text](image-1.png)

2. **Click the perspective switcher** to see available perspectives:
   - **Administrator**: Infrastructure and cluster management
   - **Developer**: Application development and deployment

3. **Switch to Developer Perspective:**
   - Click on **Developer** in the dropdown
   - The interface changes to a developer-focused view
   - Notice the left navigation menu updates with developer-centric options

4. **Switch to Administrator Perspective:**
   - Click the perspective switcher again
   - Select **Administrator**
   - The interface shows cluster-level resources and management tools

**Key Differences:**

| Feature | Developer Perspective | Administrator Perspective |
|---------|----------------------|--------------------------|
| **Focus** | Applications and workloads | Infrastructure and cluster |
| **Topology View** | Visual application map | N/A |
| **Default View** | Topology | Overview/Dashboard |
| **Navigation** | +Add, Topology, Builds, Pipelines | Compute, Networking, Storage, Administration |
| **User Audience** | Developers | Cluster admins, operations |

---

### Step 8: Verify Cluster Health (Administrator Perspective)

Let's verify you can access cluster information and that the cluster is healthy.

1. **Switch to Administrator perspective** (if not already there)

2. Click **Compute** in the left navigation menu

3. Click **Nodes** under the Compute section

4. You should see a list of cluster nodes:
   - **Name**: Node hostname
   - **Status**: Should show **Ready** for healthy nodes
   - **Roles**: master, worker, or both
   - **Age**: How long the node has been running
   - **Version**: OpenShift/Kubernetes version

![alt text](image-2.png)

![alt text](image-3.png)

5. Click on any node name to see detailed information:
   - Overview tab: CPU, memory, pod count
   - YAML tab: Full node configuration
   - Pods tab: Pods running on this node
   - Events tab: Recent node events

6. **Verify cluster health:**
   - All nodes should show **Ready** status
   - If any node shows **NotReady** or other errors, contact your cluster administrator

   - For OpenShift Local: One node with roles `control-plane,master,worker`
   - Status should be **Ready**

---

### Step 9: Explore the Developer Perspective

Now let's explore the developer-focused interface.

1. **Switch to Developer perspective** \
   """
      username: `developer`, password: `developer`
   """

![alt text](image-4.png)

2. **Explore the left navigation menu:**
   - **+Add**: Deploy applications (from Git, container images, templates)
   - **Topology**: Visual representation of your applications
   - **Search**: Find resources across projects
   - **Builds**: View and manage application builds
   - **Pipelines**: CI/CD pipeline management (if Tekton is installed)
   - **Project**: Project dropdown at the top

3. **Notice the Project dropdown** at the top of the left navigation:
   - Shows your current project context
   - All resources you create belong to a project
   - Projects provide isolation between different applications/teams

---

### Step 10: Create Your First Project

Projects in OpenShift provide namespace isolation and are where you deploy applications.

1. Ensure you're in **Developer perspective**

2. **Click the Project dropdown** at the top of the left navigation:
   - You may see a message "No projects found" if this is a new cluster
   - Or you may see existing projects if others have created them

3. Click **Create Project**

4. A dialog appears with the following fields:

5. **Fill in the project details:**
   - **Name**: `lab-000-setup`
     - Must be lowercase, alphanumeric, and hyphens
     - Must be unique across the cluster
   - **Display name**: `Getting Started with OpenShift` (optional)
     - Human-readable name shown in the console
   - **Description**: `This project is for the OpenShift setup lab.` (optional)
     - Helps document the project's purpose

6. Click **Create**

7. **Verify project creation:**
   - You're now in the `lab-getting-started` project
   - The Project dropdown shows your new project
   - The Topology view is empty (you haven't deployed anything yet)
   - You'll see a message: "No resources found"

![alt text](image-5.png)

---

### Step 11: Navigate Your Project

Now that you have a project, let's explore its structure.

1. With your `lab-getting-started` project selected, click **Topology** in the left menu (under Workloads):
   - This is the main application view
   - Currently empty because you haven't deployed any applications
   - In future labs, you'll see visual representations of your apps here

2. Click the **+Add** option in the left navigation:    # TODO: need to update this
   - Shows all the ways you can deploy applications
   - **Import from Git**: Build and deploy from source code
   - **Container images**: Deploy from existing images
   - **Developer Catalog**: Browse pre-built templates and services
   - **Database**: Deploy databases quickly
   - We'll explore these in upcoming labs

3. Inside **Projects**, click on our new project "lab-000-setup"
   - Shows project details, quotas, and limits
   - **Details** tab: Project metadata
   - **YAML** tab: Project configuration
   - **Workloads** tab: View pods, deployments, etc.
   - **RoleBindings** tab: User access control

![alt text](image-6.png)

---

## Managing OpenShift Local (CRC)

Now that you have OpenShift running, here are some useful commands for managing your local cluster:

### Stop the Cluster

When you're done working, stop CRC to free up system resources:

```bash
crc stop
```

This preserves your cluster state. Next time you run `crc start`, everything will be as you left it.

### Start the Cluster

Restart your stopped cluster:

```bash
crc start
```

### Check Cluster Status

View cluster status anytime:

```bash
crc status
```

### Delete the Cluster

To completely remove the cluster and start fresh:

```bash
crc delete
```

Warning: This deletes all projects, applications, and data. You'll need to run `crc start` to create a new cluster.

### View Console Credentials

If you forget the kubeadmin password:

```bash
crc console --credentials
```

### Open Console in Browser

Quickly open the web console:

```bash
crc console
```

---

## Key Concepts

### OpenShift Local (CRC)

- Single-node OpenShift cluster for local development
- Runs in a virtual machine using KVM/libvirt on Linux
- Pre-configured with OpenShift 4.x
- Includes all standard OpenShift components
- Perfect for learning, testing, and development
- Stops/starts quickly, preserving state

### oc Command-Line Tool

- Primary CLI for interacting with OpenShift
- Superset of `kubectl` (works with any Kubernetes cluster)
- Authenticate with `oc login`
- Create resources with `oc create`, `oc apply`
- View resources with `oc get`, `oc describe`
- Includes OpenShift-specific commands not in kubectl

### Web Console

- The primary graphical interface for OpenShift
- Accessible via web browser
- Provides both Administrator and Developer perspectives
- Alternative to `oc` command-line tool
- Runs on HTTPS with role-based access control (RBAC)

### Perspectives      # TODO: verify

- Two distinct user interfaces within one console
- **Administrator**: Infrastructure management, cluster-wide resources
- **Developer**: Application-centric workflows and deployment
- Same underlying cluster, different presentation
- Users can switch between perspectives based on their roles

### Projects

- Kubernetes namespace with additional annotations
- Provide isolation between applications and teams
- All resources (pods, services, routes) belong to a project
- Enable resource quotas and limit ranges
- Support role-based access control (RBAC)
- Cannot have duplicate names across the cluster

### Navigation

- Left sidebar: Main navigation menu
- Top bar: User menu, perspective switcher, search
- Breadcrumbs: Show current location in the console
- Context: Current project determines what resources you see

---

## Understanding Projects vs. Namespaces

**In Kubernetes:**

- Namespaces provide scope for resources
- Basic isolation and naming boundaries

**In OpenShift:**

- Projects are Kubernetes namespaces with additional features:
  - User access control and security policies
  - Resource quotas and limit ranges
  - Network policies and isolation
  - Service accounts and secrets
  - Human-readable display names and descriptions

**Bottom line:** When you create a Project in OpenShift, you're creating a Namespace with added management features.

---

## Common Console Tasks Reference

| Task | Perspective | Navigation Path |
|------|------------|----------------|
| View cluster nodes | Administrator | Compute → Nodes |
| Create a project | Developer | Project dropdown → Create Project |
| Deploy an application | Developer | +Add → Import from Git |
| View application topology | Developer | Topology |
| Check pod logs | Either | Workloads → Pods → Pod name → Logs |
| View cluster events | Administrator | Home → Events |
| Manage users | Administrator | User Management → Users |
| View resource quotas | Administrator | Administration → ResourceQuotas |

---

## Troubleshooting

### CRC Installation Issues

**"crc setup" fails with virtualization errors**

- Ensure KVM is installed and enabled:

  ```bash
  # Check if KVM is available
  lsmod | grep kvm

  # Install KVM and libvirt (Ubuntu/Debian)
  sudo apt install qemu-kvm libvirt-daemon libvirt-daemon-system

  # Install KVM and libvirt (Fedora/RHEL)
  sudo dnf install qemu-kvm libvirt libvirt-daemon-kvm
  ```

- Add your user to the `libvirt` group:

  ```bash
  sudo usermod -aG libvirt $USER
  sudo systemctl restart libvirtd
  ```

- Log out and back in for group changes to take effect

- Verify virtualization is enabled in BIOS:

  ```bash
  egrep -c '(vmx|svm)' /proc/cpuinfo
  # Should return a number greater than 0
  ```

**"Not enough memory" error**

- CRC requires minimum 9 GB RAM available
- Close other applications to free memory
- Check system resources: `crc status`
- On Linux, check available memory: `free -h`

**"crc start" hangs or times out**

- Check your internet connection (CRC downloads images)
- Increase timeout: `crc config set pull-secret-file <path> --timeout 600`
- Check firewall settings aren't blocking VM networking
- Try deleting and recreating: `crc delete` then `crc start`

**Pull secret issues**

- Ensure you copied the full JSON content
- Re-download from: https://console.redhat.com/openshift/create/local
- Specify directly: `crc start --pull-secret-file ~/Downloads/pull-secret.txt`

### Web Console Access Issues

**Can't access console URL**

- Verify CRC is running: `crc status`
- Check the correct URL: `crc console --url`
- Ensure you accepted the self-signed certificate warning
- Try using a different browser (Chrome or Firefox)
- Check `/etc/hosts` includes CRC domains:

  ```bash
  cat /etc/hosts | grep crc.testing
  ```

  Should show entries like:
  
  ```bash
  192.168.130.11 api.crc.testing
  192.168.130.11 console-openshift-console.apps-crc.testing
  ```

**SSL Certificate errors**

- OpenShift Local uses self-signed certificates
- Click **Advanced** → **Proceed** (Chrome)
- Click **Advanced** → **Accept the Risk** (Firefox)
- This is normal for local development

**Can't log in to the console**

- **For developer user**: Username `developer`, Password `developer`
- **For admin**: Get credentials with `crc console --credentials`
- Verify cluster is fully started: `crc status` should show "Running"
- Wait 2-3 minutes after `crc start` completes for full initialization

### CLI Issues

**"oc command not found"**

- Run: `eval $(crc oc-env)`
- Or add to your shell profile permanently
- Verify with: `which oc`
- Alternatively, download oc separately from mirror site

**"Unable to connect to server"**

- Check cluster is running: `crc status`
- Verify API URL: should be `https://api.crc.testing:6443`
- Ensure you're logged in: `oc login -u developer -p developer https://api.crc.testing:6443`
- Check kubeconfig: `echo $KUBECONFIG` or `~/.kube/config`

### Project Creation Issues

**"Can't create a project"**

- Ensure you're logged in as `developer` or `kubeadmin`
- The `developer` user has project creation permissions by default
- Check current user: `oc whoami`
- Verify permissions: `oc auth can-i create projects`

**"Project name already exists"**

- Project names must be unique across the cluster
- Choose a different name
- View existing projects: `oc get projects`

### Performance Issues

**Cluster is slow**

- CRC needs at least 9 GB RAM and 4 CPUs
- Stop unnecessary applications on your host
- Check resource usage: `crc status`
- Consider increasing CRC resources:

  ```bash
  crc delete
  crc config set cpus 6
  crc config set memory 12288
  crc start
  ```

**System freezes when running CRC**

- Your system may not have enough resources
- Stop CRC when not in use: `crc stop`
- Consider using a cloud-hosted OpenShift instance instead

---

## Next Steps

Congratulations! You've successfully set up OpenShift Local and created your first project. You're now ready to start deploying applications.

Continue to [Lab 001: Verify Cluster](../001-verify-cluster/README.md) to learn more about cluster health monitoring and status verification.

---

## Estimated Time

**With Installation**: 60-90 minutes (including download time)

**Without Installation** (existing cluster): 20-30 minutes

---

## Quick Reference Commands

```bash
# CRC Management
crc start                          # Start OpenShift Local
crc stop                           # Stop the cluster
crc status                         # Check cluster status
crc delete                         # Delete the cluster
crc console                        # Open web console
crc console --credentials          # Show login credentials

# oc CLI Setup
eval $(crc oc-env)                 # Add oc to PATH
oc version                         # Verify oc installation

# Login
oc login -u developer -p developer https://api.crc.testing:6443
oc login -u kubeadmin -p <pass> https://api.crc.testing:6443

# Cluster Info
oc whoami                          # Current user
oc whoami --show-console           # Get console URL
oc cluster-info                    # Cluster information
oc get nodes                       # List nodes
oc get projects                    # List projects

# Project Management
oc new-project my-project          # Create project via CLI
oc project my-project              # Switch to project
oc delete project my-project       # Delete project
```
