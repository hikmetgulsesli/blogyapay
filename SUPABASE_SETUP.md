# Supabase Setup Guide

This guide covers setting up Supabase for BlogYapay in Coolify.

## Coolify Supabase Deployment

### 1. Deploy Supabase in Coolify

1. Go to Coolify dashboard
2. Click "New Resource" → "Service" → "Supabase"
3. Configure:
   - **Name:** blogyapay-supabase
   - **Domain:** supabase.setrox.net (or your preferred subdomain)
   - **PostgreSQL Version:** 15 or 16
   - **Enable Services:**
     - [x] PostgreSQL
     - [x] PostgREST (API)
     - [x] GoTrue (Auth)
     - [x] Realtime
     - [x] Storage
     - [x] Studio (Admin Dashboard)

4. Click "Deploy"

### 2. Get Supabase Credentials

After deployment, go to Supabase service in Coolify:

1. **Supabase URL:**
   ```
   https://supabase.setrox.net
   ```

2. **API Keys:** Found in Coolify environment variables or Supabase Studio
   - **Anon Key (Public):** Used in frontend
   - **Service Role Key (Admin):** Used in backend/server-side

3. **Database Connection:**
   - Host: internal Docker network or localhost
   - Port: 5432
   - Database: postgres
   - User: postgres
   - Password: (set during deployment)

### 3. Access Supabase Studio

Navigate to: `https://supabase.setrox.net/studio`

Login with credentials from Coolify deployment.

## Database Schema Setup

### 1. Create Tables

In Supabase Studio SQL Editor, run:

```sql
-- Users table (Supabase auth handles this automatically)
-- We only need to add custom fields to auth.users metadata

-- Blogs table (stores connected Blogger accounts)
CREATE TABLE blogs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  blogger_id VARCHAR(255) NOT NULL UNIQUE,
  blog_name VARCHAR(255) NOT NULL,
  blog_url TEXT NOT NULL,
  access_token TEXT NOT NULL, -- Encrypted
  refresh_token TEXT NOT NULL, -- Encrypted
  token_expires_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Content drafts table
CREATE TABLE content_drafts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  blog_id UUID REFERENCES blogs(id) ON DELETE CASCADE,
  title VARCHAR(500) NOT NULL,
  content TEXT NOT NULL,
  status VARCHAR(50) DEFAULT 'draft', -- draft, scheduled, published, failed
  ai_model VARCHAR(100), -- gpt-4o, claude-3-5-sonnet
  seo_keywords TEXT[],
  featured_image_url TEXT,
  scheduled_at TIMESTAMPTZ,
  published_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Publishing queue table
CREATE TABLE publish_queue (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  draft_id UUID REFERENCES content_drafts(id) ON DELETE CASCADE,
  blog_id UUID REFERENCES blogs(id) ON DELETE CASCADE,
  status VARCHAR(50) DEFAULT 'pending', -- pending, processing, completed, failed
  retry_count INTEGER DEFAULT 0,
  error_message TEXT,
  scheduled_for TIMESTAMPTZ NOT NULL,
  processed_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Analytics table
CREATE TABLE blog_analytics (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  blog_id UUID REFERENCES blogs(id) ON DELETE CASCADE,
  date DATE NOT NULL,
  page_views INTEGER DEFAULT 0,
  unique_visitors INTEGER DEFAULT 0,
  adsense_revenue DECIMAL(10, 2) DEFAULT 0,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(blog_id, date)
);

-- Indexes for performance
CREATE INDEX idx_blogs_user_id ON blogs(user_id);
CREATE INDEX idx_content_drafts_user_id ON content_drafts(user_id);
CREATE INDEX idx_content_drafts_blog_id ON content_drafts(blog_id);
CREATE INDEX idx_content_drafts_status ON content_drafts(status);
CREATE INDEX idx_publish_queue_status ON publish_queue(status);
CREATE INDEX idx_publish_queue_scheduled_for ON publish_queue(scheduled_for);
CREATE INDEX idx_blog_analytics_blog_id ON blog_analytics(blog_id);
CREATE INDEX idx_blog_analytics_date ON blog_analytics(date);
```

### 2. Enable Row Level Security (RLS)

