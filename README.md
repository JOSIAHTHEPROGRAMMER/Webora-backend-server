# Webora Backend Server

[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-43853D?style=flat-square&logo=node.js&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express-000000?style=flat-square&logo=express&logoColor=white)](https://expressjs.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat-square&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=flat-square&logo=prisma&logoColor=white)](https://www.prisma.io/)

**The backend API server powering Webora's AI-powered website builder**

---

## Table of Contents

- [About](#about)
- [Tech Stack](#tech-stack)
- [Features](#features)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)
- [Database Schema](#database-schema)
- [Deployment](#deployment)
- [Configuration](#configuration)

---

## About

This repository contains the **backend server** for Webora, an AI-powered website builder. The server handles user authentication, project management, AI integration for website generation, payment processing through Stripe, and provides a RESTful API for the frontend client.

For the frontend application, visit [Webora](https://github.com/JOSIAHTHEPROGRAMMER/Webora).

### Key Capabilities

- RESTful API built with Express and TypeScript
- Secure authentication with Better Auth
- AI-powered website generation using OpenRouter
- PostgreSQL database with Prisma ORM
- Stripe integration for payment processing
- Credit-based system for AI generations
- Version control for website projects
- Publishing and sharing functionality

---

## Tech Stack

### Core Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| **Node.js** | ES2020 | Runtime Environment |
| **Express** | 5.2.1 | Web Framework |
| **TypeScript** | 5.9.3 | Type Safety |
| **Prisma** | 7.2.0 | ORM & Database Client |
| **PostgreSQL** | Latest | Database (via Neon) |

### Authentication & Security

| Technology | Version | Purpose |
|------------|---------|---------|
| **Better Auth** | 1.4.16 | Authentication System |
| **bcryptjs** | 2.4.3 | Password Hashing |
| **CORS** | 2.8.5 | Cross-Origin Resource Sharing |

### External Services

| Technology | Version | Purpose |
|------------|---------|---------|
| **OpenAI SDK** | 6.16.0 | AI Integration |
| **Stripe** | 20.2.0 | Payment Processing |
| **Neon** | - | Serverless PostgreSQL |

---

## Features

### Authentication System

- Session-based authentication with Better Auth
- Email and password authentication
- Secure session management with httpOnly cookies
- Email verification support
- User account management

### Project Management

- Create unlimited website projects
- Store and manage project code
- Conversation history tracking
- Version control for all projects
- Publish/unpublish functionality
- Project deletion and cleanup

### AI Integration

- OpenRouter integration for AI-powered generation
- Support for multiple AI models (Mistral Devstral)
- Intelligent prompt processing and enhancement
- Automatic Tailwind CSS styling
- Revision and iteration capabilities

### Credit System

- User credit balance management
- Credit deduction for project creation and revisions
- Credit purchase through Stripe
- Transaction history tracking
- Multiple pricing tiers

### Payment Processing

- Stripe checkout integration
- Webhook handling for payment confirmation
- Automatic credit allocation
- Secure payment processing
- PCI compliance

### Version Control

- Automatic version creation for AI revisions
- Version history with timestamps
- Rollback to any previous version
- Version metadata and descriptions

---

## Getting Started

### Prerequisites

Ensure you have the following installed:

- **Node.js**: v18.0.0 or higher
- **npm**: v9.0.0 or higher
- **PostgreSQL**: Or a Neon account (recommended)
- **Git**: Latest version

### Required API Keys

You'll need accounts and API keys for:

1. **Neon** (Database): [console.neon.tech](https://console.neon.tech)
2. **OpenRouter** (AI): [openrouter.ai](https://openrouter.ai)
3. **Stripe** (Payments): [dashboard.stripe.com](https://dashboard.stripe.com)

### Installation

#### Step 1: Clone the Repository

```bash
git clone https://github.com/JOSIAHTHEPROGRAMMER/Webora-backend-server.git
cd Webora-backend-server
```

#### Step 2: Install Dependencies

```bash
npm install
```

#### Step 3: Configure Environment Variables

Create a `.env` file in the root directory:

```env
# Database
DATABASE_URL="postgresql://user:password@host:port/database?sslmode=require"

# Authentication
BETTER_AUTH_SECRET="your-random-secret-key-min-32-chars"
BETTER_AUTH_URL="http://localhost:3000"

# CORS
TRUSTED_ORIGINS="http://localhost:5173"

# Environment
NODE_ENV="development"

# AI
AI_API_KEY="your-openrouter-api-key"

# Payments
STRIPE_SECRET_KEY="sk_test_your_stripe_secret_key"
STRIPE_WEBHOOK_SECRET="whsec_your_webhook_secret"
```

**Generate Secrets**:
```bash
# Generate BETTER_AUTH_SECRET
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

#### Step 4: Setup Database

```bash
# Generate Prisma Client
npx prisma generate

# Run migrations
npx prisma migrate dev

# (Optional) Seed database
npx prisma db seed
```

#### Step 5: Start Development Server

```bash
npm run dev
```

The server will be available at `http://localhost:3000`

---

## Project Structure

```
webora-backend-server/
├── configs/
│   └── openai.ts              # OpenAI/OpenRouter client setup
├── controllers/
│   ├── ProjectController.ts   # Project management logic
│   ├── userController.ts      # User and credit management
│   └── stripeWebhook.ts       # Stripe webhook handler
├── generated/
│   └── client/                # Prisma generated client
├── lib/
│   ├── auth.ts                # Better Auth configuration
│   └── prisma.ts              # Prisma client instance
├── middlewares/
│   └── auth.ts                # Authentication middleware
├── prisma/
│   ├── schema.prisma          # Database models
│   └── migrations/            # Migration history
├── routes/
│   ├── userRoutes.ts          # User-related routes
│   └── projectRoutes.ts       # Project-related routes
├── types/
│   └── express.d.ts           # TypeScript type definitions
├── server.ts                  # Express app entry point
├── package.json
├── tsconfig.json
├── vercel.json                # Vercel deployment config
└── .env                       # Environment variables
```

---

## API Documentation

### Base URL

**Development**: `http://localhost:3000`
**Production**: `https://your-backend-url.vercel.app`

### Authentication Endpoints

All authentication is handled through Better Auth:

#### Sign Up
```http
POST /api/auth/sign-up
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword",
  "name": "John Doe"
}
```

#### Sign In
```http
POST /api/auth/sign-in
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword"
}
```

**Response**:
```json
{
  "session": {
    "token": "session-token",
    "userId": "user-id"
  },
  "user": {
    "id": "user-id",
    "email": "user@example.com",
    "name": "John Doe"
  }
}
```

#### Sign Out
```http
POST /api/auth/sign-out
```

#### Get Session
```http
GET /api/auth/session
```

**Response**:
```json
{
  "session": {
    "userId": "user-id",
    "expiresAt": "2026-02-01T00:00:00.000Z"
  },
  "user": {
    "id": "user-id",
    "email": "user@example.com",
    "name": "John Doe"
  }
}
```

---

### User Endpoints

All user endpoints require authentication.

#### Get User Credits
```http
GET /api/user/credits
Authorization: Bearer <session-token>
```

**Response**:
```json
{
  "credits": 15
}
```

#### Create New Project
```http
POST /api/user/project
Authorization: Bearer <session-token>
Content-Type: application/json

{
  "initial_prompt": "Create a modern landing page for a SaaS product with pricing section"
}
```

**Response**:
```json
{
  "projectId": "clx1234567890abcdef"
}
```

**Cost**: 5 credits

#### Get Single Project
```http
GET /api/user/project/:projectId
Authorization: Bearer <session-token>
```

**Response**:
```json
{
  "project": {
    "id": "clx1234567890abcdef",
    "name": "SaaS Landing Page",
    "initial_prompt": "Create a modern landing page...",
    "current_code": "<!DOCTYPE html>...",
    "current_version_index": "clx0987654321fedcba",
    "isPublished": false,
    "userId": "user-id",
    "createdAt": "2026-01-20T10:30:00.000Z",
    "updatedAt": "2026-01-20T10:35:00.000Z",
    "conversation": [
      {
        "id": "conv-1",
        "role": "user",
        "content": "Create a modern landing page...",
        "timestamp": "2026-01-20T10:30:00.000Z"
      },
      {
        "id": "conv-2",
        "role": "assistant",
        "content": "<!DOCTYPE html>...",
        "timestamp": "2026-01-20T10:30:15.000Z"
      }
    ],
    "versions": [
      {
        "id": "ver-1",
        "code": "<!DOCTYPE html>...",
        "description": "Initial version",
        "timestamp": "2026-01-20T10:30:15.000Z"
      }
    ]
  }
}
```

#### Get All Projects
```http
GET /api/user/projects
Authorization: Bearer <session-token>
```

**Response**:
```json
{
  "projects": [
    {
      "id": "project-1",
      "name": "Project Name",
      "initial_prompt": "Create a...",
      "isPublished": false,
      "createdAt": "2026-01-20T10:30:00.000Z",
      "updatedAt": "2026-01-20T10:35:00.000Z"
    }
  ]
}
```

#### Toggle Publish Status
```http
GET /api/user/publish-toggle/:projectId
Authorization: Bearer <session-token>
```

**Response**:
```json
{
  "isPublished": true
}
```

#### Purchase Credits
```http
POST /api/user/purchase-credits
Authorization: Bearer <session-token>
Content-Type: application/json

{
  "planId": "basic" | "pro" | "enterprise"
}
```

**Response**:
```json
{
  "url": "https://checkout.stripe.com/c/pay/..."
}
```

**Pricing Plans**:
- **basic**: 50 credits for $9.99
- **pro**: 120 credits for $19.99
- **enterprise**: 300 credits for $39.99

---

### Project Endpoints

All project endpoints require authentication.

#### Make Revision
```http
POST /api/project/revision/:projectId
Authorization: Bearer <session-token>
Content-Type: application/json

{
  "message": "Make the header blue and add a contact form"
}
```

**Response**:
```json
{
  "success": true,
  "message": "Revision completed"
}
```

**Cost**: 5 credits

#### Save Project Code
```http
PUT /api/project/save/:projectId
Authorization: Bearer <session-token>
Content-Type: application/json

{
  "code": "<!DOCTYPE html>..."
}
```

**Response**:
```json
{
  "success": true,
  "message": "Project saved successfully"
}
```

#### Rollback to Version
```http
GET /api/project/rollback/:projectId/:versionId
Authorization: Bearer <session-token>
```

**Response**:
```json
{
  "success": true,
  "message": "Rollback successful"
}
```

#### Delete Project
```http
DELETE /api/project/:projectId
Authorization: Bearer <session-token>
```

**Response**:
```json
{
  "success": true,
  "message": "Project deleted successfully"
}
```

#### Get Project Preview
```http
GET /api/project/preview/:projectId
Authorization: Bearer <session-token>
```

**Response**:
```json
{
  "code": "<!DOCTYPE html>..."
}
```

---

### Public Endpoints

These endpoints do not require authentication.

#### Get Published Projects
```http
GET /api/project/published
```

**Response**:
```json
{
  "projects": [
    {
      "id": "project-1",
      "name": "Project Name",
      "initial_prompt": "Create a...",
      "createdAt": "2026-01-20T10:30:00.000Z"
    }
  ]
}
```

#### Get Published Project by ID
```http
GET /api/project/published/:projectId
```

**Response**:
```json
{
  "project": {
    "id": "project-1",
    "name": "Project Name",
    "current_code": "<!DOCTYPE html>...",
    "initial_prompt": "Create a..."
  }
}
```

---

### Webhook Endpoints

#### Stripe Webhook
```http
POST /api/stripe
Content-Type: application/json
Stripe-Signature: <signature-header>
```

**Handles Events**:
- `payment_intent.succeeded`: Adds credits to user account

---

## Database Schema

### User Model
```prisma
model User {
  id            String    @id @default(cuid())
  email         String    @unique
  name          String
  emailVerified Boolean   @default(false)
  totalCreation Int       @default(0)
  credits       Int       @default(20)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  
  projects      WebsiteProject[]
  sessions      Session[]
  accounts      Account[]
  transactions  Transaction[]
}
```

### WebsiteProject Model
```prisma
model WebsiteProject {
  id                    String   @id @default(cuid())
  name                  String
  initial_prompt        String   @db.Text
  current_code          String?  @db.Text
  current_version_index String   @default("")
  userId                String
  isPublished           Boolean  @default(false)
  createdAt             DateTime @default(now())
  updatedAt             DateTime @updatedAt
  
  conversation Conversation[]
  versions     Version[]
  user         User           @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  @@index([userId])
}
```

### Conversation Model
```prisma
model Conversation {
  id        String   @id @default(cuid())
  role      Role
  content   String   @db.Text
  timestamp DateTime @default(now())
  projectId String
  
  project WebsiteProject @relation(fields: [projectId], references: [id], onDelete: Cascade)
  
  @@index([projectId])
}

enum Role {
  user
  assistant
}
```

### Version Model
```prisma
model Version {
  id          String   @id @default(cuid())
  code        String   @db.Text
  description String?
  timestamp   DateTime @default(now())
  projectId   String
  
  project WebsiteProject @relation(fields: [projectId], references: [id], onDelete: Cascade)
  
  @@index([projectId])
}
```

### Transaction Model
```prisma
model Transaction {
  id        String   @id @default(cuid())
  isPaid    Boolean  @default(false)
  planId    String
  amount    Float
  credits   Int
  userId    String
  createdAt DateTime @default(now())
  
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  @@index([userId])
}
```

### Session Model
```prisma
model Session {
  id        String   @id
  expiresAt DateTime
  token     String   @unique
  userId    String
  
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  @@index([userId])
}
```

### Account Model
```prisma
model Account {
  id           String  @id @default(cuid())
  accountId    String
  providerId   String
  userId       String
  accessToken  String? @db.Text
  refreshToken String? @db.Text
  
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  @@index([userId])
}
```

---

## Deployment

### Deploy to Vercel

#### Step 1: Prepare for Deployment

Ensure your environment variables are configured for production:

```env
DATABASE_URL="postgresql://user:password@host:port/database?sslmode=require"
BETTER_AUTH_SECRET="<generate-new-secret-for-production>"
BETTER_AUTH_URL="https://your-backend.vercel.app"
TRUSTED_ORIGINS="https://your-frontend.vercel.app"
NODE_ENV="production"
AI_API_KEY="<your-openrouter-api-key>"
STRIPE_SECRET_KEY="<your-stripe-live-key>"
STRIPE_WEBHOOK_SECRET="<your-webhook-secret>"
```

#### Step 2: Deploy via Vercel CLI

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel
```

#### Step 3: Deploy via Vercel Dashboard

1. Go to [vercel.com/new](https://vercel.com/new)
2. Import your GitHub repository
3. Configure project settings:
   - **Framework Preset**: Other
   - **Root Directory**: `/`
   - **Build Command**: (leave empty)
   - **Output Directory**: (leave empty)
4. Add all environment variables listed above
5. Click **Deploy**

#### Step 4: Run Database Migrations

```bash
# Set production database URL
DATABASE_URL="your-production-url" npx prisma migrate deploy

# Generate Prisma Client
npx prisma generate
```

#### Step 5: Configure Stripe Webhook

1. Go to [Stripe Dashboard → Webhooks](https://dashboard.stripe.com/webhooks)
2. Click **Add endpoint**
3. Enter endpoint URL: `https://your-backend.vercel.app/api/stripe`
4. Select events to listen to:
   - `payment_intent.succeeded`
5. Copy the signing secret
6. Update `STRIPE_WEBHOOK_SECRET` in Vercel environment variables
7. Redeploy the application

#### Step 6: Update CORS Settings

Update `TRUSTED_ORIGINS` with your production frontend URL and redeploy.

---

## Configuration

### Environment Variables

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `DATABASE_URL` | PostgreSQL connection string | Yes | - |
| `BETTER_AUTH_SECRET` | Secret key for session encryption (min 32 chars) | Yes | - |
| `BETTER_AUTH_URL` | Base URL of the backend server | Yes | - |
| `TRUSTED_ORIGINS` | Allowed CORS origins (comma-separated) | Yes | - |
| `NODE_ENV` | Environment mode | No | development |
| `AI_API_KEY` | OpenRouter API key | Yes | - |
| `STRIPE_SECRET_KEY` | Stripe secret key | Yes | - |
| `STRIPE_WEBHOOK_SECRET` | Stripe webhook signing secret | Yes | - |

### Prisma Configuration

The Prisma schema is located at `prisma/schema.prisma`. To modify the database schema:

1. Edit `schema.prisma`
2. Create a migration:
   ```bash
   npx prisma migrate dev --name your_migration_name
   ```
3. Apply to production:
   ```bash
   npx prisma migrate deploy
   ```

### OpenRouter Configuration

Located in `configs/openai.ts`:

```typescript
const openai = new OpenAI({
  baseURL: "https://openrouter.ai/api/v1",
  apiKey: process.env.AI_API_KEY,
});
```

Default model: `mistralai/mistral-devstral`

### Better Auth Configuration

Located in `lib/auth.ts`:

```typescript
export const auth = betterAuth({
  database: prisma,
  emailAndPassword: {
    enabled: true,
  },
  trustedOrigins: process.env.TRUSTED_ORIGINS?.split(",") || [],
  secret: process.env.BETTER_AUTH_SECRET,
});
```

---

## Available Scripts

```bash
# Start development server
npm run dev

# Build for production
npm run build

# Start production server
npm start

# Generate Prisma Client
npx prisma generate

# Run database migrations
npx prisma migrate dev

# Deploy migrations to production
npx prisma migrate deploy

# Open Prisma Studio (database GUI)
npx prisma studio

# Seed database
npx prisma db seed
```

---

## Development Workflow

1. **Start the development server**: `npm run dev`
2. **Make changes** to controllers, routes, or models
3. **Test endpoints** using Postman or curl
4. **Update database schema** if needed with Prisma migrations
5. **Commit changes** with meaningful messages

---

## Error Handling

The API uses standard HTTP status codes:

- **200**: Success
- **201**: Created
- **400**: Bad Request
- **401**: Unauthorized
- **403**: Forbidden
- **404**: Not Found
- **500**: Internal Server Error

Error responses follow this format:

```json
{
  "error": "Error message description"
}
```

---

## Security

### Best Practices Implemented

- Session-based authentication with httpOnly cookies
- Password hashing with bcrypt
- CORS protection with trusted origins
- SQL injection protection via Prisma ORM
- Environment variable protection
- Stripe webhook signature verification
- Rate limiting (recommended for production)
- HTTPS enforcement in production

---

## Links

- **Frontend Repository**: [Webora](https://github.com/JOSIAHTHEPROGRAMMER/Webora)
- **Backend Repository**: [Backend-Server](https://github.com/JOSIAHTHEPROGRAMMER/Webora-backend-server)

---

## Acknowledgments

This project is built with the following technologies:

- [Node.js](https://nodejs.org/) - Runtime Environment
- [Express](https://expressjs.com/) - Web Framework
- [TypeScript](https://www.typescriptlang.org/) - Type Safety
- [Prisma](https://www.prisma.io/) - ORM
- [PostgreSQL](https://www.postgresql.org/) - Database
- [Better Auth](https://github.com/better-auth/better-auth) - Authentication
- [OpenRouter](https://openrouter.ai/) - AI API Gateway
- [Stripe](https://stripe.com/) - Payment Processing
- [Vercel](https://vercel.com/) - Deployment Platform
- [Neon](https://neon.tech/) - Serverless PostgreSQL

---

**Webora Backend Server** - Powering AI-driven web creation.
