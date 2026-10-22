# Changelog

## php-cve-2026-109618-v1.2.0 — 2026-10-22
- New: `Diagnose-PHP` ends with a **SUMMARY / SAMMANFATTNING**: verdict (NOT AFFECTED /
  PROBABLY NOT EXPOSED / AFFECTED - FIX / OK - PATCHED / CHECK MANUALLY), reasons,
  action, computer + date + OS, and one `computer;date;verdict;reasons` row to paste
  into an inventory list. Judges each PHP engine (one per folder) against the
  fixed-version table, and whether IIS (FastCGI/handlers) or Apache/nginx uses it.
- Tested on Windows Server 2022 / PS 5.1: clean machine, stray .php file, PHP on
  disk without IIS, IIS + vulnerable PHP 8.3.10, and after the fix (8.3.36 + backup).

## php-cve-2026-109618-linux-v1.0.0 — 2026-10-22
- New: `php/CVE-2026-109618_109619-linux/find-php-linux.sh` – read-only check for Linux.
  Finds every php/php-fpm/php-cgi binary, php.ini, how PHP is served (php-fpm units,
  Apache mod_php, nginx fastcgi_pass, processes), Docker containers with PHP, and
  .php files per folder.
- Distro backports detected from local package changelogs (dpkg / rpm), so e.g.
  Ubuntu 24.04's 8.3.6 or RHEL 9's 8.0.30 are reported PATCHED instead of vulnerable.
- One result per binary (OK / PATCHED / VULNERABLE / UNSUPPORTED / CHECK MANUALLY),
  summary line and exit codes 0/1/2 for automation.
- Tested on Debian 12, Ubuntu 22.04 / 24.04, Rocky 9, Fedora 41, Alpine 3.20,
  official php Docker images (8.3.36, 8.3.10), non-root, no-PHP and a host with a
  PHP container – see the README test table.

## php-cve-2026-109618-v1.1.0 — 2026-10-22
- New: **English version of the whole package.** One shared script with
  `-Lang en|sv` (default `en`); separate zips per language:
  - English: `Test-PHP.cmd`, `Fix-PHP.cmd` (confirm with YES), `Diagnose-PHP.cmd`, `README.md`
  - Svenska: `Testa-PHP.cmd`, `Atgarda-PHP.cmd` (bekräfta med JA), `Diagnos-PHP.cmd`, `README.md`
- Fix: Swedish output now uses correct spelling with å/ä/ö (was transliterated
  "saarbar", "aatgaerda"). The .ps1 source stays pure ASCII + UTF-8 BOM: the
  letters are written as tokens (`&a;` `&e;` `&o;`) and decoded at runtime.
- New: `Diagnos-PHP.cmd` / `Diagnose-PHP.cmd` double-click wrappers for the diagnosis.
- Tested on Windows Server 2022 / PS 5.1: parse check, EN + SV test, SV fix
  answered NEJ (nothing changed) and JA (8.3.10 -> 8.3.36, IIS running), EN
  re-check = OK, diagnosis in both languages, Swedish letters correct in the log file.

## php-cve-2026-109618-v1.0.2 — 2026-10-22
- New: `Diagnose-PHP.ps1` (read-only): scans all fixed disks in one pass for PHP
  binaries, php.ini and .php files (grouped per folder), plus IIS FastCGI/handlers/
  web.config, sites, and Apache/nginx/XAMPP services and processes. Included in the zip.
- Fix: PHP on disk that IIS does not use is no longer executed (file version only) -
  a hanging php-cgi.exe outside IIS cost 30 s per run.
- Fix: IIS FastCGI pointing at a missing php-cgi.exe is now reported as broken config.
- Fix: no fixed Windows build for the branch (e.g. PHP 8.1, newest Windows build 8.1.34)
  now gives a clear message instead of a bare 404; nothing is changed.
- Fix: "IIS har startats igen" was printed even when IIS was never stopped.
- Backup folders from an earlier fix (`*.old-<timestamp>`) are labelled as backups.
- Tested on Windows Server 2022 / PS 5.1 (9 scenarios): no IIS, IIS without PHP,
  two PHP installs in IIS (incl. WebPI layout), EOL 8.1, hanging php-cgi, missing
  php-cgi, fix 8.1 (no build), **full fix 8.3.10 -> 8.3.36** (SHA256, backup, php.ini
  kept, IIS up) and re-check = OK.

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
