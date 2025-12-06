# Overview

This is an **Internal Audit Management System** (نظام المراجعة الداخلية الذكي) - a comprehensive web application for automating internal audit operations and risk management. The system is built in Arabic (RTL layout) and provides features for audit planning, inspection visits, risk assessment, internal controls evaluation, recommendations tracking, improvement plans, and committee management.

The application follows a full-stack architecture with React on the frontend and Express on the backend, using PostgreSQL with Drizzle ORM for data persistence, and is designed to run on Replit's platform.

# User Preferences

Preferred communication style: Simple, everyday language.

# System Architecture

## Frontend Architecture

**Framework & Build Tool**
- React 18 with TypeScript for type safety
- Vite as the build tool and development server
- Wouter for client-side routing (lightweight alternative to React Router)
- TanStack Query (React Query) for server state management

**UI Component System**
- Shadcn/ui component library (New York style variant) with Radix UI primitives
- Tailwind CSS for styling with custom CSS variables for theming
- RTL (right-to-left) layout support for Arabic language
- Custom fonts: Cairo and Tajawal from Google Fonts
- Recharts for data visualization and analytics

**State Management**
- Context API for authentication state (AuthContext)
- Mock authentication system with role-based access control (admin, audit_director, auditor, user)
- TanStack Query for server state caching and synchronization

**Key Design Patterns**
- Component composition with shared UI components in `/components/ui`
- Layout wrapper pattern (MainLayout) with collapsible sidebar navigation
- Protected routes with authentication checks
- Form handling with React Hook Form and Zod validation

## Backend Architecture

**Server Framework**
- Express.js with TypeScript for the REST API
- HTTP server created with Node's native `http` module
- Middleware: express.json, express.urlencoded for request parsing

**Development vs Production**
- Development: Vite middleware mode for HMR (Hot Module Replacement)
- Production: Serves pre-built static files from `dist/public`
- Custom logging middleware for request/response tracking

**Storage Layer**
- Abstract storage interface (IStorage) for database operations
- In-memory implementation (MemStorage) for development/testing
- Production-ready structure for PostgreSQL integration via Drizzle ORM

**Build Process**
- esbuild for server bundling with selective dependency bundling (allowlist approach)
- Vite for client bundling
- Single-file server output (`dist/index.cjs`) for optimized cold starts

## Data Layer

**ORM & Migrations**
- Drizzle ORM as the query builder and migration tool
- PostgreSQL dialect configured via Neon serverless driver
- Schema-first approach with Zod validation integration (drizzle-zod)
- Migrations stored in `/migrations` directory

**Database Schema**
- Users table with UUID primary keys (gen_random_uuid)
- Schema validation using Zod for runtime type checking
- Type inference from Drizzle schema for TypeScript safety

**Current Schema**
- Single `users` table with id, username, and password fields
- Designed for extension with additional tables for audit data, risks, programs, etc.

## Authentication & Authorization

**Authentication Strategy**
- Mock authentication system in development (no real password hashing)
- Session-based architecture prepared with express-session and connect-pg-simple
- Role-based access control with four user roles

**Authorization Model**
- Permission checking via `hasPermission` method in AuthContext
- Contact officer permissions system with role-based granular permissions
- Categories: management, communication, reporting, viewing

## External Dependencies

**Database**
- PostgreSQL via Neon serverless (@neondatabase/serverless)
- Drizzle ORM for queries and migrations
- Connection via DATABASE_URL environment variable

**UI Libraries**
- Radix UI components for accessible primitives
- Lucide React for icons
- Recharts for data visualization
- date-fns for date manipulation

**Development Tools**
- Replit-specific plugins: vite-plugin-runtime-error-modal, vite-plugin-cartographer, vite-plugin-dev-banner
- Custom vite-plugin-meta-images for OpenGraph image URL rewriting
- TypeScript for type checking

**Session Management**
- express-session for session handling
- connect-pg-simple for PostgreSQL session storage (configured but not actively used with mock auth)

**Build Dependencies**
- esbuild and Vite for bundling
- tsx for TypeScript execution
- Tailwind CSS with PostCSS for styling

**Third-party Services (Prepared)**
The package.json includes dependencies suggesting planned integrations:
- Stripe for payments
- Nodemailer for email
- OpenAI and Google Generative AI for AI features
- Passport.js for authentication strategies
- Multer for file uploads
- XLSX for Excel export
- WebSocket (ws) support

**Note on Database**: While Drizzle is configured for PostgreSQL, the current implementation uses in-memory storage. The architecture is designed to swap in PostgreSQL by replacing the MemStorage implementation with a Drizzle-based storage adapter.