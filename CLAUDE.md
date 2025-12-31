# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview
BlogYapay is an AI-powered platform for automating content creation, management, and publishing across multiple Blogger accounts. The system enables a single user to manage 100+ blogs with automated content generation, theme creation, posting, and AdSense integration.

**Core Goals:**
- Automate blog creation and management via Gmail/Blogger API
- Generate unique, SEO-optimized content (text, images, video) using AI
- Schedule and publish posts automatically
- Manage AdSense integration and track revenue
- Minimize manual intervention while maintaining content quality

## Tech Stack

### Frontend
- **Next.js (App Router)** - React framework with SSR for SEO-friendly pages
- Modern UI components for dashboard, editor, and blog management

### Backend / AI Orchestration
- **Python (FastAPI)** - Async API server for AI integration
- **LangChain** - AI workflow orchestration
- **OpenAI API (GPT-4o)** - Primary content generation
- **Anthropic API (Claude 3.5 Sonnet)** - Alternative LLM with large context windows

### Database
- **Supabase (Self-hosted)** - PostgreSQL + Auth + Storage + Real-time
- Built-in authentication and user management
- File storage for images and media
- Auto-generated REST API
- Row-level security (RLS)

### APIs & Integrations
- **Google Blogger API v3** - Blog creation, posting, media upload
- **OAuth 2.0** - Secure account authorization (no password storage)
- **Redis/BullMQ** - Async job queues for content generation and publishing

### DevOps
- **Coolify** - Self-hosted PaaS on VPS
- **Docker** - Containerized deployment

## Architecture Overview

### Multi-Tier System
1. **Frontend (Next.js)**: Dashboard, blog list, content editor, scheduling UI, analytics
2. **Backend (FastAPI)**: AI content generation, Blogger API integration, queue management
3. **Database (Supabase)**: PostgreSQL + Auth + Storage + Real-time API
   - User authentication and management
   - Blog credentials and OAuth tokens (encrypted)
   - Content drafts and publish schedules
   - Image/media file storage
4. **Job Queue (Redis)**: Background processing for content generation and publishing to avoid blocking

### Key Data Flows
- **Blog Connection**: User OAuth → Store tokens → List blogs → Manage from dashboard
- **Content Generation**: User input/topic → AI generates content → Editor preview → Schedule or publish
- **Publishing**: Scheduled jobs → Queue worker → Blogger API → Post created → Status updated
- **Analytics**: Blogger API stats → Dashboard aggregation → Revenue tracking

### Security Requirements
- Encrypt API keys and OAuth tokens at rest
- Never store user passwords
- Use OAuth 2.0 for all Google account access
- Implement rate limiting to avoid API quota exhaustion
- Log all errors with retry mechanisms for API failures

## Content Generation Guidelines

