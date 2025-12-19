WordPress on Kubernetes – Infra & DevOps Assignment (Syfe)
📌 Overview

This project demonstrates a production-grade WordPress deployment on Kubernetes using Docker, Helm, Nginx (OpenResty), Prometheus, and Grafana.

The goal of this assignment is to show:

Kubernetes fundamentals

Helm-based deployments

Reverse proxy configuration

Persistent storage

Monitoring and alerting

Practical DevOps troubleshooting

🏗 Architecture
Client
  |
  v
Nginx (OpenResty + Lua)
  |
  v
WordPress (Apache + PHP)
  |
  v
MySQL (StatefulSet + Persistent Storage)


Monitoring stack:

Prometheus → Alertmanager → Grafana

📁 Project Structure
wordpress-k8s-devops-syfe/
├── docker/
│   ├── nginx/
│   ├── wordpress/
│   └── mysql/
│
├── helm/
│   └── wordpress/
│       ├── Chart.yaml
│       └── templates/
│           ├── wordpress-deployment.yaml
│           ├── mysql-statefulset.yaml
│           ├── nginx-deployment.yaml
│           ├── services.yaml
│           ├── pv.yaml
│           └── pvc.yaml
│
├── monitoring/
│   └── prometheus/
│       └── alerts.yaml
│
└── README.md

🧰 Prerequisites

Docker Desktop (with Kubernetes enabled)

kubectl

Helm

Git

Windows PowerShell

🐳 Docker Image Build (Custom Images)
WordPress Image
cd docker/wordpress
docker build -t syfe-wordpress:1.0 .


Used for:
Running WordPress with Apache + PHP inside Kubernetes.

MySQL Image
cd docker/mysql
docker build -t syfe-mysql:1.0 .


Used for:
Persistent MySQL database (StatefulSet).

Nginx (OpenResty + Lua)
cd docker/nginx
docker build -t syfe-nginx-openresty:1.1 .


Used for:
Reverse proxy in front of WordPress.

☸ Kubernetes Cluster Verification
kubectl get nodes
kubectl get pods
kubectl get services


Used for:
Verifying cluster readiness.

📦 Helm Deployment (Main Application)
Install WordPress Stack
helm install my-release helm/wordpress

Upgrade (after changes)
helm upgrade my-release helm/wordpress

Verify Pods
kubectl get pods

Cleanup
helm delete my-release

✅ Application Verification (Correct Production Method)

Instead of browser-based verification, the app is validated inside the cluster.

kubectl exec -it deployment/nginx -- curl -I http://wordpress

Expected Output
HTTP/1.1 302 Found
Location: /wp-admin/install.php


This proves:

Nginx is running

WordPress is running

Apache & PHP are functional

MySQL connection works

Reverse proxy is correct

📊 Monitoring Setup (Prometheus & Grafana)
Add Helm Repository
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

Install Monitoring Stack
helm install monitoring prometheus-community/kube-prometheus-stack

Verify Monitoring Pods
kubectl get pods -l release=monitoring

🔐 Grafana Access
Get Admin Password (Windows PowerShell)
[System.Text.Encoding]::UTF8.GetString(
  [System.Convert]::FromBase64String(
    (kubectl get secret monitoring-grafana -o jsonpath="{.data.admin-password}")
  )
)

Port Forward Grafana
kubectl port-forward svc/monitoring-grafana 3000:80


Access:

http://localhost:3000

📈 Metrics Visualization
Dashboards Used

Kubernetes / Compute Resources / Pod

Prometheus / Overview

Metrics include:

Pod CPU utilization

Pod memory usage

Kubernetes resource health

Note: Metrics may appear minimal on Windows Docker Desktop due to cAdvisor and node-exporter limitations.

🚨 Alerting Setup & Verification
Check Loaded Alert Rules
kubectl get prometheusrules

Open Alertmanager UI
kubectl port-forward svc/monitoring-kube-prometheus-alertmanager 9093:9093


Access:

http://localhost:9093

Test Alert (Always Firing – Verification Only)
alert: TestAlwaysFiring
expr: vector(1)


Applied using:

kubectl apply -f test-alert.yaml


Verified in Alertmanager UI.

Cleanup:

kubectl delete -f test-alert.yaml

⚠ Known Limitations (Local Environment)

Running on Windows + Docker Desktop

node-exporter may crash

CPU graphs may appear empty

Port-forwarding may be unreliable

These are environment limitations, not application issues.

🧠 Production Improvements

In a real production environment:

nginx-prometheus-exporter for request & 5xx metrics

mysqld-exporter for MySQL metrics

Ingress controller

TLS & HPA

External alert receivers (Slack, Email, PagerDuty)

✅ Assignment Completion Summary

✔ Docker images created
✔ Kubernetes deployment
✔ Helm-based install/upgrade/delete
✔ Reverse proxy via Nginx
✔ Persistent storage
✔ Monitoring with Prometheus & Grafana
✔ Alerting with Alertmanager
✔ Proper verification & documentation

👤 Author

Abhinav Prakash
Infra & DevOps Intern Candidate
