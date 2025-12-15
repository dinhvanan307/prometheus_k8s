# 📊 Prometheus Monitoring on Kubernetes

Dự án triển khai hệ thống giám sát (Monitoring) và cảnh báo (Alerting) cho cụm Kubernetes sử dụng **Prometheus** và **Grafana**.

![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-orange?logo=prometheus)
![Kubernetes](https://img.shields.io/badge/Kubernetes-K8s-blue?logo=kubernetes)
![Grafana](https://img.shields.io/badge/Grafana-Visualization-F46800?logo=grafana)

---

## 🏗 Kiến trúc hệ thống
Hệ thống giám sát bao gồm các thành phần:
* **Prometheus Server:** Thu thập metrics từ các ứng dụng và Node trong Cluster.
* **AlertManager:** Quản lý và gửi cảnh báo (qua Email, Slack...).
* **Kube-state-metrics:** Expose các chỉ số của Kubernetes (Pod, Deployment status...).
* **Grafana (Optional):** Hiển thị dữ liệu dưới dạng biểu đồ trực quan.

---

## 🚀 Ứng dụng demo 

<img width="1238" height="660" alt="image" src="https://github.com/user-attachments/assets/8ff4d8b7-640a-41bd-80e1-ee2d458a1d09" />

<img width="335" height="131" alt="image" src="https://github.com/user-attachments/assets/6ce60390-abbf-4501-b123-7b23b7e527da" />

## Cài Prometheus + Grafana

1. Add repo Helm
- helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
- helm repo update
2. Tạo namespace cho monitoring
- kubectl create ns monitoring
3. Cài chart kube-prometheus-stack
- Cấu hình file values.yaml và chạy "helm install kps prometheus-community/kube-prometheus-stack -n monitoring -f values.yaml".

## Truy cập giao diện

1. Grafana
- Chạy "kubectl -n monitoring port-forward svc/kps-grafana 3000:80"

<img width="544" height="162" alt="image" src="https://github.com/user-attachments/assets/f943f40a-b0ad-46f6-8d4a-687ee28cde38" />


- Truy cập "http://localhost:3000" và login

User: admin

Pass: admin123

<img width="397" height="358" alt="image" src="https://github.com/user-attachments/assets/a38e3e51-6df0-45a7-8974-55156b382455" />

<img width="890" height="421" alt="image" src="https://github.com/user-attachments/assets/c77da193-c547-405d-8b9d-faa1431b9221" />


2. Prometheus
- Chạy "kubectl -n monitoring port-forward svc/kps-kube-prometheus-stack-prometheus 9090:9090" và truy cập "http://localhost:9090"

<img width="813" height="221" alt="image" src="https://github.com/user-attachments/assets/22b90fef-ac60-408a-a406-249190a3ec57" />


3. Alertmanager
- Chạy "kubectl -n monitoring port-forward svc/kps-kube-prometheus-stack-alertmanager 9093:9093" và truy cập "http://localhost:9093"

<img width="1919" height="982" alt="image" src="https://github.com/user-attachments/assets/bb386543-67f2-4d16-bf46-846b0a23af89" />

## Giám sát "Podinfo" (Service Monitor)
1. Tạo file servicemonitor-podinfo.yaml

<img width="608" height="365" alt="image" src="https://github.com/user-attachments/assets/5b93afd4-5d1b-4d99-8023-60ff634d7715" />


- Áp dụng và kiểm tra trong Prometheus

<img width="1919" height="987" alt="image" src="https://github.com/user-attachments/assets/6898867b-a338-4f29-8c32-76c98502f668" />

Thấy trạng thái "podinfo" là "UP".

2. Tạo file alert-pod-cpu.yaml

<img width="1919" height="1034" alt="image" src="https://github.com/user-attachments/assets/94c1b85f-4134-42a9-9686-e5544d737058" />

- Áp dụng và kiểm tra.

## Kiểm tra

<img width="1919" height="985" alt="image" src="https://github.com/user-attachments/assets/647d7d96-832f-4424-8fae-9e1e45b5f87c" />

1. Trực quan hóa trong Grafana

<img width="1919" height="985" alt="image" src="https://github.com/user-attachments/assets/0b13afa2-6209-494f-b18c-de0425da482e" />

2. Kiểm tra Alert
- Tạo một PrometheusRule để cảnh báo khi CPU usage của Pod > 80% trong 1 phút.
- Tạo cấu hình AlertManager để gửi thông báo và áp dụng cấu hình với lệnh "kubectl apply -f prometheus-alert-rules.yaml"
- Deploy pod tạo CPU stress để trigger alert "kubectl apply -f cpu-stress-test.yaml"

<img width="1919" height="980" alt="image" src="https://github.com/user-attachments/assets/db926ba9-8713-46a4-aede-3de37989c7b2" />
<img width="1919" height="984" alt="image" src="https://github.com/user-attachments/assets/a57580ec-83b8-4416-afc2-32e6352b2c66" />



