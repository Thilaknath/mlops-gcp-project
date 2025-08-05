# MLOps Learning Project on GCP

This project follows a structured approach to learn MLOps by building on existing DevOps skills and gradually introducing ML-specific concepts and tools.

## Project Structure

```
mlops-gcp-project/
├── phase1-devops-to-ml/          # Bridge DevOps skills to ML
│   ├── kubernetes/               # K8s configs for ML workloads
│   ├── monitoring/              # Prometheus & Grafana for ML metrics
│   └── ray-cluster/             # KubeRay setup
├── phase2-ml-foundations/        # Learn ML vocabulary and lifecycle
│   ├── models/                  # Sample models and experiments
│   └── frameworks/              # PyTorch, Hugging Face examples
├── phase3-ray-ecosystem/         # Master Ray for AI infrastructure
│   ├── ray-core/               # Basic Ray distributed computing
│   ├── ray-train/              # Distributed training
│   ├── ray-serve/              # Model serving
│   └── ray-tune/               # Hyperparameter tuning
├── phase4-advanced/              # Specialized tooling
│   ├── deepspeed/              # Memory optimization
│   ├── benchmarking/           # MLPerf and performance testing
│   └── containerization/       # GPU containers
└── sample-project/              # End-to-end implementation
    ├── fine-tuning/            # Model fine-tuning pipeline
    ├── serving/                # Model serving infrastructure
    └── monitoring/             # Complete monitoring stack
```

## Learning Path

### Phase 1: Bridge DevOps Skills to ML World ✅
- [x] Project setup
- [x] GKE cluster with GPU nodes
- [x] NVIDIA GPU Operator installation
- [x] KubeRay Operator setup
- [x] GPU monitoring with DCGM Exporter
- [x] Grafana dashboards for ML metrics
- [x] **Ready to execute! Follow `phase1-devops-to-ml/PHASE1-WALKTHROUGH.md`**

### Phase 2: ML Foundations
- [ ] Understanding Training vs Inference vs Fine-tuning
- [ ] Working with popular models (ResNet50, BERT, LLaMA)
- [ ] PyTorch and Hugging Face basics

### Phase 3: Ray Ecosystem Mastery
- [ ] Ray Core fundamentals
- [ ] Ray Train for distributed training
- [ ] Ray Serve for model inference
- [ ] Ray Tune for hyperparameter optimization
- [ ] Ray Autoscaler for dynamic scaling

### Phase 4: Advanced Tooling
- [ ] DeepSpeed integration
- [ ] MLPerf benchmarking
- [ ] Advanced containerization

### Sample Project: End-to-End ML Pipeline
- [ ] Fine-tune Mistral-7B on samsum dataset
- [ ] Deploy with Ray Serve
- [ ] Monitor with Prometheus/Grafana
- [ ] Test REST API endpoints

## GCP Services Used (Free Tier Focus)

- **Google Kubernetes Engine (GKE)**: Free tier includes $300 credit
- **Compute Engine**: For GPU nodes (will incur costs but can be minimized)
- **Cloud Storage**: 5GB free tier for model artifacts
- **Cloud Monitoring**: Free tier for basic metrics
- **Container Registry**: 0.5GB free storage

## Cost Optimization Strategy

1. Use preemptible/spot instances for training workloads
2. Auto-scale clusters to zero when not in use
3. Use smaller models for learning (e.g., DistilBERT instead of full BERT)
4. Clean up resources after each phase
5. Monitor costs with GCP billing alerts

## Getting Started

Follow the phases in order, starting with Phase 1. Each phase builds upon the previous one and introduces new concepts gradually.
