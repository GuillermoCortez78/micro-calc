Micro-Calc — Despliegue Cloud Native
Descripción

Proyecto académico que implementa el ciclo básico de construcción y despliegue de un microservicio Spring Boot utilizando Docker, Docker Hub, Kubernetes y Minikube.

El proyecto parte del microservicio micro-calc y permite validar el proceso completo desde la compilación de la aplicación hasta su ejecución dentro de un clúster Kubernetes local.

Objetivo

Aplicar conceptos fundamentales de arquitectura Cloud Native mediante:

Compilación de una aplicación Java 21 con Maven.
Construcción de una imagen Docker.
Publicación de la imagen en Docker Hub.
Configuración de recursos Kubernetes.
Despliegue sobre Minikube.
Validación del microservicio mediante HTTP.
Tecnologías
Tecnología	Propósito
Java 21	Desarrollo del microservicio
Spring Boot	Framework de aplicación
Maven	Compilación y empaquetado
Docker	Contenerización
Docker Hub	Registro de imágenes
Kubernetes	Orquestación
Minikube	Clúster Kubernetes local
Git	Control de versiones
Arquitectura
flowchart LR
    DEV["Código fuente<br/>Spring Boot"] --> MAVEN["Maven<br/>Build"]
    MAVEN --> DOCKER["Docker<br/>Image"]
    DOCKER --> HUB["Docker Hub<br/>micro-calc:v1"]

    HUB --> K8S["Minikube / Kubernetes"]

    subgraph K8S["Cluster Minikube"]
        CONFIG["ConfigMap"]
        DEPLOY["Deployment<br/>2 réplicas"]
        POD1["Pod<br/>micro-calc"]
        POD2["Pod<br/>micro-calc"]
        SERVICE["Service<br/>8090 → 8080"]

        CONFIG --> DEPLOY
        DEPLOY --> POD1
        DEPLOY --> POD2
        SERVICE --> POD1
        SERVICE --> POD2
    end

    CLIENT["Cliente<br/>Browser / PowerShell"] --> SERVICE
Flujo de despliegue
Código fuente
     ↓
Maven Build
     ↓
Docker Image
     ↓
Docker Hub
     ↓
Kubernetes Deployment
     ↓
Pods
     ↓
Service
     ↓
Cliente HTTP
Estructura principal
micro-calc/
├── src/
├── pom.xml
├── Dockerfile
└── k8s/
    ├── namespace.yaml
    ├── configmap.yaml
    ├── deployment.yaml
    ├── service.yaml
    └── hpa.yaml
Construcción
mvn clean package -DskipTests
Construcción Docker
docker build -t USUARIO_DOCKERHUB/micro-calc:v1 .

Ejecución local:

docker run -p 8080:8080 USUARIO_DOCKERHUB/micro-calc:v1
Publicación
docker push USUARIO_DOCKERHUB/micro-calc:v1
Despliegue Kubernetes

Iniciar Minikube:

minikube start --driver=docker --cpus=2 --memory=4096

Aplicar manifiestos:

kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

Validar:

kubectl get all -n micro-calc
Prueba

Exponer el servicio:

minikube service micro-calc-service -n micro-calc

Ejemplo de operación:

GET /suma/1/2
Resultado esperado

La solución permite demostrar el flujo:

Spring Boot → Docker → Docker Hub → Kubernetes → Minikube

con múltiples réplicas del microservicio gestionadas por Kubernetes y acceso mediante un Service.

Conclusión

El proyecto demuestra de forma práctica los fundamentos de contenerización y orquestación de aplicaciones Cloud Native, utilizando Docker para empaquetar el microservicio y Kubernetes con Minikube para administrar su despliegue y ejecución.
