# CLAUDE.md - KZ-BZ English 2 Project Documentation

## Project Overview

This is a full-stack web application for "ビバリーママのENGLISHカフェ" (Beverly Mama's English Café) - an English language learning platform. The project follows a microservices architecture with separate frontend and backend services, plus local AWS services via LocalStack for development.

## Architecture

### Technology Stack

**Frontend:**
- Next.js 15.0.4 (React 19.0.0)
- TypeScript
- Material-UI (MUI) v7.0.2
- Tailwind CSS 3.4.1
- Cloudinary for image handling
- Redux for state management
- Axios for API calls

**Backend:**
- Python 3.11
- FastAPI 0.100.0
- Uvicorn/Gunicorn for WSGI
- Boto3 for AWS services
- Pydantic 2.1.1 for data validation

**Infrastructure:**
- Docker & Docker Compose
- LocalStack for local AWS services (DynamoDB, S3, SQS, etc.)
- DynamoDB Admin interface

## Project Structure

```
/Users/kazu/work/nextjs-pj/kz-bz-pj/kz-bz-english-2/kz-bz-english2/
├── frontend/                 # Next.js frontend application
│   ├── app/                 # Next.js App Router structure
│   │   ├── concept/         # Concept page
│   │   ├── contact/         # Contact page
│   │   ├── lessons/         # Lessons page
│   │   ├── news/            # News page
│   │   ├── pricing/         # Pricing page
│   │   ├── signin/          # Sign-in page
│   │   ├── signup/          # Sign-up page
│   │   ├── layout.tsx       # Root layout component
│   │   ├── page.tsx         # Home page
│   │   └── globals.css      # Global styles
│   ├── components/          # Reusable React components
│   │   ├── Concept.tsx
│   │   ├── Contacts.tsx
│   │   ├── Home.tsx
│   │   ├── Lessons.tsx
│   │   ├── MenuBar.tsx      # Navigation component
│   │   ├── NewsPage.tsx
│   │   ├── Signup.tsx
│   │   └── Slideshow.tsx
│   ├── styles/              # CSS modules and styles
│   ├── data/                # Static data files
│   ├── fonts/               # Custom fonts
│   ├── package.json
│   ├── tsconfig.json
│   ├── tailwind.config.ts
│   └── Dockerfile
├── backend/                 # Python FastAPI backend
│   └── app/
│       ├── contact/         # Contact feature module
│       │   ├── domain/      # Domain models
│       │   ├── usecase/     # Business logic
│       │   ├── interface/   # API routes
│       │   └── infrastructure/ # Data access layer
│       ├── submit_application/ # Application submission feature
│       ├── signin/          # Authentication feature
│       ├── main.py          # FastAPI application entry point
│       ├── requirements.txt
│       └── Dockerfile
├── localstack/
│   └── init-aws.sh         # LocalStack initialization script
├── docker-compose.yml      # Multi-service orchestration
└── README.md
```

## Development Setup

### Prerequisites
- Docker & Docker Compose
- Node.js 18+ (for local development)
- Python 3.11+ (for local development)

### Quick Start

1. **Start all services with Docker Compose:**
   ```bash
   docker-compose up -d
   ```

2. **Services will be available at:**
   - Frontend: http://localhost:3030
   - Backend API: http://localhost:8080
   - LocalStack: http://localhost:4566
   - DynamoDB Admin: http://localhost:8001

### Frontend Development

**Directory:** `/frontend/`

**Key Commands:**
```bash
npm run dev          # Start development server (port 3030)
npm run build        # Build for production
npm run start        # Start production server
npm run lint         # Run ESLint
```

**Configuration Files:**
- `package.json` - Dependencies and scripts
- `tsconfig.json` - TypeScript configuration
- `next.config.ts` - Next.js configuration with Cloudinary setup
- `tailwind.config.ts` - Tailwind CSS configuration

**Key Features:**
- Uses Next.js App Router (not Pages Router)
- Material-UI for component library
- Custom fonts: Geist Sans, Geist Mono, Corporate Logo Rounded, Fuwa Moco
- Cloudinary integration for image optimization
- Responsive design with Tailwind CSS

### Backend Development

**Directory:** `/backend/app/`

**Key Commands:**
```bash
# Via Docker (recommended):
docker-compose up backend

# Local development:
cd backend/app
pip install -r requirements.txt
uvicorn main:app --host 0.0.0.0 --port 8080 --reload
```

**Architecture Pattern:**
- Clean Architecture with Domain-Driven Design
- Each feature has: domain/, usecase/, interface/, infrastructure/
- FastAPI with automatic OpenAPI documentation
- DynamoDB for data persistence via LocalStack

**API Endpoints:**
- `GET /` - Root endpoint
- `POST /api/contact` - Contact form submission
- Additional endpoints for signin and application submission

### Database Setup

The project uses DynamoDB with LocalStack for local development:

**Tables Created by init-aws.sh:**
- `t_contact` - Contact form submissions
- `t_submit_application` - Course applications  
- `t_user_information` - User profiles
- `t_user_sginin_history` - Sign-in history

**Access DynamoDB Admin:**
Visit http://localhost:8001 to browse and manage DynamoDB tables.

## Environment Configuration

The project uses a `.env` file for configuration (not included in repository).

**Required Environment Variables:**
- AWS configuration for LocalStack
- Cloudinary settings for frontend
- Other service-specific configurations

## Docker Configuration

**Services in docker-compose.yml:**
- **frontend**: Next.js app (port 3030)
- **backend**: FastAPI app (port 8080)  
- **localstack**: AWS services emulation (port 4566)
- **dynamodb_admin**: Database management UI (port 8001)

## Development Workflow

1. **Feature Development:**
   - Frontend features go in `/frontend/app/` or `/frontend/components/`
   - Backend features follow the clean architecture pattern in `/backend/app/`

2. **Styling:**
   - Use Material-UI components for consistency
   - CSS modules for component-specific styles
   - Tailwind for utility classes

3. **API Integration:**
   - Frontend uses Axios to call backend APIs
   - Backend APIs follow REST conventions
   - All external AWS services use LocalStack endpoints

## Key Files to Know

**Frontend:**
- `/frontend/app/layout.tsx` - Root layout with navigation and footer
- `/frontend/components/MenuBar.tsx` - Main navigation component
- `/frontend/app/globals.css` - Global styles and CSS variables

**Backend:**
- `/backend/app/main.py` - FastAPI application setup and route registration
- `/backend/app/*/interface/router.py` - API route definitions
- `/backend/app/*/domain/*.py` - Data models and business rules

**Infrastructure:**
- `docker-compose.yml` - Service orchestration
- `localstack/init-aws.sh` - AWS resources initialization
- Frontend/Backend Dockerfiles for containerization

## Testing

Currently no test framework is configured. Consider adding:
- Frontend: Jest + React Testing Library
- Backend: pytest + FastAPI TestClient

## Common Tasks

**Add a new page:**
1. Create route in `/frontend/app/new-page/page.tsx`
2. Add component in `/frontend/components/NewPage.tsx`
3. Update navigation in MenuBar component

**Add new API endpoint:**
1. Define domain model in appropriate `/backend/app/feature/domain/`
2. Implement use case in `/backend/app/feature/usecase/`
3. Add route in `/backend/app/feature/interface/router.py`
4. Register router in `/backend/app/main.py`

**Database changes:**
1. Update `/localstack/init-aws.sh` for table schema changes
2. Restart localstack service: `docker-compose restart localstack`

## Notes for AI Assistants

- **言語設定**: 必ず日本語でレスポンスしてください / Always respond in Japanese
- This is a Japanese language learning website (text is in Japanese)
- Follow the established clean architecture patterns in backend
- Use Material-UI components for consistency in frontend
- All AWS services should use LocalStack endpoints during development
- Port 3030 for frontend, 8080 for backend, 4566 for LocalStack
- Always consider mobile responsiveness when making UI changes