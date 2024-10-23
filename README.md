# Установка Mattermost на сервер с Ubuntu

## Подключение к удалённому серверу через SSH:
```
ssh <username>@<ip_address>
```

## Необходимое ПО для работы:
- Docker
- Docker Compose
- Git
- PostgeSQL
- Mattermost Docker образ
- Mattermost дополнение для Docker решения

## Установка компонентов:

1. Установка Docker и Docker Compose:

```
sudo apt install docker.io
sudo apt install docker-compose
```

2. Запуск контейнера Mattermost на основе образа из DockerHub:

```
docker run --name mattermost-preview -d --publish 8065:8065 mattermost/mattermost-preview
```

3. Установка Git на удалённый сервер:

```
sudo apt install git
```

4. Клонирование репозитория для Mattermost Docker решения:

```
git clone https://github.com/mattermost/docker && cd docker
```

5. Копирование файлов конфигурации + настройка в редакторе Nano (сохранение изменений Ctrl+O, выход Ctrl+X):

```
cp env.example .env
nano env.example .env
```
6. Создание рабочих директорий в Docker Volumes:

```
mkdir -p ./volumes/app/mattermost/{config,data,logs,plugins,client/plugins,bleve-indexes} 
```

Эта часть команда использует опцию -p, которая позволяет создавать родительские директории при необходимости. Команда создает следующую структуру директорий:

```
./volumes/app/mattermost/config
./volumes/app/mattermost/data
./volumes/app/mattermost/logs
./volumes/app/mattermost/plugins
./volumes/app/mattermost/client/plugins
./volumes/app/mattermost/bleve-indexes
```

7. Изменение владельца и группы:

```
chown -R 2000:2000 ./volumes/app/mattermost
```

Эта команда изменяет владельца и группу директории ./volumes/app/mattermost на пользователя с ID 2000 и группу с ID 2000. Опция -R указывает на то, что изменения должны применяться рекурсивно ко всем поддиректориям и файлам.

8. Запуск контейнеров через Docker Compose:

```
docker-compose -f docker-compose.yml -f docker-compose.without-nginx.yml up -d
```

```
-f docker-compose.yml: Это флаг -f (или --file) указывает на дополнительный файл конфигурации Docker Compose. В данном случае это основной файл конфигурации.
-f docker-compose.without-nginx.yml: Аналогично предыдущему пункту, этот флаг указывает на второй файл конфигурации.
up: Эта команда запускает все сервисы, определенные в файлах конфигурации.
-d: Этот флаг означает "detached mode", то есть контейнеры будут запускаться в фоновом режиме
```