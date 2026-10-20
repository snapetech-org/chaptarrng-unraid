# Security scanning

Run `./scripts/security-scan-local.sh` to scan this checkout locally. Semgrep Community Edition uses the free `p/default` community rules with metrics disabled; the source stays on the machine. The rule pack is fetched from the public Semgrep Registry when it is missing or refreshed, so the first scan needs network access. For a disconnected run, set `SEMGREP_CONFIG` to a local rules directory and `SEMGREP_OFFLINE=1` after caching the Semgrep package.

This repository currently has no supported source, dependency, or deployment files, so both scanners report a skip. The wrapper is ready to scan if those artifacts are added later.

The local full scan is available on demand; no pre-commit hook is installed because this repository has no supported code/config input.

Local scans use Semgrep Community Edition and do not require a Semgrep SaaS account.

Trivy runs locally only; this checkout has no hosted Trivy workflow.

Scans report findings without failing by default so existing findings can be triaged. Set `SEMGREP_FAIL_ON_FINDINGS=1` or `TRIVY_EXIT_CODE=1` to make a local scan fail on findings. Trivy downloads its vulnerability and check databases automatically; use `TRIVY_OFFLINE=1` only after those databases are cached. The default timeout is 20 minutes for slow first downloads; set `TRIVY_TIMEOUT` to change it.

Run `./scripts/update-security-tools.sh` to refresh the local Semgrep package cache, pull the current Semgrep fallback image, and refresh Trivy's database. This checkout has no hosted Semgrep or Trivy workflow.

Semgrep uses the POSIX I/O backend by default for compatibility with restricted Linux runners. Set `SEMGREP_EIO_BACKEND` only if your runner needs a different backend.
