### Как запустить прокект prod окружении

[Kubectl](https://kubernetes.io/ru/docs/tasks/tools/install-kubectl/) и [minikube](https://minikube.sigs.k8s.io/docs/) должны быть установлены и настроены.

Prod окружение для проекта развернуто в yandex cloud. Во всех манифестах должен быть указан namespace `edu-jovial-mclean`

Маршрутизация траффика идет через `NodePort`

Подключить базу данных postgres в yandex cloud по [инструкции](https://cloud.yandex.ru/ru/docs/managed-postgresql/operations/connect#with-ssl_9), добавить SSL сертификат в `secrets` кластера

в папке окружения, в данном случае это `dev-yc-sirius/edu-jovial-mclean` необходимо создать файл с зависимостями, например `k8s-config.yml` и прописать в него необходимые переменные окружения:

```sh
apiVersion: v1
kind: ConfigMap
metadata:
  name: django-app-config
  namespace: edu-jovial-mclean
  labels:
    app: django-app
    tier: django
data:
  SECRET_KEY: {REPLACE_ME}
  DATABASE_URL: postgres://{USERNAME}:{PASSWORD}@{IP}:{PORT}/{DB}
  DEBUG: 'True'
  ALLOWED_HOSTS: 127.0.0.1,localhost
```