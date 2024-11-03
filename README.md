# Базовая настройка

## Запуск minikube

[Инструкция по установке](https://minikube.sigs.k8s.io/docs/start/)

```bash
minikube start
```


## Добавление токена авторизации GitHub

[Получение токена](https://github.com/settings/tokens/new)

```bash
kubectl create secret docker-registry ghcr --docker-server=https://ghcr.io --docker-username=<github_username> --docker-password=<github_token> -n default
```


## Установка API GW kusk

[Install Kusk CLI](https://docs.kusk.io/getting-started/install-kusk-cli)

```bash
kusk cluster install
```


## Настройка terraform

[Установите Terraform](https://yandex.cloud/ru/docs/tutorials/infrastructure-management/terraform-quickstart#install-terraform)


Создайте файл ~/.terraformrc

```hcl
provider_installation {
  network_mirror {
    url = "https://terraform-mirror.yandexcloud.net/"
    include = ["registry.terraform.io/*/*"]
  }
  direct {
    exclude = ["registry.terraform.io/*/*"]
  }
}
```

## Применяем terraform конфигурацию 

```bash
cd terraform
terraform apply
```

## Настройка API GW

```bash
kusk deploy -i api.yaml
```

## Проверяем работоспособность

```bash
kubectl port-forward svc/kusk-gateway-envoy-fleet -n kusk-system 8080:80
curl localhost:8080/hello
```


## Delete minikube

```bash
minikube delete
```


### Начальная архитектура:

Монолитное приложение не позволяет обеспечить масштабируемость и затрудняет добавление новых устройств/обработчиков для системы умного дома. В текущее решение входит лишь сервис управлением температуры. 

Доменные области: 
- Пользовательский Интерфейс (внешняя система)
- Упрвление температурой и хранение данных
- Управление Устройствами

База данных: PostgreSQL
Язык/Фреймворк: Java, Spring boot
Архитектура: Монотитная. сборка, логика, работа с данными находится в одном приложении. 
Взаимоедействие: синхронное.
Масштабируемость: ограничена
Развертывание: требует остановки всего приложения. 

архитектура описана в файле c4_current.plum

### Переход на микросервисную архитектуру

Архитектура описана в файле (Котнейнеры и Компоненты) c4.plum

### ER диаграмма

архитектура описана в файле er.plum