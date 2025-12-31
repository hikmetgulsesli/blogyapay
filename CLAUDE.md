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
- **Supabase (PostgreSQL)** - User data, blog credentials, content drafts, scheduling
- Built-in authentication and file storage

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
2. **Backend (FastAPI)**: API routes, AI content generation, Blogger API integration, queue management
3. **Database (Supabase)**: User accounts, blog tokens, content storage, publish schedules
4. **Job Queue**: Background processing for content generation and publishing to avoid blocking

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

# Database
# Supabase CLI commands for migrations
supabase db push                   # Apply migrations
supabase db reset                  # Reset database
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
