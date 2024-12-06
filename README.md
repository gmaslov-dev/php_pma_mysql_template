# php_pma_mysql_template

## Структура проекта:
```text

project/
├── app/                     # Корневая папка приложения
│   ├── public/              # Веб-доступная папка (корень для Nginx)
│   │   ├── index.php        # Точка входа приложения
│   │   ├── .htaccess        # Конфигурация Apache (если потребуется)
│   └── ...                  # Остальной код приложения
│
├── db_data/                 # Данные MySQL (создаются автоматически)
│
├── nginx/                   # Конфигурация и логи Nginx
│   ├── conf.d           
│   │   └── default.conf    # Основной конфигурационный файл Nginx
│   └── logs/                # Логи Nginx
│       ├── access.log       # Лог доступа
│       └── error.log        # Лог ошибок
│
├── docker-compose.yml       # Docker Compose конфигурация
│
└── README.md                # Документация проекта (по желанию)
```

### Пояснения
    app (PHP 8.3):
        Работает в режиме php-fpm.
        Монтируется локальная директория ./app как корень приложения.
        Этот сервис готов для работы как с ванильным PHP, так и для запуска Laravel в будущем.

    nginx:
        Использует кастомный конфигурационный файл nginx.conf, который нужно создать. Пример файла ниже.
        Слушает локальный порт 8080 для доступа через браузер.

    mysql:
        Сохраняет данные в локальный volume db_data.
        Настраиваются переменные среды для подключения.

    phpmyadmin:
        Подключается к базе данных через сервис mysql.
        Доступен по адресу http://localhost:8081.

## Шаги для запуска

Убедитесь, что у вас установлен Docker и Docker Compose.
Создайте директории и файлы:
    app/ — для исходного кода приложения.
    nginx/nginx.conf — для конфигурации Nginx.
    nginx/logs/ — для логов Nginx.
Запустите команду:

```bash
docker-compose up -d
```


Доступ:
    Приложение: http://localhost:8080
    PhpMyAdmin: http://localhost:8081

## Переход на Laravel

Для Laravel достаточно:

Перенести исходный код в app/.
Убедиться, что директория public/ является корневой для Nginx.
Установить зависимости Laravel через composer. 
Можно добавить сервис для Composer в docker-compose.yml или использовать локальный.

Дополнительные рекомендации:

Для ванильного PHP:
Добавьте поддиректории, например views/ для шаблонов или src/ для модулей/функций, чтобы упорядочить проект.

Для Laravel:
Когда перейдёте на Laravel, вся структура приложения 
(например, routes/, app/, storage/) автоматически появится 
внутри папки app/. Убедитесь, что public/ остаётся корнем для веб-сервера.

## Дополнительно

### Конфиг nginx
```yml
volumes:
- ./nginx/nginx.conf:/etc/nginx/nginx.conf
- ./nginx/conf.d:/etc/nginx/conf.d
```
