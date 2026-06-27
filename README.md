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
<img width="1530" height="820" alt="grafana-dashboard" src="https://github.com/user-attachments/assets/53e514b2-e01d-4429-bb57-cfadc1d0632e" />
2. GitHub Actions pipeline приложения при push в main.
<img width="1916" height="754" alt="app-ci-main" src="https://github.com/user-attachments/assets/313cfffa-8efd-4eb0-9d71-0a308daa85c1" />
3. GitHub Actions pipeline приложения при создании тега v1.0.0.
<img width="1914" height="577" alt="app-cd-tag" src="https://github.com/user-attachments/assets/80e0ac03-c4c7-4056-8b32-6e7f50f79877" />
4. GitHub Actions pipeline Terraform.
<img width="1914" height="720" alt="terraform-pipeline" src="https://github.com/user-attachments/assets/3d89115c-db12-4ad9-8202-539386cee70d" />
5. Список Docker images в Yandex Container Registry.
<img width="668" height="498" alt="container-registry" src="https://github.com/user-attachments/assets/3aa2e6b5-d9b3-489e-a61d-f539d32f3e87" />
6. Вывод kubectl get nodes.
<img width="668" height="562" alt="kubernetes-nodes-services" src="https://github.com/user-attachments/assets/af7c63b6-b581-45f7-b7f2-f6b4a0f0bfd3" />
7. Вывод kubectl get pods --all-namespaces.
<img width="671" height="954" alt="kubernetes-pods" src="https://github.com/user-attachments/assets/b32ef756-d7b3-4445-9c6f-3b75b829cc43" />
8. Вывод kubectl get svc и kubectl get svc -n monitoring.
<img width="677" height="266" alt="terraform-output" src="https://github.com/user-attachments/assets/4979a16b-7808-43b0-a67f-c8e18b5fda4d" />
9. Вывод terraform output.
<img width="670" height="362" alt="terraform-init" src="https://github.com/user-attachments/assets/d6657ee4-880a-477a-a525-ba463b7aa180" />
10. Проверка HTTP доступа к тестовому приложению.
<img width="672" height="761" alt="application-curl" src="https://github.com/user-attachments/assets/d0d51f8b-ec8c-4b35-b73e-570c65b33791" />

## Примечание

Репозиторий Ansible не предоставляется, так как Kubernetes кластер создан через Yandex Managed Service for Kubernetes.
