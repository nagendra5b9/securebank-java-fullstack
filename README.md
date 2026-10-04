# SecureBank — personal banking demo

A full-stack **simulation** for learning and portfolio use. It moves no real money and does not connect to a bank. Built with Java 21, Spring Boot, Spring Security + JWT, Maven, React, TypeScript, and MySQL.

## Run locally

Prerequisites: Docker Desktop for the full stack. For a complete Docker run, use `docker compose up --build` and open http://localhost:8081. For separate local development, use a MySQL instance you control, configure `DB_URL`, `DB_USERNAME`, and `DB_PASSWORD`, then run `mvn spring-boot:run` in `backend/` and `npm install` followed by `npm run dev` in `frontend/`. The API is at http://localhost:8080 and Swagger UI at http://localhost:8080/swagger-ui/index.html.

Set `JWT_SECRET` to a random secret of at least 32 bytes for any deployment. Defaults in `application.yml` are local-development-only. Never reuse them in production.

## First use

Register an account, sign in, and use **Add demo funds** to credit a clearly labeled simulated deposit. Open a second browser/private window and register another user to try transfers. A transfer is atomic: both balances and the ledger entries update together, and locked account rows prevent concurrent overspending. Use only fictional data.

## Included

- JWT access tokens, BCrypt passwords, Spring Security route protection, ownership checks
- Create and list accounts, simulated deposits, transfers, transaction history, monthly spending summaries
- MySQL schema via Hibernate, pessimistic row locking, transaction boundaries, validation and consistent API errors
- Responsive React dashboard, account balances, transfer form, activity list, login and registration
- Docker Compose database, OpenAPI docs, environment based configuration

## API outline

- `POST /api/auth/register` `{ "name": "A User", "email": "a@example.com", "password": "StrongPass123!" }`
- `POST /api/auth/login` `{ "email": "a@example.com", "password": "StrongPass123!" }`
- `GET /api/accounts` · `POST /api/accounts` · `POST /api/accounts/{id}/deposit`
- `GET /api/transactions?page=0&size=20` · `POST /api/transfers`
- `GET /api/analytics/monthly`

All requests other than register/login/docs require `Authorization: Bearer <token>`. Amounts are decimal currency values, positive, and rounded to two places. Demo deposits are not real payments.

## Production-minded follow-ups

For a real financial product, this demo is not sufficient. Before production: use a proper identity provider and refresh-token rotation, immutable double-entry ledger with reconciliation, idempotency keys, rate limits, fraud controls, audited admin workflows, secrets management, TLS, backups, monitoring, and security review. Do not store card or bank credentials here.
