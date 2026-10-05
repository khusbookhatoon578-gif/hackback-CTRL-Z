### File 5: `docs/API.md`

```markdown
# API.md

## Endpoint Summary Table

| Method | Endpoint Route | Description | Auth Required | Permissions | Files |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **POST** | `/api/auth/register` | Register new user | No | Public | `src/routes/auth.ts:10` |
| **POST** | `/api/auth/login` | Authenticate user & get token | No | Public | `src/routes/auth.ts:22` |
| **GET** | `/api/resources` | Fetch user's resources | Yes | Authenticated User | `src/routes/resource.ts:14` |
| **POST** | `/api/resources` | Create a new resource | Yes | Authenticated User | `src/routes/resource.ts:28` |
| **GET** | `/api/admin/users` | List all system users | Yes | Admin Only | `src/routes/admin.ts:12` |

---

## Detailed Endpoint Specifications

### 1. `POST /api/auth/register`
- **Headers:** `Content-Type: application/json`
- **Request Body:**
  ```json
  {
    "email": "user@example.com",
    "password": "SecurePassword123"
  }