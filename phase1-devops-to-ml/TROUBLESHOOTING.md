# Phase 1 Troubleshooting Guide

## Issue: NVIDIA GPU Operator Installation Timeout

### Problem
```
Error: INSTALLATION FAILED: context deadline exceeded
```

### Root Cause
The GPU Operator is trying to install but timing out because:
1. No GPU nodes are available in the cluster yet
2. The operator is waiting for GPU nodes to become ready
3. Default timeout is too short for the installation

### Solution Options

#### Option 1: Install GPU Operator AFTER Adding GPU Nodes (Recommended)

**Step 1: First add GPU node pool**
```bash
# Add GPU node pool with at least 1 node initially
gcloud container node-pools create gpu-pool \
    --cluster=$CLUSTER_NAME \
    --zone=$ZONE \
    --machine-type=n1-standard-4 \
    --accelerator=type=nvidia-tesla-t4,count=1 \
    --num-nodes=1 \
    --enable-autoscaling \
    --min-nodes=0 \
    --max-nodes=2 \
    --preemptible \
    --disk-size=100GB \
    --disk-type=pd-ssd

# Wait for nodes to be ready
kubectl get nodes -w
```

**Step 2: Then install GPU Operator**
```bash
# Install with longer timeout
helm install gpu-operator nvidia/gpu-operator \
    --namespace gpu-operator \
    --create-namespace \
    --set driver.enabled=true \
    --set toolkit.enabled=true \
    --set devicePlugin.enabled=true \
    --timeout=20m \
    --wait
```

#### Option 2: Install Without Waiting (Alternative)

```bash
# Install without --wait flag
helm install gpu-operator nvidia/gpu-operator \
    --namespace gpu-operator \
    --create-namespace \
    --set driver.enabled=true \
    --set toolkit.enabled=true \
    --set devicePlugin.enabled=true

# Monitor installation progress
kubectl get pods -n gpu-operator -w
```

#### Option 3: Skip GPU Setup for Now (Cost-Effective)

Since GPU nodes are expensive, you can skip this step and continue with CPU-only setup:

```bash
# Skip GPU operator installation
echo "Skipping GPU setup to save costs"

# Continue with Ray cluster deployment (CPU-only)
kubectl apply -f phase1-devops-to-ml/ray-cluster/kuberay-operator.yaml
```

### Verification Commands

**Check if GPU nodes are ready:**
```bash
kubectl get nodes --show-labels | grep accelerator
```

**Check GPU operator pods:**
```bash
kubectl get pods -n gpu-operator
```

**Test GPU access (only after GPU nodes are ready):**
```bash
kubectl apply -f phase1-devops-to-ml/kubernetes/nvidia-gpu-operator.yaml
kubectl logs gpu-test
```

### Cost-Saving Recommendation

For learning purposes, I recommend **Option 3** initially:
1. Skip GPU setup for now
2. Complete Phase 1 with CPU-only Ray cluster
3. Add GPU support later when you need it for training workloads

This approach will:
- Save significant costs (GPU nodes are ~10x more expensive)
- Let you learn the core concepts first
- Allow you to add GPU support when actually needed

### Next Steps

Choose one of the options above and let me know:
1. Which option you'd like to proceed with
2. Any error messages you encounter
3. When you're ready to move to the next step

The core MLOps concepts can be learned effectively with CPU-only clusters initially!
