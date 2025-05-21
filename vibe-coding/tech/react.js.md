# React App Development Instructions

## Core Principles

- Design first, then implement
- Use appropriate state management (zustand recommended)
- Decompose UI into View, Business, and Basic components
- Enforce data immutability
- Maintain component orthogonality (separation of responsibilities)
- Use object-oriented paradigm with TypeScript for complex business logic

## Performance Optimization

- Prevent unnecessary re-renders:
  - Don't create component classes in render functions
  - Pass state to leaf nodes when possible
  - Pass children as props
  - Use React.memo to prevent downward updates
  - Use useMemo/useCallback for expensive operations
  - Use unique keys for lists
  - Use useMemo for Context values
  - Separate data and API methods in Context
  - Use multiple Context providers instead of one
  - Implement Context Selectors

- Decompose components to minimize render scope
- Implement appropriate caching strategies
- Use RenderHighlight in React DevTools to detect render issues
- Optimize package size with TreeShaking

## Stability Guidelines

- Use mature libraries with version locks
- Prevent circular dependencies in Hooks
- Implement XSS protection
- Add ErrorBoundary for error handling

## Maintainability Guidelines

- Unify code style with linters
- Standardize directory and project organization
- Apply design patterns and principles
- Strictly separate views and logic
- Limit file length to 500 lines maximum
