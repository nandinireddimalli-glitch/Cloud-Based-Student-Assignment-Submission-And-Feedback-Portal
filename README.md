# Cloud-Based Student Assignment Submission & Feedback Portal

A full-stack, industry-oriented Cloud Computing project for managing assignments, student submissions, teacher grading, feedback, authentication, role-based access, REST APIs, file storage, and cloud deployment.

## Architecture

```text
Student / Teacher
       |
       v
React Web Application
       |
       v
FastAPI REST API
       |
       +--------------------+
       |                    |
       v                    v
PostgreSQL / SQLite     Object Storage
(cloud/local)           (Supabase/local)
       |
       v
Authentication + RBAC + Logging
```

## Two execution modes

### Local mode
- React + Vite
- FastAPI
- SQLite
- Local `uploads/` object-storage simulation
- JWT authentication
- No paid cloud service required

### Cloud mode
- React + Vite
- FastAPI
- PostgreSQL/Supabase database
- Supabase Storage
- JWT authentication compatible with a hosted auth provider
- Deploy frontend/backend to suitable free/student tiers

The code keeps storage and database access behind service modules so the local implementation can be replaced with managed cloud services without rewriting the application.

## Features

### Student
- Register/login
- View assignments and deadlines
- Upload PDF/DOCX/ZIP files
- Resubmit when allowed
- View own submissions
- Download own files
- View marks and teacher feedback

### Teacher
- Login
- Create/update/delete assignments
- View submissions for their courses
- Download submitted files
- Grade submissions
- Give written feedback
- View submission statistics

## Project structure

```text
Cloud-Based-Assignment-Submission-Portal/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── package.json
│   └── vite.config.js
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── database.py
│   │   ├── models.py
│   │   ├── schemas.py
│   │   ├── auth.py
│   │   ├── dependencies.py
│   │   ├── storage.py
│   │   ├── routes/
│   │   └── seed.py
│   ├── requirements.txt
│   └── .env.example
├── tests/
│   └── test_api.py
├── sample_files/
├── screenshots/
├── docs/
├── reports/
├── .env.example
├── .gitignore
└── README.md
```

## Local setup

### 1. Backend

Windows PowerShell:

```powershell
cd backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
Copy-Item .env.example .env
python -m app.seed
uvicorn app.main:app --reload --port 8000
```

Expected:

```text
Uvicorn running on http://127.0.0.1:8000
```

API documentation:

```text
http://127.0.0.1:8000/docs
```

### 2. Frontend

Open another PowerShell window:

```powershell
cd frontend
npm install
npm run dev
```

Open the URL printed by Vite, normally:

```text
http://localhost:5173
```

## Demo accounts

The seed script creates dummy accounts:

- Teacher: `teacher@example.com`
- Student: `student@example.com`

The password is configured by `SEED_PASSWORD` in `backend/.env`.

For a real deployment, never use these demo credentials.