When generating blog content for the AI module:
- Start directly with content (no "Elbette", "Tabii", "Harika", "İşte")
- No introductory comments in responses
- First line must be H1 title (#)
- Opening paragraph: 2-3 sentences
- Use H2 (##) for subheadings
- Include SEO keywords naturally
- No horizontal rules (---) as separators
- Image suggestions in [alt text] format
- Keep responses concise and structured
- Use bullet points for key information
- Validate information accuracy before outputting

### Image Requirements
- Max file size: 5MB
- Formats: JPG, PNG, WebP
- Minimum resolution: 1920x1080

## Development Practices

### When Building Features
- Think step-by-step for complex tasks
- Use XML tags for structured planning
- Provide context for component usage
- Document constraints and limitations
- Include input/output examples where helpful

### Communication Style
- Direct and concise responses
- Use markdown formatting (bold, italic, lists)
- No unnecessary intro phrases
- Highlight key points
- Ask clarifying questions when requirements are ambiguous

## Common Commands

```bash
# Frontend (Next.js)
npm install          # Install dependencies
npm run dev          # Start development server
npm run build        # Production build
npm run start        # Start production server

# Backend (FastAPI)
pip install -r requirements.txt    # Install dependencies
uvicorn main:app --reload          # Start development server
pytest                             # Run tests

# Database (Supabase)
npm run supabase:start           # Start local Supabase (Docker)
npm run supabase:stop            # Stop local Supabase
npm run db:migrate               # Run database migrations
npm run db:seed                  # Seed database
npm run supabase:gen-types       # Generate TypeScript types from database
```

## Key Integration Points

### Blogger API
- OAuth 2.0 flow for account authorization
- Rate limits: Monitor quota usage, implement backoff
- Operations: Create blog, create post, upload media, update post, delete post

### AI Content Generation
- Primary: OpenAI GPT-4o for creative content
- Secondary: Claude 3.5 Sonnet for long-form content with context
- Implement content validation before publishing
- User approval mechanism for quality control

### Scheduling System
- Randomize posting intervals to mimic human behavior
- Prevent spam detection by Google
- Support timezone-aware scheduling
- Queue management for bulk operations

## Deployment

### Coolify Deployment

**Production Environment:**
- **Hosting:** Self-hosted VPS with Coolify
- **Domain:** https://blogyapay.setrox.net
- **Port Mapping:** External port 80 → Internal port 3000
- **Auto-Deploy:** GitHub integration (push to main branch)
- **Database:** Supabase (self-hosted on same VPS)
- **Cache/Queue:** Redis (on same VPS)

### Deployment Configuration

**Dockerfile Requirements:**
- Multi-stage build for optimized image size
- Production build with Next.js standalone output
- Environment variables injected at runtime
- Health check endpoint for container monitoring

**Environment Variables:**
Required environment variables in Coolify:
```bash
# Supabase (from Coolify self-hosted Supabase)
NEXT_PUBLIC_SUPABASE_URL=https://supabase.yourdomain.com
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# Redis
REDIS_URL=redis://localhost:6379

# API Keys
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...

# Google OAuth (Blogger API)
GOOGLE_CLIENT_ID=...
GOOGLE_CLIENT_SECRET=...
GOOGLE_REDIRECT_URI=https://blogyapay.setrox.net/api/auth/callback

# Application
NEXT_PUBLIC_APP_URL=https://blogyapay.setrox.net
NODE_ENV=production
```

### Deployment Workflow

1. **Push to GitHub:** Code pushed to main branch
2. **Auto-Deploy:** Coolify detects changes and pulls latest code
3. **Build:** Docker builds image using Dockerfile
4. **Deploy:** New container started on port 3000 (mapped to 80)
5. **Health Check:** Coolify verifies container is healthy
6. **Traffic Switch:** New version receives traffic

### Database Setup

**Using Coolify Self-hosted Supabase:**

1. **Supabase Instance:** Already deployed on Coolify
2. **Get Credentials:** From Coolify Supabase dashboard
   - Supabase URL
   - Anon (public) key
   - Service role (admin) key

3. **Run Migrations:**
```bash
# After first app deployment
npm run db:migrate
```

4. **Set up Storage Buckets:**
```sql
-- In Supabase SQL editor
CREATE BUCKET blog_images;
CREATE BUCKET user_avatars;
```

5. **Enable RLS Policies:**
```sql
-- Example RLS policy for blog_credentials table
ALTER TABLE blog_credentials ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can only see their own blogs"
ON blog_credentials FOR SELECT
USING (auth.uid() = user_id);
```

### Local Development with Docker

```bash
# Start local development environment
docker-compose up -d

# View logs
docker-compose logs -f

# Stop environment
docker-compose down
```

### Monitoring & Maintenance

- **Logs:** Coolify dashboard → Logs tab
- **Resource Usage:** Monitor CPU/Memory via Coolify
- **Database Backups:** Configure automated PostgreSQL backups
- **SSL/TLS:** Automatic Let's Encrypt certificates via Coolify
- **Health Endpoint:** `/api/health` for uptime monitoring
