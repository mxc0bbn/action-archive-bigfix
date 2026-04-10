# Action Archive for BigFix Platform — v1.0.0

**First public release.** Action Archive is a self-hosted web application that collects, archives, and displays expired and stopped actions from a BigFix Root server. This is the initial general-availability release.

---

## What's in this release

### Collection and archival
- Scheduled automated collection of expired and stopped BigFix actions via the BigFix REST API
- Full action metadata captured: targets, components, scripts, relevance, results, timing
- Incremental collection — first run captures historical actions, subsequent runs collect only deltas
- Optional automatic cleanup of archived actions from the BigFix console after successful archive
- Time-partitioned target tables for efficient long-term storage and querying
- Master Operator support required for complete action coverage (see Installation Guide)

### Browse, search, and export
- Actions Browser with full-text search, multi-column filtering, sorting, and pagination
- Action Detail view showing all captured metadata for any individual action
- CSV and PDF export of search results and audit logs
- Real-time dashboard counters and collection status

### Security and access control
- Role-based access control via roles
- Configurable password policies (length, complexity, history, expiration)
- Account lockout after configurable failed login attempts
- IP-based rate limiting on authentication endpoints
- Configurable session management — idle timeout, absolute timeout, concurrent session limits
- Customizable login banner for compliance and acceptable-use notices
- API Key Management with SHA-256 hashed storage and per-key revocation
- Encrypted credential storage at rest
- HTTPS by default with TLS 1.2+ and modern cipher suites
- Self-signed certificate generated automatically; replaceable through the web UI

### Audit and compliance
- Full audit trail of all administrative actions, login attempts, and configuration changes
- Audit log export to CSV and PDF
- Backup failure events recorded in the audit log
- Pruning history table for visibility into automated retention enforcement

### Operations and administration
- Web-based system settings
- One-click database backup and restore from the web UI
- SSL certificate management through the web interface
- User email notification preferences for security and collection events
- SMTP server configuration
- Health monitoring endpoint with database, BigFix, disk, and memory status
- Configurable log levels and log retention
- Dark and light themes with persistent per-user selection

### Deployment and integration
- Single-script installer for Ubuntu 22.04 / 24.04 LTS
- Installer auto-configures PostgreSQL, Nginx, systemd, sudoers, application user, and database schema
- Installer detects existing installations and preserves database and configuration on upgrade
- Companion **Windows Audit Agent** for endpoint audit data collection (PowerShell GUI installer)
- SHA-256 manifest verifies integrity of all distributed application files at startup

---

## Prerequisites

- **Operating System:** Ubuntu Server 22.04 LTS or 24.04 LTS (amd64)
- **Privileges:** Root or sudo access on the target server
- **Network:** Internet connectivity only during installation (for package downloads); ongoing network access from the server to your BigFix Root Server's REST API
- **Resources:** Minimum 4 GB RAM, 20 GB free disk space
- **BigFix:** A dedicated BigFix Master Operator account for the collector

The installer will automatically install and configure PostgreSQL 16 and Nginx if they are not already present.

---

## How to install

```bash
wget https://github.com/mxc0bbn/action-archive-bigfix/releases/download/v1.0.0/action-archive-v1.0.0.tar.gz
tar -xzf action-archive-v1.0.0.tar.gz
cd action-archive
sudo bash install.sh
```

The installer prompts for:
1. **Admin account** — username, password, and email for the initial web admin
2. **BigFix connection** — Root Server URL, port, and operator credentials
3. **SSL certificate** — option to use an existing certificate or generate a self-signed one

After installation, browse to `https://<your-server-ip>` and sign in.

For complete installation details, prerequisites, BigFix operator setup rationale, and post-install verification steps, see the **Installation Guide PDF** included in the tarball at `action-archive/docs/ActionArchive-Installation-Guide-v1.0.0.pdf` (also attached to this release as a separate download).

---

## Release assets

| File | Description |
|---|---|
| `action-archive-v1.0.0.tar.gz` | Complete application tarball with installer, application binaries, and documentation PDFs |
| `action-archive-audit-agent-v1.0.0.zip` | Optional Windows Audit Agent installer (PowerShell GUI) |
| `ActionArchive-Installation-Guide-v1.0.0.pdf` | Installation Guide (also bundled inside the main tarball) |
| `ActionArchive-Admin-Guide-v1.0.0.pdf` | Administrator Guide (also bundled inside the main tarball) |

---

## Known limitations

- The application is designed for a single-server deployment; clustering and high-availability are not supported in v1.0.0
- BigFix REST API does not return creation date for stopped actions; date-based filtering uses last-modified time for those actions
- Initial collection of historical actions can take several minutes on first run; incremental collections are typically much faster

---

## License

Free to use and freely distributable in original, unmodified form. Modification for commercial sale or distribution is not permitted. See the `LICENSE` file in the repository or inside the tarball for the complete terms.

---

## Acknowledgments

Thank you to everyone who inspired the development, tested early builds, provided feedback on the user interface and security posture, and helped make this release possible.
