### Как запустить прокект prod окружении

[Kubectl](https://kubernetes.io/ru/docs/tasks/tools/install-kubectl/) и [minikube](https://minikube.sigs.k8s.io/docs/) должны быть установлены и настроены.

Prod окружение для проекта развернуто в yandex cloud. Во всех манифестах должен быть указан namespace `edu-jovial-mclean`

Маршрутизация траффика идет через `NodePort`