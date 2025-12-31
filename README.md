# BlogYapay

AI-powered platform for automating blog content creation and management across multiple Blogger accounts.

## Quick Start

### Local Development

1. **Clone the repository**
```bash
git clone https://github.com/hikmetgulsesli/blogyapay.git
cd blogyapay
```

2. **Install dependencies**
```bash
npm install
```

3. **Set up environment variables**
```bash
cp .env.example .env
# Edit .env with your configuration
```

4. **Start development server**
```bash
npm run dev
```

Visit http://localhost:3000

### Docker Development

Start all services (PostgreSQL, Redis, App):
```bash
docker-compose up -d
```

View logs:
```bash
docker-compose logs -f app
```

Stop services:
```bash
docker-compose down
```

## Production Deployment (Coolify)

### Prerequisites
- VPS with Coolify installed
- PostgreSQL database created
- Redis instance running
- Domain configured (blogyapay.setrox.net)

### Deployment Steps

1. **Get Database Credentials**
   - PostgreSQL connection string (provided by admin)
   - Redis connection string (provided by admin)

2. **Configure Coolify**
   - Add GitHub repository
   - Set Dockerfile path: `./Dockerfile`
   - Configure port mapping: 80 → 3000
   - Set domain: blogyapay.setrox.net

3. **Set Environment Variables**
   Add all required variables from `.env.example` in Coolify dashboard

4. **Deploy**
   - Push to main branch → Auto-deploy triggers
   - Or manually deploy from Coolify dashboard

## Tech Stack

- **Frontend:** Next.js 14+ (App Router)
- **Backend:** FastAPI (Python)
- **Database:** PostgreSQL
- **Cache/Queue:** Redis
- **AI:** OpenAI GPT-4o, Anthropic Claude
- **Deployment:** Docker + Coolify

## Project Structure

```
blogyapay/
├── app/                 # Next.js app directory
├── components/          # React components
├── lib/                 # Utility functions
├── public/              # Static assets
├── styles/              # CSS/Tailwind styles
├── api/                 # Backend API (FastAPI)
├── Dockerfile           # Production container
├── docker-compose.yml   # Local development
└── .env.example         # Environment template
```

## Documentation

- [CLAUDE.md](./CLAUDE.md) - Development guidelines and architecture
- [PRD.md](./PRD.md) - Product requirements document

## License

Proprietary - All rights reserved
