# Continuous Integration

## Мета роботи
Налаштувати GitHub Workflow для автоматичної збірки Docker-образу front-end репозиторію та його завантаження у GitHub Container Registry.

## Опис виконаних дій

### 1. Створено GitHub Workflow
У репозиторії створено workflow-файл:
.github/workflows/docker-publish.yml


Workflow налаштований з двома тригерами:
- ручний запуск (workflow_dispatch)
- автоматичний запуск при push-коміті у гілки:
  - main
  - feature/*

### 2. Налаштовано job для збірки та публікації Docker-образу
Workflow містить один job, який виконує наступні кроки:

- Клонування репозиторію з використанням actions/checkout
- Встановлення менеджера пакетів pnpm за допомогою npm
- Встановлення залежностей та збірка front-end проєкту командою pnpm run build
  - усі команди виконуються в одному скрипті
- Авторизація в GitHub Container Registry з використанням:
  - імені користувача з GitHub context
  - вбудованого `GITHUB_TOKEN` як паролю
- Збірка та публікація Docker-образу з використанням docker/build-push-action
  - контекст збірки — поточна директорія
  - параметр push встановлено в true
  - тег образу має вигляд: ghcr.io/<логін>/<ім’я_репозиторію>

### 3. Створено мінімальний front-end проєкт
Для коректної роботи workflow у репозиторії додано мінімальний front-end проєкт:
- файл package.json зі скриптом build
- Dockerfile для створення Docker-образу зі статичними файлами

### 4. Перевірка результату
Після виконання workflow Docker-образ успішно опубліковано у GitHub Container Registry.  
У вкладці **Packages** GitHub відображається контейнер з ім’ям, що відповідає імені репозиторію.

## Висновок
У результаті роботи було створено та налаштовано GitHub Workflow для автоматичної збірки та публікації Docker-образу front-end проєкту в GitHub Container Registry.

# Continuous Delivery

У межах виконання практично-лабораторного заняття було реалізовано повний CI/CD-процес із використанням хмарних сервісів Microsoft Azure та GitHub Actions.

### Виконані дії

- Створено ресурсну групу та App Service в хмарній платформі **Microsoft Azure**. <br><br>
  <img width="1132" height="241" alt="image" src="https://github.com/user-attachments/assets/866f292a-b1f3-4803-af87-a09ad5664352" />

- У межах ресурсної групи розгорнуто **Azure App Service (Web App)** на базі **Linux** з типом публікації **Docker Container** та безкоштовним планом.
- У **Azure Active Directory** створено **Service Principal** для надання GitHub доступу до ресурсів Azure.<br><br>
  <img width="1355" height="370" alt="image" src="https://github.com/user-attachments/assets/c75b8afa-60fd-4736-8726-377d71369961" />

- Облікові дані Service Principal збережено як захищений секрет **AZURE_CREDENTIALS** у налаштуваннях репозиторію GitHub.
- У репозиторії налаштовано **GitHub Actions workflow**, який:
  - автоматично збирає проєкт;
  - створює Docker-образ;
  - публікує образ у **GitHub Container Registry (GHCR)**;
  - автоматично розгортає контейнер у **Azure App Service**.
- Workflow успішно виконано, після чого веб-застосунок став доступним за URL-адресою Azure App Service.

### Результат

У результаті роботи налаштовано автоматизований процес неперервної доставки та розгортання застосунку. Кожна зміна в репозиторії приводить до автоматичного створення Docker-образу та його розгортання в хмарному середовищі Azure без ручного втручання.

Дане рішення демонструє практичне застосування підходів **Continuous Delivery** та **Continuous Deployment** із використанням сучасних інструментів CI/CD.
