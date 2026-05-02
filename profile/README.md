# apps-deployer

Платформа для автоматического деплоя stateless-приложений в Kubernetes — без знания DevOps и CI/CD.

Разработчик подключает GitHub-репозиторий, указывает фреймворк и переменные окружения, а платформа берёт на себя сборку образа, публикацию в реестр и деплой в кластер.

## Репозитории

| Репозиторий | Описание |
|---|---|
| [api-gateway](../../../../api-gateway) | Python · единая REST-точка входа для frontend, auth, projects и deployments |
| [webhook-service](../../../../webhook-service) | Python · обработчик GitHub App webhooks |
| [projects-service](../../../../projects-service) | Go · gRPC-сервис управления проектами, окружениями и конфигурациями деплоя |
| [deployments-service](../../../../deployments-service) | Python · REST API и Celery-воркеры для сборки образов и деплоя в k8s |
| [auth-service](../../../../auth-service) | Python · GitHub OAuth, выдача JWT |
| [frontend](../../../../frontend) | React/TypeScript · SPA для управления проектами и мониторинга деплоев |
| [infra](../../../../infra) | Terraform · облачная инфраструктура в Yandex Cloud (VPC, k8s, DNS, registry) |
| [infra-k8s](../../../../infra-k8s) | Helmfile · системные компоненты кластера (Traefik, PostgreSQL, Redis) |

## Стек

**Backend:** Go, Python (FastAPI, Celery), gRPC, PostgreSQL, Redis

**Frontend:** React, TypeScript, Vite, Tailwind CSS

**Инфраструктура:** Yandex Cloud, Terraform, Kubernetes, Helm, Docker

## Как это работает

```
GitHub push → webhook-service → api-gateway
                                      ↓
                           projects-service (repo+branch → env)
                                      ↓
                           deployments-service → Celery build/deploy workers
                                      ↓
                           Kaniko build + Kubernetes Deployment/Service/Ingress
```
