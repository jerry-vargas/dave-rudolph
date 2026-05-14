# Dave Rudolph

Craft CMS site for Dave Rudolph Music. The project uses Craft 5 for content management, Twig for templates, and Vite for frontend assets.

This site is currently in production. The next major phase is preparing the project to move toward a headless architecture with a Vue frontend.

## Stack

- PHP 8.2
- Craft CMS 5.8
- Twig templates
- Vite 6
- Tailwind CSS 4
- AWS S3 volume for uploads
- SendGrid mail transport

## Project structure

- `templates/` - Twig templates, blocks, and shared components
- `src/` - frontend source files for JS, SCSS, CSS, fonts, and images
- `web/` - public web root and built assets
- `config/project/` - Craft project config
- `config/` - Craft app, general, routes, and Vite config
- `DB/10-6-25.sql` - local database dump checked into the repo

## Requirements

- PHP 8.2
- Composer
- Node.js and npm
- MySQL
- DDEV is strongly recommended for local development

## Local setup

1. Install PHP dependencies:

```bash
composer install
```

2. Install frontend dependencies:

```bash
npm install
```

3. Create an environment file from the dev example:

```bash
cp .env.example.dev .env
```

4. Fill in the required environment variables in `.env`:

- `CRAFT_APP_ID`
- `ENVIRONMENT`
- `CRAFT_SECURITY_KEY`
- `CRAFT_DB_DATABASE`
- `CRAFT_DB_USER`
- `CRAFT_DB_PASSWORD`
- `PRIMARY_SITE_URL`
- `SYSTEM_EMAIL_ADDRESS`
- `SENDER_NAME`
- `REPLY_TO_ADDRESS`
- `SENDGRID_API_KEY`
- `ACCESS_KEY_ID`
- `SECRET_ACCESS_KEY`

Set `ENVIRONMENT=dev` locally. The app bootstrap reads `ENVIRONMENT`, while some config also checks `CRAFT_ENVIRONMENT`.

5. Import the local database if needed:

```bash
mysql -u <user> -p <database> < DB/10-6-25.sql
```

6. Apply Craft project config and any pending migrations:

```bash
php craft up
```

## DDEV workflow

The repo already leans toward DDEV. `package.json` includes an `up` script that assumes DDEV is installed and available.

Typical flow:

```bash
ddev start
ddev composer install
npm install
php craft up
npm run dev
```

## Frontend development

Start the Vite dev server with:

```bash
npm run dev
```

Create a production build with:

```bash
npm run build
```

Built files are written to `web/dist/`.

## Important local hostname note

Vite is currently hardcoded to the DDEV hostname `https://daverudolphmusic.ddev.site:5173` in both:

- `vite.config.js`
- `config/vite.php`

If your local DDEV project name or hostname is different, update both files or the dev asset server will not load correctly.

## Content model

From the checked-in Craft project config, the main content areas are:

- `Home` single
- `Pages` section
- `Tour Dates` section
- `Navigation` section
- global sets for navigation, footer links, and tour content

The homepage renders upcoming tour dates with Sprig pagination and pulls its main hero content from the `Home` entry.

## Integrations

- Uploads volume points to AWS S3
- Mail is configured through SendGrid
- Installed Craft plugins include CKEditor, Feed Me, Sprig, Freeform, and AWS S3

## Deployment notes

- The public document root is `web/`
- Production assets are served from `/dist/`
- Site base URL is driven by `PRIMARY_SITE_URL`
- Craft project config is committed under `config/project/`

## Current repo state

At the time of this update, the repo does not include installed Composer dependencies or `node_modules`, so a fresh checkout needs both `composer install` and `npm install`.
