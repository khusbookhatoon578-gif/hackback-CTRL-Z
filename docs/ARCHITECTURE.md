# ARCHITECTURE.md

## 1. System Architecture Overview
The system follows a layered monolithic architecture comprising an HTTP Express API Layer, Business Logic Layer, Middleware Stack, and Data Access Layer linked to PostgreSQL.

```mermaid
graph TD
    Client[Client App / Postman] -->|HTTP Requests| Router[Express Router / API Layer]
    Router --> AuthMW[Auth & Validation Middleware]
    AuthMW -->|Authorized| Controller[Controller Layer]
    AuthMW -->|Unauthorized| ErrResp[HTTP 401/403 Error]
    Controller --> Service[Service / Business Logic Layer]
    Service --> ORM[Prisma ORM Layer]
    ORM -->|SQL Queries| DB[(PostgreSQL Database)]