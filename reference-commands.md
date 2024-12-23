# Kubernetes Autoscaling Command Reference Guide

## Table of Contents
- [Core Operations](#core-operations)
- [Deployment Management](#deployment-management)
- [HPA Operations](#hpa-operations)
- [Monitoring and Debugging](#monitoring-and-debugging)
- [Advanced Configuration](#advanced-configuration)

## Core Operations

### Initial Setup and Verification
```bash
# Verify cluster access and configuration
kubectl cluster-info
kubectl get nodes

# Verify metrics-server installation
kubectl get deployment metrics-server -n kube-system
```

### Deployment Creation and Management
```bash
# Create the NGINX deployment
kubectl apply -f nginx-deployment.yaml

# Verify deployment status
kubectl get deployment nginx
kubectl describe deployment nginx

# Check pod status
kubectl get pods -l app=nginx
```

## HPA Operations

### Basic HPA Management
```bash
# Create HPA using command line
kubectl autoscale deployment nginx \
    --cpu-percent=80 \
    --min=3 \
    --max=10

# Create HPA using configuration file
kubectl apply -f nginx-hpa.yaml

# View HPA status
kubectl get hpa nginx
kubectl describe hpa nginx

# Monitor HPA metrics
kubectl get hpa nginx --watch
```

### Advanced HPA Configuration
```bash
# Update existing HPA
kubectl patch hpa nginx --patch \
    '{"spec":{"metrics":[{"resource":{"name":"cpu","target":{"averageUtilization":70,"type":"Utilization"}},"type":"Resource"}]}}'

# Delete and recreate HPA
kubectl delete hpa nginx
kubectl apply -f nginx-hpa.yaml
```

## Monitoring and Debugging

### Resource Utilization
```bash
# Monitor pod resource usage
kubectl top pods
kubectl top pods -l app=nginx

# Monitor node resource usage
kubectl top nodes

# Get detailed pod metrics
kubectl describe pod <pod-name>
```

### Logs and Events
```bash
# View HPA events
kubectl describe hpa nginx | grep -A 10 Events:

# View deployment events
kubectl describe deployment nginx | grep -A 10 Events:

# View pod logs
kubectl logs -l app=nginx --tail=100
```

## Advanced Configuration

### Resource Management
```bash
# Update deployment resources
kubectl set resources deployment nginx \
    --requests=cpu=50m,memory=64Mi \
    --limits=cpu=100m,memory=128Mi

# View current resource settings
kubectl get deployment nginx -o jsonpath='{.spec.template.spec.containers[0].resources}'
```

### Scaling Operations
```bash
# Manual scaling (if needed)
kubectl scale deployment nginx --replicas=5

# Check scaling status
kubectl rollout status deployment/nginx

# View replica set status
kubectl get rs -l app=nginx
```

## Cleanup Operations
```bash
# Remove HPA
kubectl delete hpa nginx

# Remove deployment
kubectl delete deployment nginx

# Verify cleanup
kubectl get all | grep nginx
```

## Learning Notes

1. Always verify metrics-server is running before configuring HPA
2. Monitor both CPU and memory metrics for optimal scaling
3. Use declarative YAML files for better version control
4. Implement gradual scaling to prevent resource spikes
5. Regular monitoring of HPA events helps in troubleshooting

---

> 💡 **Best Practice**: Always set appropriate resource requests and limits to ensure effective autoscaling.

> ⚠️ **Warning**: Removing HPA will not automatically scale down the deployment to its original replica count.

> 📝 **Note**: The metrics-server requires a few minutes to collect accurate resource utilization data.