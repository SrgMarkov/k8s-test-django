### Как запустить прокект в minikube

[Kubectl](https://kubernetes.io/ru/docs/tasks/tools/install-kubectl/) и [minikube](https://minikube.sigs.k8s.io/docs/) должны быть установлены и настроены.

перейдите в папку `backend_main_django` и создайте image для minikube
```sh
cd backend_main_django
minikube image build -t django-app:latest .
```

в корневой папуке проекта необходимо создать файл с зависимостями, например `k8s-config.yml` и прописать в него необходимые переменные окружения:

```sh
apiVersion: v1
kind: ConfigMap
metadata:
  name: django-app-config
  labels:
    app: django-app
    tier: django
data:
  SECRET_KEY: {REPLACE_ME}
  DATABASE_URL: postgres://{USERNAME}:{PASSWORD}@{IP}:{PORT}/{DB}
  DEBUG: 'True'
  ALLOWED_HOSTS: 127.0.0.1,localhost
```

Загружаем config map командой
```sh
kubectl apply -f k8s-config.yml
```

Запустить проект командой
```sh
kubectl apply -f k8s-django-app.yml
```

в случае изменения конфигурационных данных необходимо снова применить `apply` к файлу конфигурации и перезагрузить deploy

```sh
kubectl rollout restart deployment django-k8s
```

### Запуск Imgress в minikube

Включите аддон Ingress в Minikube
```sh
minikube addons enable ingress
```

Узнайте IP адрес ноды командой `minikube ip` и пропишите в файле `\etc\hosts` строку
```text
{Node IP} star-burger.test
```

Добавьте `star-burger.test` в ALLOWED HOSTS

Запустите конфигурацию Ingress коммандой
```sh
kubectl apply -f k8s-ingress.yml
```

и открывайте сайт в браузере по указанному доменному имени.

### Запуск очистки пользовательскизх сессий

В проекте настроена автоматическая очистка пользовательских сессий при помощи `CronJob`
Для запуска планировщика выполните комманду
```sh
kubectl apply -f k8s-django-clearsessions.yml 
```

### Запуск миграции БД

Для запуска миграции базы данных, запукстите 
```
kubectl apply -f k8s-django-migrate.yml
```
