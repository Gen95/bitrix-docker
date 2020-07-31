Разворачивал bitrix в docker контейнере

За базу брал образ https://github.com/sidigi/bitrix-docker.git

## В сборке
- PHP 7.3 (opcache, xdebug)
- nginx 1.14
- mysql 5.7
- smtp (иммитация, перехват писем небольшим скриптом на go)
- mailhog для просмотра писем

Соответствует всем тестам на БУС

## Установка зависимостей
- Docker
```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo groupadd docker
sudo usermod -aG docker $USER
```

- Docker compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

## Начало работы
- Выполните настройку окружения

Скопируйте файл `.env.example` в `.env`

```
cp .env.example .env
```

- Запустите bitrix-docker
```
make up
```

или если нет поддержки Makefile:

```
docker-compose up --build -d
```

### Установка через `bitrixsetup.php`
```
make bitrix-setup
```

- Установка будет доступна по адресу `http://localhost:8080`
> При установке `bitrix` необходимо в окне создания базы данных в графе "Сервер" 
`localhost` заменить на `mysql` (так как контейнер поднятый в сети имеет название `mysql`)

### Востановление через `restore.php`
- Скачайте `restore.php` (файл будет скачан с официального сайта автоматически)
```
make bitrix-restore url=<ссылка для переноса>
```

- Востановление будет доступна по адресу `http://localhost:8080/restore.php`
> При востановлениии необходимо в окне создания базы данных в графе "Сервер" 
`localhost` заменить на `mysql` (так как контейнер поднятый в сети имеет название `mysql`)

### Примечания

- Бэкап работы лежит в папке backup. Чтобы просмотреть работу, нужно развернуть бэкап. 
- Сделал каталог книг из готового компонента, также был добавлен фильтр по авторам.
- Средняя стоимость книг в выборке выводится на экран, но нет привязки скрипта к кнопке, по который средняя цена должна выводиться (расположена в фильтре)