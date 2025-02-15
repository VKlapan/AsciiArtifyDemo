# Порівняльний аналіз інструментів для розгортання Kubernetes кластерів в локальному середовищі

## Вступ

Для стартапу "AsciiArtify" необхідно обрати інструмент для розгортання локальних Kubernetes кластерів. У цьому порівняльному аналізі ми розглянемо три інструменти: minikube, kind та k3d. Ми також розглянемо можливість використання Podman як альтернативи Docker.

## Характеристики

| Інструмент | Підтримувані ОС та архітектури | Автоматизація | Додаткові функції |
|------------|-------------------------------|---------------|-------------------|
| Minikube   | Windows, macOS, Linux; amd64, arm64, ppc64le, s390x | Вбудована підтримка для створення скриптів та CI/CD інтеграцій | Вбудований dashboard для моніторингу |
| Kind       | Windows, macOS, Linux; amd64, arm64 | Легко інтегрується з CI/CD системами через Docker | Підтримка multi-node кластерів |
| K3d        | Windows, macOS, Linux; amd64, arm64 | Інтегрується з CI/CD системами через Docker | Висока швидкість створення кластерів, підтримка multi-node |

## Переваги та недоліки

| Інструмент | Переваги | Недоліки |
|------------|----------|----------|
| Minikube   | Легкість використання, добра документація, вбудований dashboard | Повільний запуск, обмежена масштабованість |
| Kind       | Легкість налаштування, швидкий запуск, підтримка multi-node | Залежність від Docker, складніше налаштування multi-node |
| K3d        | Швидкість запуску, підтримка multi-node, гнучкість налаштування | Залежність від Docker, складніша інтеграція з існуючими інструментами |

## Демонстрація

### Приклад розгортання застосунку "Hello World" на Kubernetes за допомогою k3d

1. Встановіть k3d:
    ```bash
    curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
    ```

2. Створіть кластер:
    ```bash
    k3d cluster create mycluster
    ```

3. Застосуйте конфігурацію (файл hello-world.yaml) для розгортання "Hello World" застосунку:
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: hello-world
    spec:
      containers:
        - name: hello
          image: tutum/hello-world
          ports:
            - containerPort: 80
          resources:
            limits:
              memory: "128Mi"
              cpu: "500m"
            requests:
              memory: "64Mi"
              cpu: "250m"
    ```

    ```bash
    kubectl apply -f hello-world.yaml
    ```

4. Перевірте статус:
    ```bash
    kubectl get pods
    ```

5. Перевірте доступність застосунку, "прокинувши" внутрішній порт 8080 на зовнішній порт 80:
    ```bash
    kubectl port-forward pod/hello-world 8080:80
    ```

    Відкрийте у браузері: [http://localhost:8080](http://localhost:8080)
    Мусите побачити таку сторінку: [https://monosnap.com/direct/EM9rMy9ItosRZ9GXS3wszwVsMjS2yC]

Демо використання K3D: [https://asciinema.org/a/NMc4iWgvg5euiQXEwA3Snw1rn]

[![asciicast](https://asciinema.org/a/NMc4iWgvg5euiQXEwA3Snw1rn.svg)](https://asciinema.org/a/NMc4iWgvg5euiQXEwA3Snw1rn)

## Висновки

| Інструмент | Рекомендації |
|------------|--------------|
| Minikube   | Рекомендується для простих PoC, де швидкість та масштабованість не є критичними |
| Kind       | Підходить для локального тестування та інтеграції з CI/CD, якщо не потрібно складних налаштувань |
| K3d        | Рекомендується для швидкого створення multi-node кластерів та складних PoC, але з обережністю щодо залежності від Docker |

## Заключення

На основі проведеного аналізу, для стартапу "AsciiArtify" рекомендується використовувати k3d для розгортання локальних Kubernetes кластерів. Він забезпечує високу швидкість та гнучкість налаштування, що важливо для швидкого розроблення та тестування PoC. Проте слід звернути увагу на залежність від Docker та можливі ризики з ліцензуванням. У цьому випадку варто розглянути можливість використання Podman як альтернативи.
