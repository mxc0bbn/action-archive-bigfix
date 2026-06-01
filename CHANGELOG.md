# Changelog

All notable changes to Action Archive for BigFix Platform are documented here.
The format follows [Keep a Changelog](https://keepachangelog.com/).

For full release notes — upgrade instructions, assets, checksums — see the
[Releases page](https://github.com/mxc0bbn/action-archive-bigfix/releases).

## [v1.2.2] - 2026-06-01

### Security
- The login form submits over POST and is bound to its client-side handler, so account credentials are never placed in a URL even if the page's scripts fail to load.
- HTTP access logs record the request path without its query string.

### Changed
- Email test and send failures now include the mail server's own response text, making a misconfigured SMTP account far easier to diagnose.

### Fixed
- The "Password Reset Requests" toggle under Administration > System Settings > Notifications now controls whether the self-service password reset email is sent. When disabled, users who request a reset receive the same generic confirmation, no email is sent, and the request is recorded in the audit log. With the toggle disabled, self-service reset is effectively turned off and an administrator resets passwords from User Management. The toggle defaults to Enabled.

## [v1.2.1] - 2026-05-28

### Added
- New `APP_BASE_URL` setting in `.env`. Populated automatically by the installer from the server's primary IP address. Used to build outbound password reset email links so they point at the canonical server URL even when the inbound HTTP request carries a forged `Host` header. Edit this in `/opt/bigfix-archive/config/.env` if the server is normally accessed at a different hostname.
- New `/api/admin/health` endpoint that returns the detailed health view (BigFix server URL, hostname, OS, Python version, database size, memory). Requires administrator authentication.
- Archived Actions list: click anywhere on a row to open the action; filters and the text search apply automatically as you change them.
- Page layout caps content width on wide displays for more comfortable reading and consistent column alignment.
- BigFix Server Trust section under Administration > System Settings > Connections. Captures the BigFix server's TLS certificate, shows its subject, issuer, validity, and SHA-256 fingerprint for review, and adds it to the system trust store on confirmation. Removes the need to run `openssl` and `update-ca-certificates` by hand when enabling "Verify SSL Certificate = Yes" against a BigFix server with an internal CA.
- Password Changed notification: the user receives an email whenever their password is changed, regardless of the path (self-service reset via emailed link, logged-in password change, forced change after admin reset, or administrator-initiated reset from User Management). Includes time, source IP, and method. Toggle under Administration > System Settings > Notifications; users can also opt out for their own account from My Account > Notification Preferences.

### Changed
- `/api/health` (unauthenticated) now returns only the overall status indicator, application version, and build date. The detailed system information that was previously included on this endpoint is available on the new `/api/admin/health` endpoint.
- The cert-management helper used by the SSL Certificate upload feature is installed at `/usr/local/sbin/bigfix-archive-install-cert`, owned by root, with the application user only able to invoke it via the existing sudoers rule. Upgrades from v1.2.0 remove the prior `/opt/bigfix-archive/scripts/install-cert.sh` automatically.

### Fixed
- Password reset email links are built from `APP_BASE_URL` instead of the inbound `Host` and `X-Forwarded-Proto` headers.
- CSV and Excel exports of actions, targets, audit log, application log, collection log, and PostgreSQL log sanitize cell values that would otherwise be interpreted as spreadsheet formulas when opened in Excel or LibreOffice Calc.
- Constant-time comparison is used for the provenance verification token check.
- Outbound HTTPS to the BigFix server now honors CAs added to the system trust store via `update-ca-certificates`, so "Verify SSL Certificate = Yes" works against BigFix servers using an internal CA.
- "Verify SSL Certificate = Yes" works against BigFix servers that present only a leaf certificate (the common configuration), on both Ubuntu 24.04 and 26.04. Previously, only 26.04 accepted leaf-only chains by default.
- Save BigFix Settings refuses an empty or scheme-less Server URL and returns a clear error, preventing a corrupted setting that would make every subsequent Test Connection report a misleading protocol error.

## [v1.2.0] - 2026-05-26

### Added
- Server Migration: export the application database, encrypted BigFix and SMTP credentials, and configuration to a single passphrase-protected file (.aa-mig), and import it on a different Action Archive server. Supports moving a deployment to new hardware or to a newer Ubuntu release without losing data or having to rebuild configuration by hand.
- Ubuntu Server 26.04 LTS support. The same installer package works on both supported releases; it detects the host's Python version and selects the matching compiled modules automatically.

### Changed
- Minimum supported Ubuntu version is now 24.04 LTS. Support for 22.04 has been removed.

### Fixed
- Web UI Backup download and Restore actions no longer fail silently. They previously returned an authentication error caused by a token-key mismatch.

## [v1.1.4] - 2026-05-18

### Added
- Pre-install prerequisite check: the installer verifies the tools it needs and its network access, lists every missing item and connectivity problem at once, and offers to install the missing packages before continuing.
- BigFix connection is verified during installation (reachability and authentication) before the install proceeds, with an option to re-enter the details, continue anyway, or abort.

### Changed
- Web admin and BigFix operator password prompts now show masked feedback and require confirmation.
- Admin email is optional at installation and can be set later in the web interface.

### Fixed
- Passwords containing special characters, including a single quote, are handled correctly during installation.

## [v1.1.3] - 2026-05-15

### Added
- Performance page under Administration > Logs: at-a-glance health gauges for the system, web server, database, and collection pipeline, with detail panels, a network throughput chart, and an in-place explanation of every metric.
- Application version shown on the login screen and in the header.
- Guided PostgreSQL tuning during install, sized to the server's detected memory, CPU count, and storage type. Optional and declinable; existing tuning is preserved on upgrade.

### Fixed
- Installer now places the bundled BigFix Audit Agent package, so the download card appears on a fresh install (regressed in v1.1.2).
- Installer now installs the certificate helper and its sudoers rule, so SSL certificate upload works on a fresh install (regressed in v1.1.2).
- Installer upgrade mode restored: re-running over an existing install preserves configuration, credentials, database, SSL certificate, and the admin account, and reports the installed version before you choose (present in v1.1.0, lost in v1.1.2).
- Database health score recalibrated for Action Archive's normal query patterns.

## [v1.1.2] — 2026-04-21

### Added
- OpenAPI 3.x specification (`docs/openapi.json`) bundled with the install and published in the repository, for programmatic API discovery and client generation.
- Installer prompt for BigFix authentication method (password or API token).

### Fixed
- Installer now correctly deploys the `docs/` directory so integrity verification succeeds on first run.
- Installer banner displays the current release version.
- Admin Guide Appendix A rewritten as a pointer to the OpenAPI specification; authentication mechanisms described accurately.

## [v1.1.1] — 2026-04-16

### Added
- Targeted email notifications: event emails go to the initiating user, not all recipients.
- Console state reconciliation: each collection flags archived actions no longer present in the BigFix Console.
- Console filter dropdown on the Actions list (Any / Still in Console / Removed from Console).
- Multi-Action Group child handling: child actions cannot be individually deleted; delete the parent group instead.

### Fixed
- Completed collections now send an email notification (previously only failures did).
- Single-column Session Relevance responses parsed correctly when `<Tuple>` wrappers are absent.

## [v1.1.0] — 2026-04-16

### Added
- BigFix API token authentication as an alternative to operator username/password.
- Installer upgrade mode that preserves `.env`, database, SSL certificate, admin users, and settings on re-run.
- Bundled BigFix Audit Agent installer, downloadable from Administration → System Settings → API Keys. No internet access required post-install.

## [v1.0.2] — 2026-04-14

### Fixed
- Certificate upload: helper script and sudoers rule were missing from the tarball on fresh installs, preventing `POST /api/admin/certificate/upload` from completing.

## [v1.0.1] — 2026-04-13

### Fixed
- Collection could fail with `InFailedSqlTransaction` on target rows with missing computer names. Substitutes a stable placeholder, rolls back per-action errors, and relaxes the `NOT NULL` constraint.

### Added
- CSV export on Collection, Application, Nginx, and PostgreSQL log views.

## [v1.0.0] — 2026-04-13

Initial public release.

[v1.2.2]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.2.2
[v1.2.1]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.2.1
[v1.2.0]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.2.0
[v1.1.4]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.1.4
[v1.1.3]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.1.3
[v1.1.2]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.1.2
[v1.1.1]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.1.1
[v1.1.0]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.1.0
[v1.0.2]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.0.2
[v1.0.1]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.0.1
[v1.0.0]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.0.0
