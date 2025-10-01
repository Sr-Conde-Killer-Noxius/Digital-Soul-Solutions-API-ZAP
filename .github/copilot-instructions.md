# Copilot Instructions for Evolution API

These guidelines are for AI coding agents (Copilot, Claude, Cursor, etc.) working in the Evolution API codebase. Follow these rules to maximize productivity and maintain project standards.

## Project Architecture
- **Multi-tenant WhatsApp API platform** built with Node.js 20+, TypeScript 5+, Express.js.
- **Major modules:**
  - `src/api/controllers/`: Thin HTTP route handlers
  - `src/api/services/`: Business logic (core functionality)
  - `src/api/routes/`: Express route definitions (RouterBroker pattern)
  - `src/api/integrations/`: External integrations (WhatsApp, chatbots, events, storage)
  - `src/dto/`: Data Transfer Objects (simple classes, no decorators)
  - `src/guards/`: Auth middleware
  - `src/repository/`: Prisma data access
  - `prisma/`: DB schemas/migrations (PostgreSQL & MySQL)
  - `src/validate/`: JSONSchema7 validation schemas

## Key Patterns & Conventions
- **Service Layer:** All business logic in `api/services/`, keep under 200 lines, use dependency injection.
- **Controllers:** Thin, only route/validation, delegate to services.
- **RouterBroker:** Use for all Express routes, with `dataValidate` for input validation.
- **DTOs:** Plain classes, no decorators. Use JSONSchema7 for validation.
- **Integrations:** Add new providers under `src/api/integrations/` following existing patterns.
- **Multi-tenancy:** All logic must be instance-scoped (by `instanceName` or `instanceId`).
- **Database:** Always use PrismaRepository, include `instanceId` in queries.
- **Input validation:** Use JSONSchema7, not class-validator.
- **Naming:**
  - Files: `feature.kind.ts` (e.g., `whatsapp.baileys.service.ts`)
  - Classes: PascalCase
  - Variables/functions: camelCase
  - Constants: UPPER_SNAKE_CASE
- **Code style:** 2-space indent, single quotes, trailing commas, 120-char line limit, import order via `simple-import-sort`.

## Developer Workflows
- **Dev server:** `npm run dev:server`
- **Build:** `npm run build`
- **Prod run:** `npm run start:prod`
- **Lint:** `npm run lint` (auto-fix), `npm run lint:check`
- **Conventional commits:** `npm run commit`
- **DB provider:** Set `DATABASE_PROVIDER=postgresql` or `mysql` before DB commands.
- **Migrations:** `npm run db:migrate:dev:win` (Windows), `npm run db:migrate:dev` (Unix)
- **Prisma Studio:** `npm run db:studio`
- **Docker stack:** `docker-compose -f docker-compose.dev.yaml up -d`

## Integration & Event Patterns
- **WhatsApp providers:** Baileys, Business API, Evolution API (see `src/api/integrations/channel/`)
- **Chatbots:** OpenAI, Dify, Typebot, Chatwoot, etc. (see `src/api/integrations/chatbot/`)
- **Events:** Use EventEmitter2 for internal, WebSocket/RabbitMQ/SQS/NATS/Pusher for external
- **Storage:** S3/MinIO for media (see `src/api/integrations/storage/`)

## Security & Validation
- **API key auth:** via `apikey` header
- **Guards:** Use for all protected routes
- **Rate limiting:** Enforced globally
- **Webhook signature validation:** Required for external integrations

## Communication & Documentation
- **User-facing:** Respond in Portuguese (PT-BR)
- **Code/comments:** English
- **API responses:** English
- **Error messages:** Portuguese
- **Document new endpoints, integrations, and migrations**

## Examples
- **Service:** See `src/api/services/whatsapp.baileys.service.ts`
- **Controller:** See `src/api/controllers/whatsapp.controller.ts`
- **Router:** See `src/api/routes/whatsapp.router.ts`
- **DTO:** See `src/dto/whatsapp.dto.ts` and `src/validate/whatsapp.schema.ts`

For more, see `AGENTS.md`, `.cursor/rules/`, and `README.md`.
