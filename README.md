# CloudFlow

Статичний демонстраційний вебзастосунок для лабораторної роботи з CI/CD.
Сторінка працює в контейнері на базі nginx, а HTML і CSS перевіряються
лінтерами html-validate та Stylelint.

## Локальна перевірка

```bash
npm install
npm run lint
```

## Запуск у Docker

```bash
docker build -t cloudflow .
docker run --rm -p 80:80 cloudflow
```

Після запуску сторінка буде доступна за адресою <http://localhost>.

## CI/CD

Workflow `.github/workflows/ci.yml` після push або pull request до гілки
`main` запускає лінтери та перевіряє збірку Docker-образу. Після push у `main`
він також публікує теги `latest` і SHA коміту в Docker Hub.

У налаштуваннях GitHub-репозиторію потрібно створити два Actions secrets:

- `DOCKERHUB_USERNAME` — ім'я користувача Docker Hub;
- `DOCKERHUB_TOKEN` — access token із Docker Hub.

## Розгортання з Watchtower

На VM скопіюйте `.env.example` у `.env`, укажіть своє ім'я користувача Docker
Hub і запустіть сервіси:

```bash
cp .env.example .env
docker compose up -d
```

Compose запустить вебзастосунок на порту `80` і Watchtower. Кожні 30 секунд
Watchtower перевірятиме новий тег `latest`, оновлюватиме контейнер застосунку
та видалятиме старий образ.
