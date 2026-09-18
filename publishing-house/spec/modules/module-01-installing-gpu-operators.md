# Module 01 — Installing GPU Operators

### Brief Overview

This module walks participants through installing the two operators required to expose a GPU-capable worker node to Red Hat OpenShift: the Node Feature Discovery (NFD) Operator and the NVIDIA GPU Operator. Both are installed through OperatorHub in the OpenShift console. By the end of this module the GPU worker node is labeled, the GPU Operator daemon sets are running, and the node is ready to accept GPU-scheduled workloads.

### Audience and Time

- **Target personas:** Solutions architects, sales engineers, technical account managers
- **Prerequisites for this module:** Admin access to a running OpenShift 4.21 cluster; basic familiarity with OpenShift console navigation and the operator model
- **Estimated duration:** 30 minutes

### Learning Objectives

- Navigate OperatorHub and install the Node Feature Discovery Operator from Red Hat
- Create a NodeFeatureDiscovery custom resource to trigger hardware label discovery on cluster nodes
- Install the NVIDIA GPU Operator and create a ClusterPolicy custom resource to deploy the full GPU driver and plugin stack
- Confirm that the GPU worker node is labeled and that all GPU Operator daemon set pods reach a Running state

### Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Install the Node Feature Discovery Operator | 10 min |
| 2 | Install the NVIDIA GPU Operator | 10 min |
| 3 | Verify GPU node readiness | 10 min |

### Detailed Steps

1. Log in to the OpenShift console using the pre-provided admin credentials.
2. In the left-hand navigation menu, select **Operators > OperatorHub**.
3. In the OperatorHub search field, type `Node Feature Discovery`.
4. Select the **Node Feature Discovery** tile published by Red Hat (not the community version).
5. Click **Install**. Accept the default namespace (`openshift-nfd`) and click **Install** again.
6. Navigate to **Operators > Installed Operators** and wait until the NFD Operator status shows **Succeeded**.
7. Click the installed NFD Operator, then select the **NodeFeatureDiscovery** tab and click **Create NodeFeatureDiscovery**.
8. Accept the default CR configuration and click **Create**. This starts the NFD daemon set, which scans each node and applies hardware-capability labels.
9. Return to **Operators > OperatorHub** and search for `NVIDIA GPU Operator`.
10. Select the **NVIDIA GPU Operator** tile and click **Install**. Accept the default namespace (`gpu-operator-resources`) and click **Install** again.
11. In **Operators > Installed Operators**, wait until the NVIDIA GPU Operator status shows **Succeeded**.
12. Click the installed GPU Operator, select the **ClusterPolicy** tab, and click **Create ClusterPolicy**.
13. Accept the default ClusterPolicy configuration and click **Create**. The operator deploys a set of daemon sets for the NVIDIA driver, device plugin, and DCGM exporter.
14. Navigate to **Workloads > Pods**, filter by namespace `gpu-operator-resources`, and confirm that all pods reach a **Running** state (this may take 3–5 minutes while the driver daemon set initialises).
15. Navigate to **Compute > Nodes**, select the GPU-capable worker node, and click the **Labels** tab. Confirm that the label `nvidia.com/gpu.present=true` is present, confirming that NFD detected the GPU hardware.

### Key Takeaways

- The Node Feature Discovery Operator must be installed and a NodeFeatureDiscovery CR created before the NVIDIA GPU Operator can correctly identify GPU nodes.
- The NVIDIA GPU Operator manages the entire GPU software stack (driver, device plugin, monitoring) through a single ClusterPolicy CR — no manual driver installation is required on the node.
- Operator status transitions from **Installing** to **Succeeded** are visible in **Installed Operators** and indicate the control-plane components are healthy; daemon set pod readiness indicates worker-node readiness.
- Node labels added by NFD (e.g., `nvidia.com/gpu.present`, `nvidia.com/gpu.product`) are what allow the scheduler and OpenShift AI to target GPU nodes for workloads.

### Infrastructure Notes

- The GPU worker node (AWS g4dn or p3 family) is pre-provisioned with a GPU-capable instance type. Driver dependencies are pre-staged so participants start at the operator installation step.
- The GPU Operator daemon set pods will not reach Running until NFD has labeled the node; installation order (NFD first, then GPU Operator) matters.
- Namespace for NFD: `openshift-nfd`. Namespace for GPU Operator components: `gpu-operator-resources`.
- Expected total pod count in `gpu-operator-resources` after a healthy ClusterPolicy creation: varies by GPU Operator version; all pods should be Running or Completed with no CrashLoopBackOff entries.
