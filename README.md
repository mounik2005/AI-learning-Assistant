# LearnAI - Personalized Learning Assistant

> AI-powered learning platform with Reading Comprehension, Homework Helper, Progress Dashboard & Gamification

## 🏗️ Project Structure

```
LearnAI-PersonalizedLearningAssistant/
├── frontend/                  # React + Vite + TypeScript + Tailwind
│   ├── src/
│   │   ├── components/
│   │   │   ├── layout/        # AppLayout, Sidebar, MobileNav
│   │   │   └── ui/            # shadcn/ui reusable components
│   │   ├── contexts/
│   │   │   └── AuthContext.tsx # Authentication state
│   │   ├── hooks/
│   │   │   └── use-toast.ts   # Toast notification hook
│   │   ├── lib/
│   │   │   └── utils.ts       # Utility functions
│   │   ├── pages/
│   │   │   ├── Index.tsx           # Landing page
│   │   │   ├── Login.tsx           # Login page
│   │   │   ├── Register.tsx        # Registration page
│   │   │   ├── Dashboard.tsx       # Progress dashboard
│   │   │   ├── ReadingComprehension.tsx  # AI quiz generator
│   │   │   ├── HomeworkHelper.tsx   # AI homework assistant
│   │   │   ├── Gamification.tsx     # Badges & leaderboard
│   │   │   ├── Profile.tsx          # User profile
│   │   │   └── NotFound.tsx         # 404 page
│   │   ├── App.tsx
│   │   ├── App.css
│   │   ├── main.tsx
│   │   └── index.css          # Design system (CSS variables)
│   ├── index.html
│   ├── package.json
│   ├── vite.config.ts
│   ├── tailwind.config.ts
│   ├── postcss.config.js
│   ├── tsconfig.json
│   ├── tsconfig.app.json
│   ├── components.json
│   └── .env.example
│
├── backend/                   # AWS Lambda + SAM
│   ├── lambda/
│   │   ├── auth/
│   │   │   └── index.mjs      # Authentication (DynamoDB)
│   │   ├── comprehension/
│   │   │   └── index.mjs      # AI quiz generation (Bedrock)
│   │   ├── homework/
│   │   │   └── index.mjs      # AI homework help (Bedrock)
│   │   └── progress/
│   │       └── index.mjs      # Progress tracking (DynamoDB)
│   ├── template.yaml          # AWS SAM/CloudFormation
│   └── README.md
│
└── README.md                  # This file
```

## 🚀 Quick Start

### Prerequisites
- **Node.js 18+** and **npm**
- **AWS CLI** configured with credentials
- **AWS SAM CLI** (`pip install aws-sam-cli`)
- **Amazon Bedrock** model access enabled (Claude 3 Haiku)

### 1. Run Frontend Locally

```bash
cd frontend
npm install
npm run dev
```
Open http://localhost:5173 in your browser.

> The frontend works standalone with mock data. Connect the backend for real AI features.

### 2. Deploy AWS Backend

```bash
cd backend

# Build and deploy
sam build
sam deploy --guided

# Follow the prompts:
#   Stack Name: learnai-backend
#   Region: us-east-1
#   Allow SAM CLI IAM role creation: Yes

# Note the API URL from the outputs
```

### 3. Connect Frontend to Backend

```bash
cd frontend
cp .env.example .env
# Edit .env and set VITE_API_URL to your API Gateway URL
npm run build
```

## 🏛️ Architecture

```
┌─────────────┐     ┌──────────────┐     ┌─────────────────┐
│   React UI  │────▶│ API Gateway  │────▶│  Lambda Functions│
│  (Frontend) │     │   (REST)     │     │                  │
└─────────────┘     └──────────────┘     │  • auth          │
                                          │  • comprehension │
                                          │  • homework      │
                                          │  • progress      │
                                          └───────┬──────────┘
                                                  │
                              ┌────────────────────┼────────────┐
                              ▼                    ▼            ▼
                        ┌──────────┐       ┌──────────┐  ┌──────────┐
                        │ DynamoDB │       │ Bedrock  │  │    S3    │
                        │ (Users & │       │ (Claude  │  │(Passages)│
                        │ Progress)│       │  AI)     │  │          │
                        └──────────┘       └──────────┘  └──────────┘
```

## 🛠️ AWS Services Used

| Service | Purpose |
|---------|---------|
| **Lambda** | Serverless backend functions |
| **API Gateway** | REST API endpoints |
| **DynamoDB** | NoSQL database (Users, Progress) |
| **S3** | File storage for reading passages |
| **Amazon Bedrock** | AI model (Claude) for questions & explanations |
| **IAM** | Access control & permissions |
| **SageMaker** | (Optional) Custom ML model training |
| **EC2** | (Optional) Hosting for custom services |

## 📊 Database Schema (DynamoDB)

### LearnAI_Users
| Field | Type | Description |
|-------|------|-------------|
| email (PK) | String | Partition key |
| userId | String | UUID |
| name | String | Full name |
| role | String | student / parent / teacher |
| passwordHash | String | Hashed password |
| salt | String | Password salt |
| points | Number | Gamification points |
| badges | List | Earned badge names |
| streak | Number | Learning streak (days) |

### LearnAI_Progress
| Field | Type | Description |
|-------|------|-------------|
| userId (PK) | String | Partition key |
| timestamp (SK) | String | Sort key |
| type | String | quiz / homework |
| score | Number | Correct answers |
| totalQuestions | Number | Total questions |
| subject | String | Subject name |
| pointsEarned | Number | Points earned |

## 💰 Cost (AWS Free Tier)

| Service | Free Tier | Est. Cost |
|---------|-----------|-----------|
| Lambda | 1M req/mo | $0 |
| API Gateway | 1M calls/mo | $0 |
| DynamoDB | 25 GB | $0 |
| S3 | 5 GB | $0 |
| Bedrock | Pay per token | ~$2-10/mo |

## 📄 License

MIT
