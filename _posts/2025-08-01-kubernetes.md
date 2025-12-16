---
layout: single
title: "[k8s] ingress/service/deployment 삼각관계"
date: 2025-08-02 20:21:02 +0900
categories: Kubernetes
share: true
---

사용자가 Kubernetes에 배포한 웹으로 접속하는 경우를 예로 들면  
ex. http://chatgpt.mycompany.com

<img src="/assets/images/kubernetes_flow_diagram.svg" alt="다이어그램" width="400">

### Ingress & Ingress Controller

ingress가 해당 요청의 호스트명(Host)\*\*을 기준으로 정의된 규칙에 따라 해석하고 라우팅을 수행한다.

클러스터에서 Ingress 리소스가 실제로 동작하려면, 반드시 Ingress Controller가 실행 중이어야 한다.  
Ingress는 단순히 트래픽 라우팅 규칙을 정의한 객체이고,  
Ingress Controller가 실제로 그 규칙에 따라 요청을 수신하고 라우팅하는 역할을 한다.

### Service

Kubernetes에서는 Service를 사용해 Pod 집합을 추상화하고,  
안정적으로 접근할 수 있도록 지원한다.

### Deployment

n개의 pod가 실행되길 원한다면, Deployment를 사용하면 된다.

대신 어떤 순간에는 몇 개의 Pod가 동작 중인지,  
어떤 Pod가 정상 상태인지조차 확실히 알 수 없는 상황이 발생할 수 있다.

### Example

사내 ChatGPT를 서비스 한다면 아래와 같이 예를 들 수 있다.  
(아래) ingress.yaml

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-internal
spec:
  rules:
  - host: chatgpt.mycompany.com
  http:
    paths:
    - path: /
      pathType: Prefix
      backend:
        service:
        name: chatgpt-service
        port:
          number: 80
```

(아래) service.yaml

```
apiVersion: v1
kind: Service
metadata:
name: chatgpt-service
  spec:
    selector:
      app.kubernetes.io/name: chatgptapp
    ports:
    - protocol: TCP
        port: 80
        targetPort: 9376
```

(아래) Deployment.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: chatgpt-deployment
  labels:
    app: chatgptapp
spec:
  replicas: 2  # 실행할 파드 개수
  selector:
    matchLabels:
      app: chatgptapp
  template:
    metadata:
      labels:
        app: chatgptapp
    spec:
      containers:
        - name: chatgpt
          image: chatgpt:latest
          ports:
            - containerPort: 80
```
