# Module 02 — Running GPU-Accelerated Workloads

### Brief Overview

This module guides participants through Red Hat OpenShift AI to launch a GPU-accelerated Jupyter workbench and execute a notebook that confirms GPU availability and runs a compute workload on it. Building on the operator setup completed in Module 1, participants create a Data Science Project, configure a workbench with a GPU accelerator, and verify that CUDA is accessible from Python before running a sample matrix operation. The module demonstrates the end-to-end path from OpenShift infrastructure to a data scientist's working environment.

### Audience and Time

- **Target personas:** Solutions architects, sales engineers, technical account managers
- **Prerequisites for this module:** Completion of Module 1 (GPU Operator and NFD installed, GPU worker node labeled and ready)
- **Estimated duration:** 30 minutes

### Learning Objectives

- Access the Red Hat OpenShift AI dashboard from the OpenShift console application launcher
- Create a Data Science Project to provide a namespace-scoped workspace for AI workloads
- Launch a Jupyter workbench configured with an NVIDIA GPU accelerator
- Run a Jupyter notebook that confirms CUDA availability and executes a GPU-accelerated computation

### Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Access OpenShift AI and create a Data Science Project | 5 min |
| 2 | Create a GPU-enabled workbench | 10 min |
| 3 | Run the GPU-accelerated notebook | 15 min |

### Detailed Steps

1. From the OpenShift console, click the **application launcher** (grid icon, upper-right corner) and select **Red Hat OpenShift AI** to open the OpenShift AI dashboard in a new browser tab.
2. In the OpenShift AI dashboard left-hand menu, click **Data Science Projects**, then click **Create data science project**.
3. Enter a project name (e.g., `gpu-workload-demo`) and click **Create**. OpenShift AI creates a corresponding OpenShift namespace.
4. Inside the new project, click **Create workbench**.
5. Enter a workbench name (e.g., `gpu-notebook`).
6. Under **Notebook image**, select an image that supports GPU workloads, such as **PyTorch** or **Standard Data Science**.
7. Under **Deployment size**, select **Small** (sufficient for this exercise).
8. Under **Accelerator**, open the dropdown and select **NVIDIA GPU**. Set the **Number of GPUs** to `1`.
9. Leave storage settings at their defaults and click **Create workbench**.
10. Wait on the workbench list page until the workbench status changes to **Running** (approximately 1–3 minutes while the pod is scheduled onto the GPU node).
11. Click **Open** next to the running workbench. The Jupyter notebook interface opens in a new browser tab.
12. In the Jupyter file browser, open the pre-loaded notebook file `gpu-test.ipynb`.
13. Run the first cell. It imports PyTorch and prints the result of `torch.cuda.is_available()`. Confirm the output is `True`, indicating the notebook environment has access to the GPU.
14. Run the second cell. It performs a matrix multiplication on the GPU and prints elapsed time. Observe that the operation completes successfully and that the device reported is `cuda:0`.
15. Return to the OpenShift AI dashboard, navigate to the workbench entry, and note the GPU memory utilization indicator confirming the GPU was used during notebook execution.

### Key Takeaways

- Red Hat OpenShift AI's workbench interface allows a data scientist to request a GPU accelerator through a form — no YAML authoring or node-selector configuration is required.
- OpenShift AI relies on the node labels applied by NFD and the device plugin deployed by the NVIDIA GPU Operator (both from Module 1) to schedule the workbench pod onto the correct node and to expose the GPU device inside the container.
- `torch.cuda.is_available()` returning `True` within the notebook confirms the full stack — cloud instance, GPU Operator, device plugin, and OpenShift AI — is working end to end.
- This workflow (project > workbench > notebook) is repeatable and self-service: a data scientist does not need cluster-admin access to create a project or launch a workbench.

### Infrastructure Notes

- The GPU worker node must have all GPU Operator daemon set pods in Running state before a workbench with a GPU accelerator can be scheduled; if the workbench pod stays Pending, check pod events and GPU Operator pod status in `gpu-operator-resources`.
- The PyTorch notebook image includes CUDA libraries compatible with the driver version deployed by the NVIDIA GPU Operator; no additional driver configuration is needed inside the workbench container.
- The pre-loaded notebook file `gpu-test.ipynb` must be present in the default workbench persistent volume or included in the notebook image used for this lab.
- OpenShift AI must be installed on the cluster (as a separate operator) before this module can be completed; confirm the OpenShift AI dashboard is reachable from the application launcher before participants begin Module 2.
