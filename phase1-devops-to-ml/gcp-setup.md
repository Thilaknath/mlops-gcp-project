# GCP Setup for MLOps Learning Project

## Prerequisites

1. **GCP Account**: Ensure you have a GCP account with the $300 free credit
2. **gcloud CLI**: Install and configure gcloud CLI
3. **kubectl**: Install kubectl for Kubernetes management
4. **Docker**: Install Docker for container operations

## Step 1: Enable Required APIs

```bash
# Enable required GCP APIs
gcloud services enable container.googleapis.com
gcloud services enable compute.googleapis.com
gcloud services enable storage.googleapis.com
gcloud services enable monitoring.googleapis.com
gcloud services enable logging.googleapis.com
```

## Step 2: Set Project Variables

```bash
# Set your project ID (replace with your actual project ID)
export PROJECT_ID="your-mlops-project-id"
export REGION="us-central1"
export ZONE="us-central1-a"
export CLUSTER_NAME="mlops-cluster"

# Set the project
gcloud config set project $PROJECT_ID
gcloud config set compute/region $REGION
gcloud config set compute/zone $ZONE
```

## Step 3: Create GKE Cluster with GPU Support

We'll create a cost-optimized cluster that can scale to zero when not in use:

```bash
# Create the GKE cluster with autoscaling
gcloud container clusters create $CLUSTER_NAME \
    --zone=$ZONE \
    --machine-type=e2-standard-4 \
    --num-nodes=1 \
    --enable-autoscaling \
    --min-nodes=0 \
    --max-nodes=3 \
    --enable-autorepair \
    --enable-autoupgrade \
    --disk-size=50GB \
    --disk-type=pd-standard \
    --preemptible \
    --enable-ip-alias \
    --enable-network-policy
```

## Step 4: Add GPU Node Pool (When Needed)

For ML workloads requiring GPUs, we'll add a separate node pool:

```bash
# Add GPU node pool (use only when needed for training)
gcloud container node-pools create gpu-pool \
    --cluster=$CLUSTER_NAME \
    --zone=$ZONE \
    --machine-type=n1-standard-4 \
    --accelerator=type=nvidia-tesla-t4,count=1 \
    --num-nodes=0 \
    --enable-autoscaling \
    --min-nodes=0 \
    --max-nodes=2 \
    --preemptible \
    --disk-size=100GB \
    --disk-type=pd-ssd
```

## Step 5: Configure kubectl

```bash
# Get cluster credentials
gcloud container clusters get-credentials $CLUSTER_NAME --zone=$ZONE

# Verify connection
kubectl get nodes
```

## Cost Optimization Tips

1. **Use Preemptible Instances**: Up to 80% cost savings
2. **Auto-scaling to Zero**: Cluster scales down when not in use
3. **Spot GPU Instances**: Use when available for training workloads
4. **Resource Quotas**: Set limits to prevent accidental overspend
5. **Billing Alerts**: Set up alerts at $50, $100, $200 thresholds

## Cleanup Commands

When you're done with a phase or want to save costs:

```bash
# Scale down all deployments
kubectl scale deployment --all --replicas=0

# Delete GPU node pool when not needed
gcloud container node-pools delete gpu-pool --cluster=$CLUSTER_NAME --zone=$ZONE

# Delete entire cluster (when completely done)
gcloud container clusters delete $CLUSTER_NAME --zone=$ZONE
```

## Next Steps

1. Install NVIDIA GPU Operator
2. Install KubeRay Operator
3. Set up monitoring with Prometheus and Grafana
4. Deploy your first Ray cluster