```sql
-- Enable RLS on all tables
ALTER TABLE blogs ENABLE ROW LEVEL SECURITY;
ALTER TABLE content_drafts ENABLE ROW LEVEL SECURITY;
ALTER TABLE publish_queue ENABLE ROW LEVEL SECURITY;
ALTER TABLE blog_analytics ENABLE ROW LEVEL SECURITY;

-- Blogs policies
CREATE POLICY "Users can view their own blogs"
ON blogs FOR SELECT
USING (auth.uid() = user_id);

CREATE POLICY "Users can insert their own blogs"
ON blogs FOR INSERT
WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can update their own blogs"
ON blogs FOR UPDATE
USING (auth.uid() = user_id);

CREATE POLICY "Users can delete their own blogs"
ON blogs FOR DELETE
USING (auth.uid() = user_id);

-- Content drafts policies
CREATE POLICY "Users can view their own drafts"
ON content_drafts FOR SELECT
USING (auth.uid() = user_id);

CREATE POLICY "Users can insert their own drafts"
ON content_drafts FOR INSERT
WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can update their own drafts"
ON content_drafts FOR UPDATE
USING (auth.uid() = user_id);

CREATE POLICY "Users can delete their own drafts"
ON content_drafts FOR DELETE
USING (auth.uid() = user_id);

-- Publish queue policies (similar pattern)
CREATE POLICY "Users can view their own queue items"
ON publish_queue FOR SELECT
USING (
  EXISTS (
    SELECT 1 FROM content_drafts
    WHERE content_drafts.id = publish_queue.draft_id
    AND content_drafts.user_id = auth.uid()
  )
);

-- Analytics policies
CREATE POLICY "Users can view their own analytics"
ON blog_analytics FOR SELECT
USING (
  EXISTS (
    SELECT 1 FROM blogs
    WHERE blogs.id = blog_analytics.blog_id
    AND blogs.user_id = auth.uid()
  )
);
```

### 3. Create Storage Buckets

```sql
-- Create buckets for file storage
INSERT INTO storage.buckets (id, name, public)
VALUES
  ('blog-images', 'blog-images', true),
  ('user-avatars', 'user-avatars', true);

-- Storage policies for blog images
CREATE POLICY "Users can upload blog images"
ON storage.objects FOR INSERT
TO authenticated
WITH CHECK (bucket_id = 'blog-images');

CREATE POLICY "Anyone can view blog images"
ON storage.objects FOR SELECT
TO public
USING (bucket_id = 'blog-images');

CREATE POLICY "Users can update their own blog images"
ON storage.objects FOR UPDATE
TO authenticated
USING (bucket_id = 'blog-images' AND owner = auth.uid());

CREATE POLICY "Users can delete their own blog images"
ON storage.objects FOR DELETE
TO authenticated
USING (bucket_id = 'blog-images' AND owner = auth.uid());

-- Storage policies for user avatars
CREATE POLICY "Users can upload their own avatars"
ON storage.objects FOR INSERT
TO authenticated
WITH CHECK (bucket_id = 'user-avatars' AND owner = auth.uid());

CREATE POLICY "Anyone can view avatars"
ON storage.objects FOR SELECT
TO public
USING (bucket_id = 'user-avatars');

CREATE POLICY "Users can update their own avatars"
ON storage.objects FOR UPDATE
TO authenticated
USING (bucket_id = 'user-avatars' AND owner = auth.uid());

CREATE POLICY "Users can delete their own avatars"
ON storage.objects FOR DELETE
TO authenticated
USING (bucket_id = 'user-avatars' AND owner = auth.uid());
```

### 4. Configure Auth Settings

In Supabase Studio → Authentication → Settings:

1. **Email Auth:**
   - Enable email/password authentication
   - Enable email confirmations
   - Set redirect URLs:
     - `https://blogyapay.setrox.net/auth/callback`
     - `http://localhost:3000/auth/callback` (for development)

2. **Site URL:**
   - Production: `https://blogyapay.setrox.net`
   - Development: `http://localhost:3000`

3. **Email Templates:**
   - Customize confirmation, reset password emails
   - Add BlogYapay branding

## Environment Variables for BlogYapay App

Add these to Coolify environment variables for BlogYapay app:

```bash
NEXT_PUBLIC_SUPABASE_URL=https://supabase.setrox.net
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbG... (from Supabase Studio)
SUPABASE_SERVICE_ROLE_KEY=eyJhbG... (from Supabase Studio - keep secret!)
```

## Testing the Setup

### 1. Test Supabase Connection

Create a test file: `lib/supabase/client.ts`

```typescript
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = process.env.NEXT_PUBLIC_SUPABASE_URL!
const supabaseAnonKey = process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!

export const supabase = createClient(supabaseUrl, supabaseAnonKey)
```

### 2. Install Supabase Client

```bash
npm install @supabase/supabase-js
```

### 3. Test Query

```typescript
const { data, error } = await supabase
  .from('blogs')
  .select('*')
  .limit(10)

console.log(data, error)
```

## Backup Strategy

1. **Automated Backups:** Configure in Coolify
2. **Frequency:** Daily at 3 AM
3. **Retention:** 30 days
4. **Location:** VPS local storage or S3-compatible storage

## Security Checklist

- [x] RLS enabled on all tables
- [x] Service role key stored securely (never in frontend)
- [x] HTTPS enabled for all endpoints
- [x] Email confirmation enabled
- [x] Strong password policy configured
- [x] Rate limiting configured in Supabase
- [x] OAuth tokens encrypted at rest

## Troubleshooting

### Cannot connect to Supabase
- Check if Supabase service is running in Coolify
- Verify DNS is pointing to correct IP
- Check firewall rules

### RLS policies blocking queries
- Test with service role key first
- Check policy conditions
- View logs in Supabase Studio

### Storage upload fails
- Check bucket policies
- Verify file size limits
- Check CORS settings
