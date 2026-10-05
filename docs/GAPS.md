### File 6: `docs/GAPS.md`

```markdown
# GAPS.md

## Identified Flaws & Code Vulnerabilities

| # | Flaw Description | File & Line | Affected User | Proposed Fix |
| :- | :--- | :--- | :--- | :--- |
| 1 | Unhandled asynchronous database errors crash Node process. | `src/controllers/resourceController.ts:45` | All Users | Wrap database calls in `try/catch` and forward to global error handling middleware. |
| 2 | Missing permission check on resource update endpoint. | `src/controllers/resourceController.ts:88` | Standard Users | Add ownership check (`resource.userId === req.user.id`) before updating. |
| 3 | Raw password validation accepts blank or white-space-only strings. | `src/routes/auth.ts:15` | New Users | Add minimum length (`.min(8)`) check in Zod schema. |
| 4 | SQL wildcard characters not escaped in resource search query. | `src/services/searchService.ts:22` | All Users | Escape search strings before feeding into database query builder. |
| 5 | CORS configured to accept all origin domains (`*`). | `src/index.ts:18` | All Users | Restrict CORS allowed origins to specific application domain via env configuration. |
| 6 | Sensitive user details returned in error stack traces in production mode. | `src/middleware/error.ts:12` | System / Security | Suppress stack traces when `NODE_ENV === 'production'`. |
| 7 | No payload size limits defined on request body parser. | `src/index.ts:12` | Application Server | Add `express.json({ limit: '10kb' })` middleware limit. |
| 8 | Missing rate-limiting on login endpoint allows brute-force attacks. | `src/routes/auth.ts:22` | System / Users | Implement `express-rate-limit` middleware on auth routes. |

---

## Rebuild Improvements Selected for Implementation

### Improvement 1: Missing Permission Check Fix (Addressing Flaw #2)
- **Location:** `src/controllers/resourceController.ts:88`
- **Issue:** Any authenticated user can modify or delete resources owned by other users by supplying an arbitrary resource ID parameter.
- **Rebuild Action:** Implement explicit owner verification logic in the resource update route handler to ensure that `resource.userId` matches `req.user.id`.

### Improvement 2: Request Rate Limiting & Payload Defense (Addressing Flaw #7 & #8)
- **Location:** `src/index.ts:12` & `src/routes/auth.ts:22`
- **Issue:** Authentication routes are vulnerable to brute-force credential stuffing, and endpoints lack body size constraints.
- **Rebuild Action:** Apply strict express rate-limiting (maximum 10 requests per minute) on sensitive authentication endpoints and limit JSON request body sizes to 10kb.