# Chat SDK - GitHub Copilot Instructions

## Project Overview

This is an AI chatbot application built with Next.js and the AI SDK. The application serves as a template for building powerful chatbot applications with modern web technologies.

## Tech Stack

- **Frontend Framework**: Next.js 15+ (App Router)
- **UI Component Library**: shadcn/ui with Tailwind CSS and Radix UI primitives
- **State Management**: React hooks and SWR for data fetching
- **AI Integration**: AI SDK for LLM interaction
- **Authentication**: NextAuth.js (Auth.js)
- **Database**: PostgreSQL via Neon (serverless postgres)
- **File Storage**: Vercel Blob
- **Animation**: Framer Motion
- **Styling**: Tailwind CSS
- **Code Formatting/Linting**: Biome

## Project Structure

- `/app`: Next.js App Router pages and API routes
  - `/(auth)`: Authentication-related components and routes
  - `/(chat)`: Chat interface components and routes
- `/artifacts`: Code for different artifact types (code, image, sheet, text)
- `/components`: Reusable React components
  - `/ui`: Base UI components (mostly from shadcn/ui)
- `/hooks`: Custom React hooks
- `/lib`: Utility functions and core logic
  - `/ai`: AI-related utilities and model definitions
  - `/db`: Database schema and queries
  - `/editor`: Code editor configuration

## Key Concepts

### Artifacts

Artifacts are structured content outputs from the AI, such as:

- Code blocks with executable capabilities
- Text documents with formatting
- Spreadsheets with data manipulation
- Images with editing capabilities

The artifact system uses:

- `create-artifact.tsx`: Base class and interfaces for artifacts
- `artifact.tsx`: UI components for displaying artifacts
- Artifact-specific implementations in `/artifacts/{type}/client.tsx`

### Authentication

Authentication is implemented using Auth.js (NextAuth) with:

- Email/password authentication
- Session management
- User permissions

### Chat Interface

The chat interface consists of:

- Message components
- Input components with multimodal capabilities
- Streaming responses handling
- Message history

### Database

The application uses Drizzle ORM with Neon Postgres for:

- Chat history
- User data
- Document/artifact storage

#### Local Database Setup

For local development, the project uses PostgreSQL in a Docker container:

- Container name: `postgres-dev`
- Database name: `ai_chatbot_db`
- Default credentials: `postgres:mysecretpassword`
- Port: 5432 (standard PostgreSQL port)

Docker setup command:

```bash
docker run --name postgres-dev -e POSTGRES_PASSWORD=mysecretpassword -e POSTGRES_DB=ai_chatbot_db -p 5432:5432 -d postgres
```

Required environment variables in `.env.local`:

```
POSTGRES_URL=postgres://postgres:mysecretpassword@localhost:5432/ai_chatbot_db
USE_SQLITE=false
```

After setting up the database, run migrations:

```bash
pnpm db:migrate
```

Database management commands:

- Start container: `docker start postgres-dev`
- Stop container: `docker stop postgres-dev`
- Access database: `docker exec -it postgres-dev psql -U postgres -d ai_chatbot_db`
- Reset database: `docker rm -f postgres-dev` followed by recreation and migration

## Development Guidelines

### Components

- Use TypeScript for all components
- Follow the existing pattern of UI components in `/components/ui`
- For new UI components, follow the shadcn/ui pattern

### Server-Side Logic

- Use Server Actions for data mutation when possible
- API routes are in `/app/**/api` directories following Next.js App Router patterns

### Styling

- Use Tailwind CSS for styling
- Use `cn()` utility for conditional class names
- Follow the existing color scheme and design system

### State Management

- Use React hooks for component state
- Use SWR for data fetching and caching
- Use context providers for global state

### New Features

- Add new capabilities as artifacts when applicable
- Follow the pattern in existing artifacts for new implementations
- Ensure proper TypeScript typing for all new code

### AI Integration

- Default model is xAI Grok, but the system supports other providers
- Model configurations are in `/lib/ai/models.ts`
- Stream handling is in `/components/data-stream-handler.tsx`

## Common Tasks

### Adding a New UI Component

1. Create the component in `/components/ui`
2. Export it from the appropriate barrel file
3. Use it in your application components

### Adding a New Artifact Type

1. Create files in `/artifacts/[type]/client.tsx` and `/artifacts/[type]/server.ts`
2. Follow the pattern from existing artifacts
3. Register the new artifact in `/components/artifact.tsx`

### Modifying Database Schema

1. Update schema definitions in `/lib/db/schema.ts`
2. Run migrations with `pnpm db:migrate`

### Working with API Routes

1. Create new API routes in `/app/**/api/[route]/route.ts`
2. Use the API route from the frontend with fetch or SWR

## Testing

- End-to-end tests use Playwright
- Test files are in `/tests` directory
- Run tests with `pnpm test`
