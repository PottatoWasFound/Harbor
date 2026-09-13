# Harbor for macOS

**Created by darils.**
A native macOS development tool for Laravel, inspired by Laragon.

Manage your projects, PHP, Composer, databases, queues, Vite, logs, and other development tools — all in one place.

## Downloads

- [Download Harbor for Apple silicon Macs](Harbor-macOS-arm64.zip?raw=true)
- [Download the complete source code](Harbor-source.zip?raw=true)

The source archive contains the complete Swift package, all source folders, tests, build scripts, documentation, and MIT license. Extract it before building; the source tree is packaged in the archive rather than expanded at this repository's root.

## Run Harbor

1. Download and extract `Harbor-macOS-arm64.zip`.
2. Open `Harbor.app`. You may move it to Applications.
3. Review your tools in Services, then create or import a Laravel project.

Requires an Apple silicon Mac running macOS 13 or later. This is a locally ad-hoc signed development build; it is not Developer ID signed or notarized for public distribution.

## Build from source

Download and extract `Harbor-source.zip`. In the extracted `Harbor` folder, run:

```sh
bash scripts/check.sh
bash scripts/build.sh
```

Apple's command line developer tools with Swift 5.9 or later are required. The build produces `Harbor.app` beside the extracted source folder.

## Features

- Laravel project creation and import
- PHP selection and Composer/npm dependency installation
- Homebrew controls for MySQL, PostgreSQL, Redis, Mailpit, and Nginx
- Laravel web server, queues, scheduler, Vite, and optional Horizon/Reverb controls
- Artisan workbench, project terminals, environment-file access, and live logs
- Optional Terminal workflows for Valet domains and HTTPS
- Existing Laravel Herd PHP and Composer detection

## Development status

Version 0.1 is a working first release. Runtime tools are supplied by Homebrew or your existing installation. It is not full Laragon feature parity: a database browser, remote sharing, and dedicated starter-kit setup screens are not included. Horizon/Reverb need their project packages; Valet setup is separate and can conflict with an existing Herd installation.

See [VALIDATION.md](VALIDATION.md) for completed checks. The source archive's README contains full setup details and limitations.

## License

[MIT License](LICENSE), copyright 2026 darils.

Harbor is independent and is not affiliated with Laravel or Laragon.
