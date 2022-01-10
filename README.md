# lektor-webpack-project

Для сборки был создан следующий файл config.yml:

Версия:
```
version: 2.1
```

Включение уже написанной конфигурации для работы с python:
```
orbs:
  python: circleci/python@1.2
```

Указание образа виртуальной машины, на которой будет происходить сборка и анализ:
```
jobs:
  build-and-test: 
    docker:
      - image: cimg/python:3.8
    steps:
      # подключение к репозиторию гитхаба с правами записи
      - add_ssh_keys:
          fingerprints:
            - 47:28:1d:4c:cc:b6:41:59:2e:0f:76:15:68:9d:3b:64

      - checkout
      # установка зависимостей (включая lektor)
      - run: pip install -r requirements.txt
      - run: npm install webpack/
      # сборка и деплой сайта на лекторе
      - run: lektor build -f webpack && lektor deploy

```

Запуск указанных jobs: 
```
workflows:
  sample: 
    jobs:
      - build-and-test
```

Демонстрация успешной сборки:

![successful-build](https://github.com/polinalazebnikova/lektor-webpack-project/blob/master/lektor-webpack-circleci-success.png?raw=true)
