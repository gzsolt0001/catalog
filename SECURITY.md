# Security Policy

## Scope

This repository contains generated templates, metadata, and supporting configuration files used for repository scaffolding and automation purposes.

The repository itself does not provide hosted services or production runtime components.


## Supported Versions

Only the current `main` branch is considered supported.

| Version | Supported |
| --- | --- |
| main | ✅ |
| actual | ✅ |

## Reporting a Vulnerability

Please do not report security vulnerabilities through public GitHub issues, discussions, or pull requests.

Questions, comments, and security-related reports regarding this repository may be
submitted through the [official NAV contact form](https://nav.gov.hu/ugyfeliranytu/keressen_minket/levelkuldes?subjectid=92).

When submitting a report, please include:
- affected template or component,
- reproduction steps,
- potential impact,
- suggested mitigation (if available).

We will make reasonable efforts to acknowledge reports in a timely manner.

## Generated Repository Responsibility

Repositories generated from these templates may introduce additional components,
dependencies, workflows, or configurations maintained independently by downstream project owners.

Security issues affecting generated repositories are considered in scope only if the root cause originates from this template repository or its generation process.

## Sensitive Data Handling

Do not submit:
- taxpayer data,
- personal data,
- credentials,
- API keys,
- production secrets,
- confidential operational information

through issues, pull requests, discussions, or example configurations.

## Supply Chain and Dependency Considerations

Maintainers should regularly review:
- referenced GitHub Actions,
- third-party dependencies,
- container image references,
- automation permissions,
- default security settings.

Consumers of generated repositories are responsible for validating generated outputs before production usage.