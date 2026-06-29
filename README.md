[English](#english) | [Русский](#русский)

---

<a name="english"></a>
# k8s-deploy-lab

A demo FastAPI application with three deployment targets: raw Kubernetes manifests, Helm chart, and Docker Swarm. Prometheus metrics included out of the box.

## Application

FastAPI app exposing:

| Endpoint | Description |
|----------|-------------|
| `GET /` | App status |
| `GET /health` | Health check |
| `GET /ready` | Readiness check |
| `GET /metrics` | Prometheus metrics (request count + latency histogram) |

## Deployment Options

### Kubernetes — raw manifests

```bash
kubectl apply -f manifests/namespace.yaml
kubectl apply -f manifests/
```

### Kubernetes — Helm

```bash
helm install demo-app helm/app/ -f helm/app/values.yaml
```

### Docker Swarm

```bash
docker stack deploy -c swarm/docker-stack.yml demo
```

### Local

```bash
cd app
docker build -t demo-app .
docker run -p 8000:8000 demo-app
```

## Manifests

| File | Description |
|------|-------------|
| `namespace.yaml` | Kubernetes namespace |
| `deployment.yaml` | App Deployment |
| `service.yaml` | ClusterIP Service |
| `ingress.yaml` | Ingress |
| `configmap.yaml` | ConfigMap |
| `secret.yaml` | Secret template |

## Tech Stack

- Python 3 / FastAPI
- prometheus-client
- Kubernetes / Helm / Docker Swarm

---

<a name="русский"></a>
# k8s-deploy-lab

Демо FastAPI-приложение с тремя вариантами деплоя: raw Kubernetes-манифесты, Helm chart и Docker Swarm. Метрики Prometheus включены из коробки.

## Приложение

FastAPI-приложение с эндпоинтами:

| Эндпоинт | Описание |
|----------|----------|
| `GET /` | Статус приложения |
| `GET /health` | Проверка здоровья |
| `GET /ready` | Проверка готовности |
| `GET /metrics` | Метрики Prometheus (счётчик запросов + гистограмма задержек) |

## Варианты деплоя

### Kubernetes — raw манифесты

```bash
kubectl apply -f manifests/namespace.yaml
kubectl apply -f manifests/
```

### Kubernetes — Helm

```bash
helm install demo-app helm/app/ -f helm/app/values.yaml
```

### Docker Swarm

```bash
docker stack deploy -c swarm/docker-stack.yml demo
```

### Локально

```bash
cd app
docker build -t demo-app .
docker run -p 8000:8000 demo-app
```

## Манифесты

| Файл | Описание |
|------|----------|
| `namespace.yaml` | Kubernetes namespace |
| `deployment.yaml` | Deployment приложения |
| `service.yaml` | ClusterIP Service |
| `ingress.yaml` | Ingress |
| `configmap.yaml` | ConfigMap |
| `secret.yaml` | Шаблон секретов |

## Технологический стек

- Python 3 / FastAPI
- prometheus-client
- Kubernetes / Helm / Docker Swarm
