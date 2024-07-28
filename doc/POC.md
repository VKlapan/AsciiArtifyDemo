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
   ```bash
   kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"
   ```
   ```bash
   kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"|base64 -d;echo
   ```
