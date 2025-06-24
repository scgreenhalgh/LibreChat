# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# LibreChat Development Guide

## Project Overview

LibreChat is an all-in-one AI conversation platform that brings together multiple AI models (OpenAI, Anthropic, Google, Azure, etc.) with the original ChatGPT styling. It's a full-stack application built with React (frontend) and Express.js (backend), using MongoDB as the database.

**Key Features:**
- Multi-model support (OpenAI, Anthropic, Google Vertex AI, BingAI, etc.)
- Multimodal chat with image upload and analysis
- Plugin system for extended functionality
- Custom presets and conversation branching
- Multi-user authentication with JWT
- Multilingual support (18+ languages)
- Docker deployment ready

## Tech Stack

### Frontend (Client)
- **Framework:** React 18 with TypeScript (transitioning from JS)
- **State Management:** Recoil
- **Styling:** Tailwind CSS
- **Forms:** React Hook Form
- **Data Fetching:** React Query (TanStack Query)
- **UI Components:** Radix UI primitives
- **Build Tool:** Vite
- **Testing:** Jest + React Testing Library

### Backend (API)
- **Runtime:** Node.js
- **Framework:** Express.js
- **Database:** MongoDB with Mongoose ODM
- **Authentication:** Passport.js with JWT
- **AI Integration:** Official SDKs (OpenAI, Anthropic, etc.)
- **File Storage:** Local/Firebase/OpenAI
- **Search:** Meilisearch
- **Caching:** Redis/MongoDB (Keyv)
- **Testing:** Jest + Supertest

### DevOps & Tools
- **Containerization:** Docker & Docker Compose
- **Package Management:** npm (with support for Bun)
- **Code Quality:** ESLint + Prettier
- **Version Control:** Git with GitFlow
- **E2E Testing:** Playwright
- **Pre-commit Hooks:** Husky

## Project Structure

```
LibreChat/
├── api/                    # Backend Express.js application
│   ├── app/               # Core application logic
│   │   ├── clients/       # AI provider client implementations
│   │   │   ├── AnthropicClient.js
│   │   │   ├── OpenAIClient.js
│   │   │   ├── GoogleClient.js
│   │   │   └── ...
│   │   └── index.js
│   ├── cache/             # Caching implementations
│   ├── config/            # Configuration files
│   ├── models/            # Mongoose models
│   │   ├── User.js
│   │   ├── Conversation.js
│   │   ├── Message.js
│   │   └── ...
│   ├── server/            # Express server setup
│   │   ├── controllers/   # Route controllers
│   │   ├── middleware/    # Express middleware
│   │   ├── routes/        # API routes
│   │   └── services/      # Business logic services
│   ├── strategies/        # Passport authentication strategies
│   └── utils/             # Utility functions
├── client/                # Frontend React application
│   ├── src/
│   │   ├── components/    # React components
│   │   ├── hooks/         # Custom React hooks
│   │   ├── store/         # Recoil state management
│   │   ├── data-provider/ # React Query setup
│   │   ├── localization/  # i18n translations
│   │   └── utils/         # Frontend utilities
│   └── public/            # Static assets
├── packages/              # Shared packages (monorepo)
│   └── data-provider/     # Shared data fetching logic
├── docs/                  # Documentation
├── e2e/                   # Playwright E2E tests
└── config/                # Configuration scripts
```

## Key Commands

### Development
```bash
# Install dependencies (from root)
npm install

# Start development servers
npm run backend:dev    # API server with nodemon (port 3080)
npm run frontend:dev   # React dev server with hot reload (port 3090)

# Alternative: Bun commands
bun run b:api:dev     # API with Bun
bun run b:client:dev  # Frontend with Bun
```

### Building & Production
```bash
# Build frontend
npm run frontend      # Builds client to /client/dist

# Run production
npm run backend       # Start production API server

# Docker
docker-compose up -d  # Start all services
```

### Testing
```bash
# Unit tests
npm run test:api      # API tests
npm run test:client   # Frontend tests

# E2E tests
npm run e2e           # Playwright tests (local)
npm run e2e:ci        # Playwright tests (CI)
```

### Database & User Management
```bash
# User management
npm run create-user
npm run delete-user
npm run ban-user

# Balance management
npm run add-balance
npm run list-balances
```

## Configuration

### Environment Variables (.env)
Key environment variables to configure:

```bash
# Server
HOST=localhost
PORT=3080
MONGO_URI=mongodb://127.0.0.1:27017/LibreChat
DOMAIN_CLIENT=http://localhost:3080
DOMAIN_SERVER=http://localhost:3080

# AI Providers (set as needed)
OPENAI_API_KEY=
ANTHROPIC_API_KEY=
GOOGLE_API_KEY=
AZURE_API_KEY=

# Authentication
JWT_SECRET=<your-secret>
JWT_REFRESH_SECRET=<your-refresh-secret>

# Optional Services
MEILISEARCH_URL=
REDIS_URI=
```

