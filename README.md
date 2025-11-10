# Projeto - CI/CD - GitHub Actions - Manifesto

Este projeto demonstra a implementação completa de um pipeline **CI/CD automatizado** utilizando **Docker**, **GitHub Actions**, **Kubernetes** e **ArgoCD**.
O pipeline foi desenvolvido para a aplicação `hello-app` (em **FastAPI**) e um repositório de manifests Kubernetes (`hello-manifests`), possibilitando **build**, **deploy** e **atualizações contínuas** sem intervenção manual.


## Criação do repositório `hello-manifests`
_Quando o `hello-app` envia uma nova versão de imagem, o `deployment.yaml` é atualizado automaticamente com a nova tag.  
O ArgoCD detecta essa mudança e aplica o novo deploy._  
- Criar um repositório **público** que contém os manifests *Kubernetes* monitorados pelo *ArgoCD*  
- Criar uma pasta `hello-manifests` que contenha o *deployment* do hello-app no Kubernetes e a *exposição* do serviço (ClusterIP)  
- Crie `deployment.yaml` para fazer o deployment do hello-app no Kubernetes  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-app
  labels:
    app: hello-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-app
  template:
    metadata:
      labels:
        app: hello-app
    spec:
      containers:
        - name: hello-app
          image: SEU_NOME_DOCKER_HUB/hello-app:latest  
          ports:
            - containerPort: 8000
          readinessProbe:
            httpGet:
              path: /
              port: 8000
            initialDelaySeconds: 3
            periodSeconds: 5
```

• Crie `service.yaml` para fazer a exposição do serviço (ClusterIP)  
```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-app-svc
spec:
  selector:
    app: hello-app
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8000
  type: ClusterIP
```



A aplicação vem de outro repositório: <href> https://github.com/guilourenzo1/hello-app </href>
