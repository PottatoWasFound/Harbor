# Harbor for macOS

Created by darils.

New to Harbor? Start with the [step-by-step tutorial](TUTORIAL.md).

A native Laravel development manager inspired by Laragon. Built with SwiftUI, Foundation, and a small process-group helper. No JavaScript desktop runtime or third-party Swift dependencies.

## Open the app

Build the app with `bash scripts/build.sh`, then open `Harbor.app` beside this source folder. The build targets the Mac's architecture and macOS 13 or later. You can move the app to Applications yourself. It is locally ad-hoc signed, not Developer ID signed or notarized for public distribution.

Harbor detects Homebrew in `/opt/homebrew` and `/usr/local`. It also detects PHP and Composer from an existing Laravel Herd installation.

1. Open **Services** to review installed tools. Install any missing services you want.
2. Create a Laravel project or import a folder containing `artisan` and `composer.json`.
3. For imported projects, use **More → Install Composer dependencies** if needed. Use **Install npm dependencies** before starting Vite.
4. Start the web server and open the site. Start queues, scheduler, and other processes when your app needs them.
5. Configure the project's `.env` for its own database, mail, queue, and cache connections.

## Implemented

| Area | Available in Harbor |
| --- | --- |
| Projects | Create via Composer, import, search, persistent registry, remove from list without deleting files |
| PHP | Homebrew version installation and selection per project; existing Herd default PHP detection |
| Databases | Install/start/stop MySQL and PostgreSQL through Homebrew; SQLite works through Laravel |
| Supporting services | Redis, Mailpit and optional Nginx installation/start/stop |
| Development | Laravel server, Vite, queue worker, scheduler, Horizon, Reverb |
| Artisan | Arbitrary command arguments, common shortcuts, confirmation for custom/state-changing commands |
| Files | Finder, project terminal, `.env` and Laravel log opening |
| Dependencies | Composer and npm install with live output |
| Domains and HTTPS | Optional Valet setup/link/secure workflows in Terminal |
| Lifecycle | Menu bar controls, all-project web start, stop project jobs, shutdown of owned process groups |
| Logs | Live output, command exit status, copy and reveal log files |

## Scope and limitations

This is a working 0.1 development build, not complete Laragon feature parity. Runtime binaries are supplied by your Mac, Homebrew, or Herd; Harbor is not a portable bundled PHP distribution.

- Horizon and Reverb controls require their Composer packages and project configuration. Starter kits, authentication, Sail, Pint, Pest, and other Laravel tools are available through the project terminal/Artisan workbench; they do not have dedicated setup screens. Sail needs Docker.
- Local domains and certificates are delegated to Valet. Installing Valet changes system DNS/Nginx configuration and can conflict with Herd or other local servers. On a Mac already using Herd, use Herd's domain/certificate management instead of installing another Valet setup. Harbor's selected PHP applies to Harbor's commands; Valet/Herd manage their own site runtime.
- Harbor's web server uses `127.0.0.1:<port>`, Vite uses `<port+1000>`, and Reverb uses `<port+2000>`. Set Reverb environment variables accordingly. Reserved project ports are checked for overlap. External port conflicts appear as command failures in logs; Harbor does not claim a process is HTTP-ready just because it is running.
- Homebrew services are shared system tools, not isolated Harbor instances. **Start** uses `brew services run` without enabling login startup. Services keep running when Harbor quits. Existing service configurations, including their bind addresses and credentials, remain in effect.
- Project processes stop when Harbor quits normally. Force-killing Harbor can leave processes running. A closed window keeps Harbor active in the menu bar.
- No built-in database browser, database/user creation UI, remote sharing/tunnels, deployment, auto-update, or full PHP extension manager yet.
- The log folder persists across sessions and is not automatically pruned. The live text view retains the latest 100–120 KB; complete output stays in the log file. Logs can contain project data.

## Build and checks

Requires Apple command line developer tools with Swift 5.9 or later:

```sh
bash scripts/check.sh
bash scripts/build.sh
```

The build script creates `../Harbor.app`, an app icon, and a local ad-hoc signature. Build on an Intel Mac to produce an Intel binary. The checks use a standalone Swift executable, so full Xcode and XCTest are not required. Checks cover argument handling, shell quoting, project validation, port conflicts, persistence and corruption handling, process groups, descendant cleanup, and failed executable launches.

Application data defaults to `~/Library/Application Support/Harbor`. Developers can set `HARBOR_DATA_DIR` to an isolated folder for testing. The app never runs a project command through a shell; Terminal scripts quote filesystem paths explicitly. Importing a project does not execute it. Creating projects and installing dependencies intentionally runs Composer/npm scripts.

## References

- [Laragon features](https://laragon.org/docs)
- [Laravel documentation](https://laravel.com/docs)
- [Laravel Valet](https://laravel.com/docs/valet)
- [Homebrew service commands](https://docs.brew.sh/Manpage#services-subcommand)

Harbor is independent and is not affiliated with Laravel or Laragon.
