<div align="center">

# Hanami Infrastructure

**Production deployment, operations, and cross-project architecture for the Hanami ecosystem.**

</div>

## Purpose

This repository contains the shared configuration and operational tooling used to run Hanami projects together.

Application source code and image build workflows remain in their own repositories:

- [Hanami Bot](https://github.com/hanami-osu/bot)
- [Hanami Web](https://github.com/hanami-osu/web)
- [osu!guessr](https://github.com/hanami-osu/osu-guessr)
- [Hanami Companion](https://github.com/hanami-osu/companion)

This repository is responsible for deploying those applications, connecting their shared services, documenting production operations, and coordinating architecture that affects multiple projects.

## Responsibilities

- production Docker Compose configuration
- reverse proxy and routing configuration
- shared MariaDB, Redis, and network configuration
- deployment and rollback scripts
- database backup and restore tooling
- health checks, monitoring, and alerting
- operational runbooks
- architecture decisions affecting multiple Hanami projects
- shared account and authentication planning

## Planned structure

```text
.
├── compose/
│   ├── production.yml
│   └── .env.example
├── caddy/
│   ├── Caddyfile
│   └── snippets/
├── scripts/
│   ├── deploy.sh
│   ├── rollback.sh
│   ├── backup.sh
│   └── restore.sh
├── monitoring/
├── docs/
│   ├── architecture.md
│   ├── adr/
│   └── runbooks/
└── README.md
```

Directories should be added as they become useful. The repository should stay small and practical rather than introducing infrastructure that Hanami does not yet need.

## Deployment principles

- Application repositories build and publish versioned Docker images.
- Production deployments pin explicit image versions instead of relying only on `latest`.
- Services expose health checks where practical.
- Database migrations run as an explicit deployment step.
- Deployments must have a documented rollback path.
- Backups are only considered complete once restoration has been tested.
- Persistent data, database dumps, credentials, and production environment files are never committed.

## What does not belong here

- application source code
- production secrets or `.env` files
- database dumps and persistent volumes
- project-specific feature requests or bug reports
- Docker image build logic that belongs to an application repository

Project-specific work should be tracked in the repository responsible for that code. Issues involving multiple projects, shared deployment, authentication, infrastructure, or ecosystem-wide architecture may be tracked here.

## Initial goals

1. Document the current production topology.
2. Create a single reproducible production Compose deployment.
3. Add Caddy routing and shared network configuration.
4. Add automated database backups and a tested restore process.
5. Add deployment, health-check, and rollback scripts.
6. Document the shared Hanami account architecture and migration plan.

## Security

This repository may remain public as long as all configuration is sanitized. Production IP addresses, credentials, private keys, tokens, internal-only endpoints, and sensitive operational details must stay outside Git.

---

Hanami is an independent community project and is not affiliated with or endorsed by osu! or ppy Pty Ltd.
