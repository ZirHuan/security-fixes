# Changelog

## php-cve-2026-109618-v1.0.1 — 2026-10-22
- Fix: test hung for a long time when IIS had no PHP configured. The fallback
  disk search (`Get-ChildItem 'C:\PHP*' -Recurse -Depth 2`) scanned the whole C:
  drive on Windows PowerShell 5.1 (wildcard + -Recurse ignores -Depth) and could
  pick up an unrelated php-cgi.exe anywhere on disk as a fix target. Replaced with
  a bounded check of fixed folders (C:\, Program Files, C:\tools, C:\inetpub).
- New: IIS readable but no FastCGI PHP -> "INTE SAARBAR VIA IIS" result.
- New: PHP found on disk but not used by IIS is shown as INFO, never auto-fixed.
- Fix: unreadable version no longer reported as "OK" -> "KONTROLLERA MANUELLT".
- New: 30 s timeout on `php-cgi.exe -i` (hidden error dialogs can't hang the test).
- Tested on Windows Server 2022 / PS 5.1: no IIS, IIS without PHP, IIS + PHP 8.3.10.
  Fix step (download + swap) unchanged from v1.0.0 and not re-tested in this release.

## php-cve-2026-109618-v1.0.0 — 2026-10-21
- New: Windows/IIS PHP check-and-fix tool for CVE-2026-109618 / CVE-2026-109619.
  Detects php-cgi.exe used by IIS FastCGI, compares against fixed releases
  (8.1.36 / 8.2.35 / 8.3.36 / 8.4.27 / 8.5.12), downloads the matching build from
  windows.php.net with SHA256 verification, backs up and swaps the PHP folder,
  keeps php.ini, auto-rollback on failure, transcript log.
