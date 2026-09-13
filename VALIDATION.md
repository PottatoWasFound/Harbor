# Validation — September 13, 2026

Tested on this Apple silicon Mac with Apple Swift 6.3.3, Herd PHP 8.4.23, and the installed Homebrew/Node tools.

## Passed

- Debug and optimized production builds.
- Nine standalone check groups: argument parsing, shell quoting and injection resistance, project names, project port allocation/overlap/exhaustion, persistence and corrupted data preservation, duplicate ports, Laravel folder detection, worker process groups and child cleanup, and missing executable errors.
- Native app launch, accessibility inspection, and visual review of the overview window. PHP from Herd and Homebrew MySQL were detected correctly.
- Local app/helper code signatures and ICNS icon format verified.
- Created an isolated Laravel 13 project through Composer. Dependency installation, app key generation, SQLite creation, and initial migrations completed.
- Executed Artisan environment information, migration status, and the generated application's test suite successfully.
- Laravel's development server returned its welcome page with HTTP 200.
- A second server using the same port failed as expected.
- Laravel queue worker and scheduler started and remained running.
- Vite served its development client with HTTP 200.
- Stopping the managed process groups closed the Laravel and Vite listening ports.

## Not verified end to end

- Installing or starting/stopping Homebrew services was not exercised, to avoid altering services shared with other applications. Read-only discovery was verified.
- Valet DNS and certificate setup was not run; this Mac already has Herd.
- Horizon and Reverb were not installed into the test app.
- Every UI button was not individually exercised. Project commands were verified using the same packaged process helper and command arguments.
- Intel builds, older supported macOS releases, Developer ID signing, notarization, and distribution on another Mac were not tested.

This is a development release. See README.md for the implemented feature set and remaining limitations.
