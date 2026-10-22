# Changelog

## php-cve-2026-109618-v1.0.0 — 2026-10-21
- New: Windows/IIS PHP check-and-fix tool for CVE-2026-109618 / CVE-2026-109619.
  Detects php-cgi.exe used by IIS FastCGI, compares against fixed releases
  (8.1.36 / 8.2.35 / 8.3.36 / 8.4.27 / 8.5.12), downloads the matching build from
  windows.php.net with SHA256 verification, backs up and swaps the PHP folder,
  keeps php.ini, auto-rollback on failure, transcript log.
