### File 2: `docs/PRD.md`

```markdown
# PRD.md (Product Requirements Document)

## 1. Problem Statement
Users require an intuitive, reliable system to manage entity workflows with strict authorization boundaries, reliable data consistency, and auditability. The existing system contains edge-case vulnerabilities in input handling and route permissions.

## 2. User Roles & Capabilities
- **Guest / Unauthenticated User:**
  - View public landing page (`/`).
  - Access registration and login routes (`/api/auth/register`, `/api/auth/login`).
- **Standard User:**
  - Authenticate and manage self-owned resources (`/api/resources`).
  - View personal dashboard and update account settings.
- **Administrator:**
  - Access administrative endpoints (`/api/admin/*`).
  - Manage user roles, override resource locks, and execute system diagnostics.

## 3. Core User Flow (Step-by-Step)
1. **Authentication:** User submits credentials at `/api/auth/login`. System verifies password hash and issues a signed JWT.
2. **Resource Fetch:** User sends GET request to `/api/resources` with `Authorization: Bearer <token>`.
3. **Validation & Auth Check:** Auth middleware decodes token, assigns user context, and checks permissions.
4. **Execution & Response:** Controller queries PostgreSQL database via Prisma ORM and returns JSON payload.

## 4. Requirement Prioritization (MoSCoW)

### Must Have (Core Rebuild)
- Complete JWT authentication flow (Login, Register, Token Verification).
- Full CRUD operations on primary domain entities (`User`, `Resource`).
- Strict schema validation on all POST/PUT inputs using Zod.
- Implementation of acceptance criteria covering all 3 Killer Tests.

### Should Have
- Global centralized error handling middleware.
- Comprehensive health check endpoint (`/api/health`).

### Could Have
- Rate limiting middleware for public endpoints.
- Structured JSON logging.

### Won't Have (Out of Scope for Rebuild)
- Third-party OAuth integrations (Google/GitHub login).
- Websocket live updates.
- Microservices decomposition (Must remain a clean monolithic architecture).

## 5. Killer Test Acceptance Criteria

### Killer Test 1: Unique Constraint & Duplicate Prevention
- **Given** an existing user registered with email `user@example.com`,
- **When** a registration request is sent with the identical email `user@example.com`,
- **Then** the API must reject the request with HTTP status `409 Conflict` and return a structured JSON error body `{ "error": "Email already exists" }`.

### Killer Test 2: Unauthorized Route Access Block
- **Given** an authenticated user with role `Standard User`,
- **When** sending a GET request to `/api/admin/users`,
- **Then** the API must reject the request with HTTP status `403 Forbidden` and return `{ "error": "Insufficient permissions" }`.

### Killer Test 3: Data Integrity on Cascading Delete
- **Given** an existing `Resource` record associated with a `User`,
- **When** the `User` account is deleted,
- **Then** dependent `Resource` records must be safely handled according to foreign key constraints (`ON DELETE CASCADE`) without orphan records remaining.