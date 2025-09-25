# Kubernetes Services Manifest Files

This directory contains various Kubernetes service manifest files demonstrating different service types and configurations.

## Service Types

### ClusterIP (Default)
- **File**: `web-app-service.yaml`, `database-service.yaml`, `api-services.yaml`
- **Purpose**: Internal cluster communication only
- **Use Case**: Backend services, databases, internal APIs

### NodePort
- **File**: `nodeport-service.yaml`
- **Purpose**: Exposes service on each node's IP at a static port
- **Use Case**: Development, testing, or when you need direct node access

### LoadBalancer
- **File**: `web-app-service.yaml`, `api-services.yaml`
- **Purpose**: Exposes service externally using cloud provider's load balancer
- **Use Case**: Production web applications, public APIs

### ExternalName
- **File**: `external-service.yaml`
- **Purpose**: Maps service to external DNS name
- **Use Case**: External databases, third-party APIs

### Headless Service
- **File**: `database-service.yaml`, `monitoring-services.yaml`
- **Purpose**: Direct pod-to-pod communication (clusterIP: None)
- **Use Case**: StatefulSets, databases requiring direct pod access

## Directory Structure

```
kubernetes/
├── services/
│   ├── web-app-service.yaml       # Frontend web application services
│   ├── database-service.yaml      # Database services (PostgreSQL, Redis)
│   ├── api-services.yaml          # API and microservices
│   ├── nodeport-service.yaml      # NodePort service examples
│   ├── external-service.yaml      # External services and endpoints
│   ├── ingress-controller-service.yaml  # NGINX ingress controller
│   └── monitoring-services.yaml   # Monitoring stack services
└── README.md
```

## Key Service Configuration Elements

### Basic Service Structure
```yaml
apiVersion: v1
kind: Service
metadata:
  name: service-name
  namespace: default
  labels:
    app: app-name
spec:
  type: ClusterIP|NodePort|LoadBalancer|ExternalName
  selector:
    app: app-name
  ports:
    - name: port-name
      port: 80
      targetPort: 8080
      protocol: TCP
```

### Common Annotations
- `service.beta.kubernetes.io/aws-load-balancer-type: "nlb"` - AWS Network Load Balancer
- `service.beta.kubernetes.io/aws-load-balancer-internal: "true"` - Internal load balancer
- `metallb.universe.tf/address-pool: production-public-ips` - MetalLB address pool

### Port Configuration
- **port**: The port that will be exposed by this service
- **targetPort**: The port on the pod that the service should proxy to
- **nodePort**: The port on each node (only for NodePort services)
- **protocol**: TCP or UDP (default is TCP)

## Usage Examples

### Apply all services
```bash
kubectl apply -f kubernetes/services/
```

### Apply specific service
```bash
kubectl apply -f kubernetes/services/web-app-service.yaml
```

### Get services
```bash
kubectl get services
kubectl get svc -n monitoring
```

### Describe service
```bash
kubectl describe service web-app-service
```

### Test service connectivity
```bash
kubectl port-forward service/web-app-service 8080:80
```

## Best Practices

1. **Use ClusterIP for internal services** - Default and most secure
2. **Use LoadBalancer for external-facing services** in production
3. **Use NodePort sparingly** - Mainly for development or edge cases
4. **Label consistently** - Use consistent labeling strategy
5. **Use namespaces** - Organize services by environment or application
6. **Define resource limits** - In corresponding deployments
7. **Use headless services** for StatefulSets requiring direct pod access
8. **Monitor service health** - Implement health checks and monitoring
