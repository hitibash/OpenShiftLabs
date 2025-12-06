# Lab 003: Creating Your First OpenShift Project

## Overview

In OpenShift, a **project** is a Kubernetes namespace with additional security and management features. If you're familiar with Kubernetes namespaces, think of projects as namespaces plus RBAC (Role Based Access Control) policies, network isolation, and resource tracking. This lab introduces you to creating and managing projects in OpenShift.

## Learning Objectives

By completing this lab, you will:

- Understand how OpenShift projects extend Kubernetes namespaces
- Create a project using the OpenShift web console
- Navigate between projects and understand project context
- Explore project settings and access controls

## Prerequisites

- Access to an OpenShift cluster (Developer Sandbox or self-hosted)
- Familiarity with Kubernetes concepts (namespaces, pods, deployments)
- Basic understanding of Docker and containerization
- Web browser to access the OpenShift console

---

## Background: Projects vs Namespaces

**What you know from Kubernetes:**
- Namespaces provide logical isolation for resources
- Resources are scoped to namespaces
- RBAC policies can be applied per namespace

**OpenShift Projects add:**
- User-friendly management through the console
- Automatic RBAC policy creation
- Network isolation by default (NetworkPolicies)
- Resource quotas and limits (can be applied)
- Project-level annotations and metadata

**Key point**: A project IS a namespace with extra features. When you create a project named `my-project`, you get a namespace called `my-project` plus additional OpenShift-specific resources.

---

## Lab Instructions

### Step 1: Access the OpenShift Web Console

1. Navigate to your OpenShift console URL in a web browser
2. Log in with your credentials
3. You'll land on the cluster overview dashboard

**Note**: Unlike vanilla Kubernetes, OpenShift provides a built-in web console for all cluster interactions.

---

### Step 2: Switch to Developer Perspective

OpenShift console has two perspectives:
- **Administrator**: Cluster-wide operations (similar to `kubectl` with cluster-admin)
- **Developer**: Application-focused view (project-scoped operations)

**Switch to Developer:**
1. Click the perspective switcher in the top-left corner
2. Select **Developer**

This view is optimized for deploying and managing applications within your projects.

---

### Step 3: Create Your First Project

1. Look for the **Project** dropdown near the top of the console
2. Click it and select **Create Project**

3. Fill in the project details:
   - **Name**: `lab-003-demo`
     - Must be lowercase, alphanumeric with hyphens
     - This becomes your Kubernetes namespace name
   
   - **Display Name**: `Lab 003 Demo Project`
     - User-friendly name (can have spaces and capitals)
   
   - **Description**: `Learning OpenShift projects and namespaces`
     - Helps document the project's purpose

4. Click **Create**

**What just happened:**
- OpenShift created a Kubernetes namespace: `lab-003-demo`
- Created RBAC policies granting you admin access
- Set up default network policies
- Created project metadata and tracking resources

---

### Step 4: Verify Project Resources

While still in the web console, let's see what OpenShift created:

1. Click **Project** in the left navigation
2. View the **Details** tab:
   - Status: Active
   - Name: `lab-003-demo` (the namespace)
   - Labels and annotations

3. Click the **Project Access** tab:
   - You'll see your user has `admin` role in this project
   - Similar to having namespace-admin RBAC in Kubernetes

---

## What's Next?

Now that you have a project (namespace), you're ready to deploy workloads! In the upcoming labs, you'll:
- Deploy container images (like `docker run`)
- Create Kubernetes Deployments and Services
- Build images from source using OpenShift BuildConfigs
- Expose services externally with Routes

Continue to [Lab 004: Deploying Your First Application](../004-docker-lifecycle/README.md) where you'll deploy a container image to your new project.