### Configuration File (librechat.yaml)
Advanced configuration for endpoints, models, and features. See `librechat.example.yaml` for reference.

## Development Guidelines

### Code Standards
- Follow Airbnb JavaScript Style Guide
- Use ESLint and Prettier (configs provided)
- CommonJS for Node.js modules
- TypeScript for new frontend code

### API Development Pattern
Follow the MVC-like pattern with separation of concerns:

1. **Routes** (`/server/routes/`): Define endpoints and middleware
2. **Controllers** (`/server/controllers/`): Handle request/response logic
3. **Services** (`/server/services/`): Business logic and external integrations
4. **Models** (`/models/`): Mongoose schemas and database operations

### Frontend Development
- Use TypeScript for new components
- Keep components small and focused (single responsibility)
- Use Recoil sparingly - prefer local state when possible
- Data fetching through `data-provider` with React Query
- Follow React best practices and hooks patterns

### State Management
- **Global State (Recoil):** Only for truly global data (user, settings, conversations)
- **Local State:** For component-specific data
- **React Query:** For server state and caching

### Git Workflow
- Create feature branches from `main`
- Use descriptive commit messages
- Small, focused PRs preferred
- Run tests before submitting PR
- Update documentation as needed

## Common Development Tasks

### Adding a New AI Provider
1. Create client in `/api/app/clients/NewProviderClient.js`
2. Extend `BaseClient` class
3. Add to `/api/server/services/Endpoints/`
4. Update frontend model options
5. Add environment variables
6. Update documentation

### Adding a New Feature
1. Plan the data model changes (if any)
2. Create API endpoints following the pattern
3. Add frontend components with TypeScript
4. Include appropriate tests
5. Update relevant documentation

### Debugging
- Backend logs: `./api/logs/debug-{date}.log`
- Enable verbose logging: `DEBUG_CONSOLE=true`
- Use React DevTools and Redux DevTools
- MongoDB Compass for database inspection

## Testing Strategy

### Unit Tests
- Backend: Jest with mocked dependencies
- Frontend: React Testing Library
- Focus on business logic and critical paths

### Integration Tests
- API endpoints: Supertest
- Database operations: In-memory MongoDB

### E2E Tests
- Playwright for critical user flows
- Test against local development environment

## Deployment Considerations

### Docker Deployment
- Use provided `docker-compose.yml`
- Override with `docker-compose.override.yml` for customization
- Supports multi-platform builds (amd64, arm64)

### Cloud Deployment
Supported platforms:
- DigitalOcean
- Azure
- Railway
- Render
- Hugging Face
- Zeabur

### Production Checklist
- Set strong JWT secrets
- Configure rate limiting
- Enable HTTPS
- Set up monitoring/logging
- Configure backup strategy
- Review security settings

## Contributing

1. Check existing issues and PRs
2. Discuss new features before implementing
3. Follow code standards and patterns
4. Include tests for new functionality
5. Update documentation
6. Submit PR with clear description

## Resources

- [Documentation](https://docs.librechat.ai)
- [GitHub Issues](https://github.com/danny-avila/LibreChat/issues)
- [Discord Community](https://discord.librechat.ai)
- [Contributing Guide](./docs/contributions/how_to_contribute.md)
- [Breaking Changes](./docs/general_info/breaking_changes.md)

## Architecture Decisions

### Why Monorepo?
- Shared code between frontend and backend
- Unified versioning and deployment
- Easier dependency management

### Why Multiple AI Clients?
- Each provider has unique APIs and features
- Allows provider-specific optimizations
- Easier to maintain and debug

### Why Recoil + React Query?
- Recoil: Simple atoms for global client state
- React Query: Powerful server state management
- Clear separation of concerns

### Database Schema Design
- Conversations and Messages as separate collections
- User-centric design with references
- Optimized for chat application patterns
- Support for conversation branching

## Troubleshooting

### Common Issues
1. **MongoDB Connection**: Ensure MongoDB is running and URI is correct
2. **API Keys**: Verify all required API keys are set
3. **Port Conflicts**: Check if ports 3080/3090 are available
4. **Build Errors**: Clear node_modules and reinstall
5. **Docker Issues**: Check Docker daemon and compose version

### Debug Mode
Enable comprehensive logging:
```bash
DEBUG_LOGGING=true
DEBUG_CONSOLE=true
```

This guide should help you understand and work with the LibreChat codebase effectively. For specific implementation details, refer to the inline documentation and existing code patterns.