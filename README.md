# DevOps diploma practice in Yandex Cloud

## Project repositories

Terraform infrastructure:

https://github.com/neD555/diploma-infra

Test application with Dockerfile and CI/CD:

https://github.com/neD555/diploma-app

Kubernetes manifests and monitoring notes:

https://github.com/neD555/diploma-k8ss

## Deployed services

Test application:

http://158.160.176.150

Grafana:

http://158.160.172.142

Grafana login:

admin

Grafana password command:

kubectl --namespace monitoring get secrets monitoring-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo

## Docker image

Yandex Container Registry image:

cr.yandex/crprddkr87q49dkd6bl2/diploma-app:v1.0.0

## Infrastructure

The infrastructure was created with Terraform in Yandex Cloud.

Created resources:

- Terraform backend in Yandex Object Storage.
- VPC network.
- Subnets in multiple availability zones.
- Yandex Container Registry.
- Yandex Managed Kubernetes cluster.
- Node group with 3 worker nodes.
- LoadBalancer for the test application.
- Prometheus, Grafana, Alertmanager via kube-prometheus-stack.

## CI/CD

Application CI/CD:

- Repository: https://github.com/neD555/diploma-app
- GitHub Actions builds Docker image on push to main.
- GitHub Actions builds and deploys image on tag v1.0.0.
- Deployed image: cr.yandex/crprddkr87q49dkd6bl2/diploma-app:v1.0.0

Terraform CI/CD:

- Repository: https://github.com/neD555/diploma-infra
- GitHub Actions runs terraform init, fmt, validate and plan.

## Kubernetes checks

Useful commands:

kubectl get nodes

kubectl get pods --all-namespaces

kubectl get svc

kubectl get svc -n monitoring

## Terraform checks

Main Terraform directory:

cd ~/diploma-infra/terraform

Terraform output:

terraform output

Terraform init with remote backend:

terraform init \
  -backend-config="access_key=$(cat ~/.terraform-yc/backend-access-key)" \
  -backend-config="secret_key=$(cat ~/.terraform-yc/backend-secret-key)"

## Screenshots for review

Recommended screenshots:

1. Grafana Kubernetes dashboard.
2. Application CI pipeline on main branch.
3. Application CD pipeline on tag v1.0.0.
4. Terraform GitHub Actions pipeline.
5. Yandex Container Registry image list.
6. kubectl get nodes.
7. kubectl get pods --all-namespaces.
8. kubectl get svc and kubectl get svc -n monitoring.
9. terraform output.
10. Test application opened in browser or checked with curl.

## Notes

Ansible repository is not provided because the Kubernetes cluster was created with Yandex Managed Service for Kubernetes.