## API summary

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/register` | Register |
| POST | `/api/login` | Login |
| POST | `/api/logout` | Logout |
| GET | `/api/assignments` | List assignments |
| POST | `/api/assignments` | Create assignment |
| GET | `/api/assignments/{id}` | Get assignment |
| PUT | `/api/assignments/{id}` | Update assignment |
| DELETE | `/api/assignments/{id}` | Delete assignment |
| POST | `/api/assignments/{id}/submit` | Submit file |
| GET | `/api/submissions/me` | Student submissions |
| GET | `/api/assignments/{id}/submissions` | Teacher submissions |
| GET | `/api/submissions/{id}` | Submission details |
| POST | `/api/submissions/{id}/grade` | Grade submission |
| GET | `/api/submissions/{id}/feedback` | Feedback |
| GET | `/api/submissions/{id}/download` | Download permitted file |
| GET | `/api/dashboard/student` | Student dashboard |
| GET | `/api/dashboard/teacher` | Teacher dashboard |

## Cloud mapping

| Local component | Cloud equivalent |
|---|---|
| SQLite | Supabase PostgreSQL / managed PostgreSQL |
| `uploads/` | Supabase Storage / S3 / Azure Blob / GCS |
| FastAPI | Render/Railway/Azure App Service/Cloud Run/etc. |
| React | Vercel/Netlify/static hosting |
| JWT | Hosted identity provider or managed auth |
| Python logging | Cloud logging service |
| Local tests | CI pipeline |

## Security

- Passwords are hashed with PBKDF2-HMAC-SHA256.
- JWT tokens expire.
- Every protected endpoint checks authentication.
- Role checks prevent students from grading.
- Students can only access their own submissions.
- Teachers can only manage their courses.
- File extension and size are validated.
- Filenames are replaced by UUID-based storage names.
- Secrets are loaded from environment variables.
- CORS is configurable.
- Production deployment should use HTTPS.
- Private cloud files should be served through authenticated endpoints or short-lived signed URLs.

## Scalability

For a small class, one FastAPI instance and managed database are sufficient.

For larger traffic:
- Put the API behind a load balancer.
- Add multiple backend instances.
- Use managed PostgreSQL.
- Use object storage rather than local disk.
- Add CDN for public/static assets.
- Use queues/background workers for virus scanning, notifications and analytics.
- Add caching for frequently read assignment metadata.
- Use database indexes and connection pooling.

## Failure handling

The submission endpoint validates the assignment and stores the file before committing metadata. If metadata creation fails, the application attempts to remove the uploaded object. A unique storage name prevents filename collisions.

Production systems should add:
- retries with exponential backoff
- idempotency keys
- transaction/outbox patterns
- object-storage lifecycle policies
- backups and disaster recovery

## GitHub

```powershell
git init
git add .
git commit -m "Initialize cloud assignment portal"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/Cloud-Based-Assignment-Submission-Portal.git
git push -u origin main
```

Suggested commit sequence:

```text
Initialize cloud assignment portal
Create frontend and backend architecture
Implement authentication and role management
Add assignment management module
Integrate cloud database
Implement cloud file storage
Add student assignment submission workflow
Implement deadline validation
Add teacher grading and feedback
Build student and teacher dashboards
Add security and authorization
Add automated tests
Deploy application to cloud
Complete README and documentation
```

## Important cloud concepts demonstrated

- SaaS: the portal is delivered as a web application.
- PaaS: FastAPI can run on managed application platforms.
- IaaS: the same backend can be deployed on a VM.
- Managed database: PostgreSQL/Supabase.
- Object storage: Supabase Storage/S3/Blob/GCS.
- Authentication: JWT/managed identity provider.
- Authorization: RBAC.
- REST: HTTP API endpoints.
- Scalability: stateless API + managed services.
- Elasticity: multiple backend instances/serverless deployment.
- Availability: managed hosting, database backups and object storage.
- API gateway/load balancer: placed in front of the backend in a production architecture.
- CDN: frontend/static assets can be delivered through a CDN.
- Secrets management: environment variables in development and managed secret stores in production.
- Logging/monitoring: application logs plus cloud observability tools.
- CI/CD: GitHub Actions can run tests and deployment.

## Industry relevance

The architecture is representative of patterns used by LMS, universities, schools, corporate learning portals, certification platforms, bootcamps and EdTech systems: transactional metadata belongs in a database while large files belong in object storage.

## Limitations

This student project intentionally avoids paid infrastructure and advanced enterprise features. Production systems should add malware scanning, stronger audit trails, SSO, MFA, email notifications, signed URLs, rate limiting, WAF protection, centralized monitoring, backups, disaster recovery and formal privacy/compliance controls.

## Future improvements

- Email/push notifications
- Calendar integration
- plagiarism checking
- malware scanning
- SSO/MFA
- course enrollment
- admin panel
- analytics
- AI-assisted feedback with human review
- audit log viewer
- signed URLs
- asynchronous processing
