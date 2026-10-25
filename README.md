# Butwal Skills Training Institute Online Admission

A self-contained ASP.NET Core 10 admissions site and administrator dashboard for Butwal Skills Training Institute.

## Run locally

```powershell
dotnet restore
dotnet run
```

Without `DATABASE_URL`, the application uses SQLite at `App_Data/admissions.db` and stores Data Protection keys beside that database. Set `ADMISSION_DB_PATH` to change the local database path. Create the first local administrator using `dotnet run -- --setup-admin`.

## Production storage and administrator

Set these environment variables in the hosting service:

- `DATABASE_URL`: PostgreSQL connection URL. The application requires TLS and uses a small connection pool (maximum 8 connections).
- `ADMIN_USERNAME`: initial administrator name. It is created only when the account does not already exist.
- `ADMIN_PASSWORD`: initial administrator password (14–256 characters). Existing credentials are never overwritten at startup.

PostgreSQL schema creation is idempotent. Applications, payments, administrator accounts, audit history, and Data Protection keys are stored in that database. Payment claims stay **Pending Verification** until an administrator verifies them. The dashboard has an authenticated CSV export for backups.

## Publish and container

The repository uses a flat layout: the solution and `AdmissionWeb.csproj` are at the root. Build with `dotnet build ButwalAdmission.sln`. Render can build the included Dockerfile; the container listens on port 10000 and runs as a non-root user. Never commit database URLs, passwords, or environment files.

See [DEPLOY_GUIDE.md](DEPLOY_GUIDE.md) for the GitHub, Neon, and Render setup steps. Audit notes are in [REVIEW_AND_REPAIR.md](REVIEW_AND_REPAIR.md).
