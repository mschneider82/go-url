# Go URL
[![CircleCI](https://circleci.com/gh/alexbrazier/go-url.svg?style=svg)](https://circleci.com/gh/alexbrazier/go-url)
[![Cypress Dashboard](https://img.shields.io/badge/cypress-dashboard-brightgreen.svg)](https://dashboard.cypress.io/#/projects/7dct13/runs)
[![dependencies](https://img.shields.io/david/alexbrazier/go-url.svg?path=frontend)](https://david-dm.org/alexbrazier/go-url?path=frontend)

A simple URL shortener written in Go with a React frontend and Postgres database

# Features

- Shorten urls based on a user defined key
- Alias a key to point to another short url
- Open multiple pages at once by separating keys with a comma
- Alias a key to point to multiple other keys
- Opensearch integration to provide suggestions directly to browser
- Frontend to view most popular searches and search to find existing links
- Frontend to allow anyone to add and edit links
- Optional authentication using Azure AD
- Slack `/` command integration
- Slackbot integration

# Getting Started

Install Docker and Docker Compose, and run:

```sh
docker-compose up
```

Open http://localhost:8080/go

## Development

Run Postgres manually or with Docker

```sh
docker-compose up postgres
```

Install node (via nvm), yarn & go
```sh
brew install nvm yarn go
nvm install
```

### Start frontend
```sh
cd frontend
yarn
yarn start
```

### Start API
```sh
cd api
dep ensure
POSTGRES_PASS=password HOSTS=localhost APP_URI=http://localhost:3000 go run server.go
```

## Enviroment Configuration

| Env Var              | Required | Default        | Example                      | Description                                                                                            |
|----------------------|----------|----------------|------------------------------|--------------------------------------------------------------------------------------------------------|
| `HOSTS`                | yes      |                | go.domain.com,go2.domain.com | List of comma separated hosts that the server will be able to be accessed from                         |
| `BLOCKED_HOSTS`        |          |                | go.domain.com,go2.domain.com | List of hosts you want to block from being linked - HOSTS are already included to stop recursive calls |
| `APP_URI`              | yes      |                | https://go.domain.com        | Default URI of app - used to link back to app                                                          |
| `PORT`                 |          | 1323           |                              | Port the app will run on                                                                               |
| `DEBUG`                |          | false          |                              | Enable more logging                                                                                    |
| `JSON_LOGS`            |          | false          |                              | Use JSON logs where possible                                                                           |
| `POSTGRES_ADDR`        |          | localhost:5432 |                              | Postgres db address                                                                                    |
| `POSTGRES_DATABASE`    |          | go             |                              | Postgres db name                                                                                       |
| `POSTGRES_USER`        |          | postgres       |                              | Postgres user                                                                                          |
| `POSTGRES_PASS`        | yes      |                |                              | Postgres password                                                                                      |
| `SLACK_TOKEN`          |          |                | xoxb-xxxxxxxxx-xxxxxxxx-xxxx | Slack OAuth token to enable slackbot                                                                   |
| `SLACK_SIGNING_SECRET` |          |                | xxxxxxxxxxx                  | Slack signing secret to enable Slack `/go` command                                                     |
| `ENABLE_AUTH`          |          | false          |                              | Enable Azure auth or not - if enabled, all other fields must be filled in                              |
| `AD_TENANT_ID`         |          |                |                              | Azure AD tenant ID                                                                                     |
| `AD_CLIENT_ID`         |          |                |                              | Azure AD client ID                                                                                     |
| `AD_CLIENT_SECRET`     |          |                |                              | Azure AD client secret                                                                                 |
| `SESSION_TOKEN`        |          |                |                              | Secret session token to store the user sessions                                                        |


## TODO

- Currently does not work in load balancer if running Slackbot
  - If load balancing is required, then you could run a single instance with Slackbot enabled, then the rest without.
