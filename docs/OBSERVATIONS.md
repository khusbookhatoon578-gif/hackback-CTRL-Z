# OBSERVATIONS.md

## 1. Core Technology Stack
- **Language / Runtime:** Node.js (v18.16.0) (`package.json:12`)
- **Web Framework:** Express.js (v4.18.2) (`package.json:18`)
- **Database / ORM:** PostgreSQL (v15.0) via Prisma ORM (v4.14.0) (`prisma/schema.prisma:1`, `package.json:22`)
- **Authentication:** JSON Web Tokens (jsonwebtoken v9.0.0) (`package.json:25`)
- **Validation:** Zod (v3.21.4) (`package.json:29`)

## 2. Local Environment & Setup Instructions
### Required Environment Variables
- `PORT` (e.g., Application port)
- `DATABASE_URL` (e.g., PostgreSQL connection string)
- `JWT_SECRET` (e.g., Token signing secret)
- `NODE_ENV` (e.g., `development` or `production`)

### Local Execution Commands
```bash
# Install dependencies
npm install

# Apply database migrations
npx prisma migrate dev

# Seed database (if applicable)
npm run seed

# Run local development server
npm run dev