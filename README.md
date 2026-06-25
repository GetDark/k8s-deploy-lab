# k8s-deploy-lab

Kubernetes deployment lab: raw manifests + Helm chart + Docker Swarm stack for a FastAPI app with Prometheus metrics, liveness/readiness probes, rolling updates, and resource limits.

## Structure

```
k8s-deploy-lab/
├── app/                    # FastAPI app with /health, /ready, /metrics
│   ├── main.py
│   ├── requirements.txt
│   └── Dockerfile
├── manifests/              # Raw kubectl manifests
│   ├── namespace.yaml
│   ├── configmap.yaml
│   ├── secret.yaml         # ⚠ fill before applying
│   ├── deployment.yaml     # 2 replicas, rolling update, resource limits
│   ├── service.yaml        # ClusterIP
│   └── ingress.yaml        # nginx ingress + cert-manager TLS
├── helm/app/               # Helm chart (same stack, parametrized)
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
└── swarm/
    └── docker-stack.yml    # Docker Swarm overlay network
```

## Deploy with kubectl

```bash
# Apply in order
kubectl apply -f manifests/namespace.yaml
kubectl apply -f manifests/configmap.yaml
kubectl apply -f manifests/secret.yaml
kubectl apply -f manifests/deployment.yaml
kubectl apply -f manifests/service.yaml
kubectl apply -f manifests/ingress.yaml

# Check rollout
kubectl rollout status deployment/demo-app -n demo-app

# Rollback
kubectl rollout undo deployment/demo-app -n demo-app
```

## Deploy with Helm

```bash
helm upgrade --install demo-app ./helm/app \
  --namespace demo-app \
  --create-namespace \
  --set ingress.host=app.yourdomain.com \
  --set secrets.DATABASE_URL="postgresql://..." \
  --set secrets.SECRET_KEY="$(openssl rand -hex 32)"
```

## Deploy with Docker Swarm

```bash
# Init swarm (once)
docker swarm init

# Create secrets
echo "postgresql://..." | docker secret create database_url -
echo "$(openssl rand -hex 32)" | docker secret create secret_key -

# Deploy stack
docker stack deploy -c swarm/docker-stack.yml demo

# Check services
docker service ls
docker service ps demo_app
```

## Key concepts demonstrated

- **Namespace isolation** — dedicated namespace per app
- **ConfigMap / Secret separation** — non-sensitive vs sensitive config
- **Rolling update** — zero-downtime deploys (`maxUnavailable: 0`)
- **Liveness + readiness probes** — K8s knows when to send traffic
- **Resource requests/limits** — predictable scheduling
- **Prometheus annotations** — auto-scraping via `prometheus.io/*`
- **Helm templating** — same manifests, any environment
- **Swarm overlay network** — multi-host container networking
