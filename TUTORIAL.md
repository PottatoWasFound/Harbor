# Harbor tutorial

Harbor is a small macOS home for Laravel projects. It keeps your project folders together, starts the processes Laravel needs, and shows their output in one place. Your project files stay where you put them; Harbor only stores a project registry and its own logs.

## 1. Install Harbor

1. Download `Harbor-macOS-arm64.zip` from the [latest release](https://github.com/PottatoWasFound/Harbor/releases).
2. Double-click the ZIP and open `Harbor.app`.
3. If macOS asks whether to open it, choose **Open**. This development build is ad-hoc signed and is not notarized.
4. Harbor remains available in the menu bar when you close its window.

Harbor can use PHP and Composer from Laravel Herd, or install tools through Homebrew. Open **Services** to see what is already available. A green status means the service is running; **Install** adds a missing tool; **Start** and **Stop** control a Homebrew service.

## 2. Create a Laravel project

1. Select **New project** on the Overview page.
2. Enter a lowercase project name such as `shop-demo`.
3. Choose a parent folder such as `~/Documents/Code`.
4. Select **Create project**.

Harbor runs Composer's Laravel project creation command and assigns a free web port. When it finishes, the project appears in the sidebar. A fresh Laravel project uses SQLite by default, so it can run without MySQL.

To bring in an existing project, choose **Import**, select its root folder, and make sure it contains both `artisan` and `composer.json`. Harbor does not delete or move imported files.

## 3. Install project dependencies

Open the project, select **More**, and choose **Install Composer dependencies**. For a frontend, choose **Install npm dependencies**. These commands run inside the selected project folder and their output appears under **Activity**.

If a project came from Git, Composer dependencies are normally not committed, so run Composer install before starting the app. Run npm install when the project has a `package.json`.

## 4. Start the web app

Open a project and select **Start** on the **Web server** card. Harbor runs Laravel's local development server on `127.0.0.1:<project port>`. Select **Open site** to view it in a browser.

The web server card changes to **Stop** while it is running. Harbor records the command output in **Activity** and stops the process group when you quit Harbor normally.

Harbor checks project ports when assigning them. A project's Vite port is its web port plus 1000, and its Reverb port is its web port plus 2000. This prevents the common development processes from overlapping.

## 5. Start Vite and frontend hot reload

After npm install, select **Start** on the **Vite** card. Harbor runs `npm run dev` bound to loopback. Vite uses the project's web port plus 1000. Keep Vite running while editing CSS, JavaScript, or Vue/React components.

For a production-style asset build, open the project **Terminal** and run `npm run build`.

## 6. Configure local services

Go to **Services** and install or start the services your `.env` expects:

| Service | Typical local address | Use |
| --- | --- | --- |
| MySQL | `127.0.0.1:3306` | Relational database |
| PostgreSQL | `127.0.0.1:5432` | Alternative relational database |
| Redis | `127.0.0.1:6379` | Cache, sessions, and queues |
| Mailpit | SMTP `127.0.0.1:1025` | Capture outgoing mail |
| Mailpit | Inbox `http://127.0.0.1:8025` | View captured mail |

Harbor does not overwrite your `.env`. Open **Edit .env** in the project toolbar and set values that match your own database, cache, mail, and queue configuration. Homebrew service credentials and settings remain controlled by Homebrew.

## 7. Run migrations and seed data

The **Artisan workbench** provides shortcuts for common commands. Try **Migrations** to run `migrate:status`, or type a command such as:

```text
migrate
db:seed
storage:link
route:list
```

Read-only commands run immediately. Commands that can change application or database state show a confirmation first. Harbor passes arguments directly to Artisan and does not run them through a shell.

## 8. Run queues and scheduled tasks

Start **Queue worker** when the app dispatches background jobs. Harbor runs `php artisan queue:work --tries=3 --timeout=90`.

Start **Scheduler** when the app defines scheduled tasks. Harbor runs `php artisan schedule:work` and keeps it alive while you develop. Use **schedule:list** in the Artisan workbench to inspect scheduled tasks.

For Redis-backed queues, start Redis first and set `QUEUE_CONNECTION=redis` in `.env`. For database-backed queues, run the queue-table migration your Laravel version requires.

## 9. Horizon, Reverb, and interactive tools

Horizon and Reverb are optional Laravel packages. Install and configure them in the project first, then use their Harbor process cards:

```text
composer require laravel/horizon
composer require laravel/reverb
php artisan horizon:install
php artisan reverb:install
```

Reverb uses the web port plus 2000. Set `REVERB_PORT` and `VITE_REVERB_PORT` in `.env` to that port when your project needs WebSockets.

For Tinker, Sail, Laravel installer, Pint, Pest, starter kits, or any interactive command, select **Terminal**. Harbor opens a project-aware shell with the same PHP and tool paths it uses for Artisan.

## 10. Pretty URLs and HTTPS

Harbor's default address is a loopback port because it does not replace your system web server. If you want `shop-demo.test` and local HTTPS, use an existing Laravel Herd setup or select **Settings & domains → Set up Valet in Terminal**. Then use **More → Link .test domain** and **Secure with HTTPS**.

Valet changes macOS DNS and Nginx configuration. Do not install a second domain manager if Herd already manages your sites; use Herd's controls instead.

## 11. Read logs and stop cleanly

Select **Activity** to see running processes, exit codes, and live output. Expand a job to copy its visible output or reveal its complete log file. Harbor keeps logs under `~/Library/Application Support/Harbor/Logs`.

Use a process card's **Stop** button to stop one job. **Stop project processes** stops project jobs while leaving shared Homebrew services alone. Quitting Harbor normally stops its project process groups; closing the window leaves the menu-bar app and running processes active.

## 12. Common fixes

**“Install PHP and Composer first.”** Open Services. Harbor may detect them from Herd, or you can install them through Homebrew.

**“Composer dependencies are missing.”** Run **More → Install Composer dependencies**, then try the process again.

**“This project has no package.json.”** Vite is only available for projects with a frontend package manifest.

**The browser cannot connect.** Expand the web job in Activity. The log usually shows a port conflict, a missing PHP extension, or an application boot error. Confirm that the URL uses the project's displayed port.

**Mailpit or Redis does not start.** A Homebrew service is shared with other applications. Check its status in Services and inspect the Homebrew service configuration if another app already owns the port.

**A command needs prompts or an editor.** Use the project Terminal. Harbor's workbench is designed for non-interactive Artisan commands.

**I changed `.env` but the app still uses old values.** Stop and restart the project process. Laravel can cache configuration; run `optimize:clear` from the workbench when appropriate.

## Next steps

Read the [Laravel documentation](https://laravel.com/docs), especially the guides for [Artisan](https://laravel.com/docs/artisan), [queues](https://laravel.com/docs/queues), [scheduling](https://laravel.com/docs/scheduling), and [Vite](https://laravel.com/docs/vite). Harbor is a local process manager; Laravel remains responsible for your application's code and configuration.

Created by darils.
