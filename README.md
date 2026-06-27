# Дипломный практикум DevOps в Yandex Cloud

## Репозитории проекта

Terraform инфраструктура:

https://github.com/neD555/diploma-infra

Тестовое приложение с Dockerfile и CI/CD:

https://github.com/neD555/diploma-app

Kubernetes манифесты и описание мониторинга:

https://github.com/neD555/diploma-k8ss

## Развернутые сервисы

Тестовое приложение:

http://158.160.176.150

Grafana:

http://158.160.172.142

Логин Grafana:

admin

Команда для получения пароля Grafana:

kubectl --namespace monitoring get secrets monitoring-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo

## Docker образ

Образ в Yandex Container Registry:

cr.yandex/crprddkr87q49dkd6bl2/diploma-app:v1.0.0

## Инфраструктура

Инфраструктура создана в Yandex Cloud с помощью Terraform.

Созданные ресурсы:

- Terraform backend в Yandex Object Storage.
- VPC сеть.
- Подсети в нескольких зонах доступности.
- Yandex Container Registry.
- Yandex Managed Kubernetes cluster.
- Группа worker nodes из 3 узлов.
- LoadBalancer для тестового приложения.
- Prometheus, Grafana, Alertmanager через kube-prometheus-stack.

## CI/CD

CI/CD тестового приложения:

- Репозиторий: https://github.com/neD555/diploma-app
- GitHub Actions собирает Docker image при push в main.
- GitHub Actions собирает и деплоит Docker image при создании тега v1.0.0.
- Развернутый образ: cr.yandex/crprddkr87q49dkd6bl2/diploma-app:v1.0.0

CI/CD Terraform:

- Репозиторий: https://github.com/neD555/diploma-infra
- GitHub Actions выполняет terraform init, fmt, validate и plan.

## Проверка Kubernetes

Команды для проверки:

kubectl get nodes

kubectl get pods --all-namespaces

kubectl get svc

kubectl get svc -n monitoring

## Проверка Terraform

Основная директория Terraform:

cd ~/diploma-infra/terraform

Вывод Terraform output:

terraform output

Инициализация Terraform с remote backend:

terraform init \
  -backend-config="access_key=$(cat ~/.terraform-yc/backend-access-key)" \
  -backend-config="secret_key=$(cat ~/.terraform-yc/backend-secret-key)"

## Скриншоты для проверки

К работе приложены скриншоты:

1. Grafana dashboard с метриками Kubernetes.
2. GitHub Actions pipeline приложения при push в main.
3. GitHub Actions pipeline приложения при создании тега v1.0.0.
4. GitHub Actions pipeline Terraform.
5. Список Docker images в Yandex Container Registry.
6. Вывод kubectl get nodes.
7. Вывод kubectl get pods --all-namespaces.
8. Вывод kubectl get svc и kubectl get svc -n monitoring.
9. Вывод terraform output.
10. Проверка HTTP доступа к тестовому приложению.

## Примечание

Репозиторий Ansible не предоставляется, так как Kubernetes кластер создан через Yandex Managed Service for Kubernetes.
