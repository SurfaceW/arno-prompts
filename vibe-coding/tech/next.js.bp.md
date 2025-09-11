# Next.js App Architecture Guide - AI Instructions

## Server Architecture

### API Design

- Use RESTful API design
- Implement stateless and serverless design
- Add version control in pathname for APIs: `/api/v1/*`

### Layer Structure

1. **Application Layer (Controller)**
   - Handle request/response in web layer
   - Use composable patterns for routes and actions
   - Zero business logic in this layer

2. **Service Layer**
   - Handle business logic with domain services
   - Return unified response objects
   - Use interface-oriented design
   - Follow SOLID, DRY, KISS principles

3. **Manager Layer**
   - Create reusable business logic modules
   - Implement atomic, modular business logic
   - Place utils/helpers here

4. **Data Persistence Layer**
   - Use appropriate DB (SQL/NoSQL) for features
   - Implement ORM (Prisma/TypeORM)
   - Design for data scaling and consistency
   - Use transactions for sensitive operations

### Next.js Best Practices

- Use turbo-pack + git-submodules + npm packages
- Implement layered cache system
- Use EdgeRuntime for static content
- Separate ServerComponent/ClientComponent data
- Use server-actions first for seamless server-client logic

### Directory Structure

```
.
├── app                 # Main app entry
├── components          # Shared components
├── configs             # App configs
├── lib                 # Shared libraries
├── prisma              # DB schema
├── public              # Static files
├── services            # Shared services
├── types               # Global types
```

## Web Client Architecture

### React Patterns

- Separate Logic and View
- Layer state management:
  - ServerState: server-managed state
  - SharedContext: React.Context for global state
  - SharedState: redux/zustand for shared state
  - ReactiveState: RxJS/Mobx for reactive state
  - LocalComponentState: useState/useReducer
- Use Hook-Style First approach

### Logic Encapsulation

- **Service**: API clients, data services
- **Manager**: Business logic modules

### File Naming Conventions

- `*.type.ts`: TypeScript types
- `*.constant.ts`: Constants
- `*.util.ts`: Utility functions
- `*.manager.ts`: Business managers
- `*.service.ts`: Business services
- `*.store.ts`: State stores
- `*.hook.ts`: React hooks
- `*.client.tsx`: Client components
- `*.server.tsx`: Server components
- `*.server.action.ts`: Server actions

### Method Naming Conventions

- Use verb+noun: `createUser`, `getUser`
- CRUD operations: `create`, `get/list`, `update`, `delete`
- Common operations: `search`, `fetch`, `count`
- State transitions: `init`, `start`, `stop`
- Batch operations: `batch`, `bulk`, `multi`

### Error Handling

- Use native `Error` objects
- Utilize HTTP status codes for responses
- Implement ErrorBoundary for component errors
