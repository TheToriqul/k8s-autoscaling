# Kubernetes Autoscaling Troubleshooting Guide

## Common Issues and Solutions

### 1. HPA Shows Unknown Metrics
**Problem:** HPA shows `<unknown>` for metrics
**Solution:**
- Verify metrics-server is running:
  ```bash
  kubectl get deployment metrics-server -n kube-system
  ```
- Check resource requests are defined in deployment
- Wait 1-2 minutes for metrics collection

### 2. Pods Not Scaling Up
**Problem:** Deployment doesn't scale despite high load
**Possible Solutions:**
1. Verify HPA configuration:
   ```bash
   kubectl describe hpa nginx
   ```
2. Check if max replicas limit is reached
3. Ensure cluster has available resources
4. Verify metrics are being collected properly

### 3. Resource Limits Issues
**Problem:** Pods being OOMKilled or CPU throttled
**Solution:**
- Adjust resource limits in deployment:
  - Increase memory limit above "128Mi"
  - Increase CPU limit above "100m"
- Monitor actual resource usage:
  ```bash
  kubectl top pods
  ```

## Debugging Commands
```bash
# Check HPA status
kubectl get hpa nginx -o yaml

# View HPA events
kubectl describe hpa nginx

# Check metrics availability
kubectl get --raw "/apis/metrics.k8s.io/v1beta1/pods"
```

## Best Practices
1. Always monitor HPA events during initial setup
2. Start with conservative scaling thresholds
3. Gradually adjust based on actual usage patterns
4. Keep resource requests and limits balanced