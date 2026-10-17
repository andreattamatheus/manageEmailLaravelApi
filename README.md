# Manage Email Laravel API

A Laravel API for managing email records: create, read, update, and delete records, including parsing raw email content. All endpoints (besides auth) require a Bearer token.

## Tech Stack

- PHP 8.1+, Laravel
- MySQL
- Laravel Sanctum (auth), queues

## Prerequisites

- PHP 8.1+
- Composer
- MySQL
- Node.js + npm (for frontend assets)

## Installation

```sh
git clone https://github.com/andreattamatheus/manageEmailLaravelApi
cd manageEmailLaravelApi
composer install
```

Copy `.env.example` to `.env` and fill in your database credentials:

```
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=YOUR_DATABASE
DB_USERNAME=YOUR_USERNAME
DB_PASSWORD=YOUR_PASSWORD
```

```sh
php artisan optimize
php artisan migrate:fresh --seed
php artisan serve
```

Seeding creates two users: `backoffice@yopmail.com` and `admin@yopmail.com`, both with password `123123123`.

## Background Jobs

```sh
php artisan schedule:list     # list scheduled jobs
php artisan email:parse       # parse raw email content
php artisan queue:listen      # process queued jobs
```

## Quality & Tests

```sh
./vendor/bin/pint
./vendor/bin/phpstan analyse
php artisan test
```

## API Endpoints

### Auth

| Method | URL | Description |
|---|---|---|
| POST | `/register` | Create a new user |
| POST | `/login` | Log in and receive a Bearer token |

### Email Records (`/api/v1/emails`)

| Method | URL | Description |
|---|---|---|
| POST | `/api/v1/emails` | Create a record, parses raw email content |
| GET | `/api/v1/emails` | List all records (excludes deleted, pagination optional) |
| GET | `/api/v1/emails/{emailId}` | Fetch a single record |
| PUT | `/api/v1/emails/{emailId}` | Update a record |
| DELETE | `/api/v1/emails/{emailId}` | Soft-delete a record |

All record endpoints require:

```
Authorization: Bearer {your_token}
```

## Challenge

Take-home engineering assessment. Raw emails collected by a SendGrid inbound webhook are stored in a `successful_emails` table.

- **Parsing command:** extract only the plain-text body from the raw `email` column (no HTML, only printable characters and `\n`) and save it to `raw_text`. It runs every hour and skips records already processed. No external parsing services allowed.
- **Authenticated REST API** over the same table:
  - **Store** — create a record and parse it automatically
  - **Get by ID**
  - **Update** by ID
  - **Get all** — excluding deleted records, pagination optional
  - **Delete by ID** — soft delete only

**Evaluated on:** code quality and cleanliness, architecture, accuracy of the parsed output, and development time.

## Contact

Matheus Andreatta — [@andreattamatheus](https://github.com/andreattamatheus)
