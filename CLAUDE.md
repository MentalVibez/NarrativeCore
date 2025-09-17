# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**NarrativeCore** is a mobile-first PWA for creative writers built with Next.js 14+ (App Router), Supabase, and OpenAI. The MVP focuses on voice-to-text capture, AI-powered writing analysis, and a conversational AI writing partner.

## Tech Stack

- **Next.js 14+ (App Router)** with TypeScript and Server Actions
- **Tailwind CSS** + **shadcn/ui** components
- **Supabase** (Postgres, Auth, RLS, Realtime)
- **OpenAI API** (GPT for analysis + chat partner)
- **PWA** capabilities (service worker, offline support)
- **Zod** for validation, **Drizzle ORM** for type-safe DB queries
- **Vercel** deployment target

## Development Commands

```bash
# Development
pnpm dev              # Start development server
pnpm build           # Production build
pnpm start           # Start production server

# Code Quality
pnpm lint            # ESLint
pnpm typecheck       # TypeScript checking
pnpm format          # Prettier formatting

# Database
pnpm db:push         # Apply schema changes
pnpm db:seed         # Seed development data
```

## Architecture Overview

### Core Features
1. **Voice-to-Text Capture**: Web Speech API with OpenAI Whisper fallback
2. **Document Management**: CRUD with autosave and real-time sync
3. **AI Writing Analysis**: Structured tone/style/voice analysis using OpenAI
4. **Conversational AI Partner**: Context-aware chat about documents
5. **PWA Capabilities**: Offline editing with background sync

### Database Schema (Supabase)
- `profiles`: User metadata and subscription tiers
- `documents`: User documents with content and metadata
- `analyses`: AI analysis results stored as JSONB

### File Structure
```
src/
├── app/
│   ├── (auth)/          # Login/signup flows
│   ├── (dashboard)/     # Main app routes
│   └── api/             # Server actions
├── components/
│   ├── ui/              # shadcn components
│   ├── editor/          # Document editor
│   ├── VoiceFab.tsx     # Voice capture FAB
│   ├── AnalysisPanel.tsx
│   └── ChatPartner.tsx
└── lib/
    ├── supabase/        # DB client/server setup
    ├── ai/              # OpenAI integration
    ├── pwa/             # PWA utilities
    └── db/              # Drizzle schemas
```

## Key Implementation Details

### PWA Strategy
- Service worker caches app shell + last 10 documents
- IndexedDB for offline document storage
- Background sync on reconnection

### AI Integration
- Structured OpenAI responses using Zod schemas
- Rate limiting for free tier users
- Contextual chat with document excerpts

### Mobile Optimization
- Touch-friendly UI with safe area handling
- Voice capture as primary input method
- Optimized for <3.5s TTI on 4G

### Security
- Supabase RLS policies for user data isolation
- Server Actions for all mutations
- CSP headers and input validation

## Environment Variables

Required for development:
```
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
OPENAI_API_KEY=
```

## Development Workflow

1. Use Server Actions for all database mutations
2. Follow mobile-first design principles
3. Implement offline-first document editing
4. Structure AI responses with Zod schemas
5. Test PWA functionality on actual mobile devices

## Testing Strategy

- Unit tests for utils and AI schema parsing
- E2E tests (Playwright) for critical user flows
- Lighthouse CI for PWA performance validation
- Mobile device testing for voice capture flows