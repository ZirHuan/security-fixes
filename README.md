# security-fixes

Small, self-contained tools that **check for** and **remediate** specific security
vulnerabilities. One folder per fix, named `<product>/<CVE-id(s)>-<platform>`.
Each folder has its own README with usage, what it changes, and how to roll back.

> Use at your own risk. Every tool takes a backup and asks for confirmation
> before changing anything, but test on a non-production system first.

## Index

| Product | CVE | Platform | Severity | Tool | Status |
|---|---|---|---|---|---|
| PHP | CVE-2026-109618, CVE-2026-109619 (unauthenticated RCE) | Windows / IIS (FastCGI) | Critical, CVSS 9.2 | [php/CVE-2026-109618_109619-win-iis](php/CVE-2026-109618_109619-win-iis/) | v1.1.0 · [⬇ English](https://github.com/ZirHuan/security-fixes/releases/download/php-cve-2026-109618-v1.1.0/PHP-IIS-Fix-CVE-2026-109618-109619-v1.1.0-en.zip) · [⬇ Svenska](https://github.com/ZirHuan/security-fixes/releases/download/php-cve-2026-109618-v1.1.0/PHP-IIS-Fix-CVE-2026-109618-109619-v1.1.0-sv.zip) |

## Conventions

- **Check first, fix second.** Every tool has a read-only test mode; fixing is opt-in.
- **Backup + rollback.** Anything replaced is backed up first and restored on failure.
- **Verify downloads.** Vendor checksums (SHA256) are checked before installing.
- **Log everything.** Each run writes a timestamped log.
- **Versioning.** Tags are prefixed per fix, e.g. `php-cve-2026-109618-v1.0.0`.
- **No environment data.** No hostnames, IPs, customer names or credentials in this repo.
- PowerShell scripts are pure ASCII + UTF-8 BOM so they parse on Windows PowerShell 5.1.

**Easy download:** every fix is published as a ready-to-run zip under
[Releases](https://github.com/ZirHuan/security-fixes/releases) — unzip and double-click.

New fix? Copy [`_template/`](_template/) and add a row to the index.
