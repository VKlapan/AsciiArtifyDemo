1. Створюємо кластер
   ```bash
   k3d cluster create argo
   ```
2. Налаштовуємо простір імен
   ```bash
   kubectl create namespace argocd
   ```
3. Інсталюємо ArgoCD
   ```bash
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```
4. Перевіряємо інсталяцію
   ```bash
   kubectl get all -n argocd
   ```
5. Отримаємо доступ до інтерфейсу ArgoCD GUI за допомогою Port Forwarding - за рахунок переадресації портів з локального порту 8080 на віддалений 443
   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:443&
   ```
6. Отримаємо пароль для логіну admin в Web-інтерфейс ArgoCD за адресою localhost:8080

![Alt text](https://monosnap.com/image/C9egsfmeZheVkO2mq7JZcX8IqR3KF7)

   ```bash
   kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"
   ```
   ```bash
   kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"|base64 -d;echo
   ```

DEMO

[![asciicast](https://asciinema.org/a/Xh9vtDaAXJ60MDZao60gm0WeB.svg)](https://asciinema.org/a/Xh9vtDaAXJ60MDZao60gm0WeB)
