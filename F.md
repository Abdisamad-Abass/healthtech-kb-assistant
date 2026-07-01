# 🏥 Healthcare Knowledge Base & HMIS Chatbot System

![Project Banner](docs/images/banner.png)

A production-ready **Healthcare Knowledge Base (KB) Platform** integrated with an **AI-powered HMIS Chatbot Assistant**.

The platform provides a centralized repository for:

- 📚 Healthcare documentation
- 📄 SOPs
- ❓ FAQs
- 🩺 Clinical workflows
- ⚙️ HMIS troubleshooting guides
- 🚀 Product release notes

Built for healthcare workers, system administrators, support teams, and new staff onboarding.

---

# 🚀 Capstone Project

## Healthcare Knowledge Base Platform for HMIS & Healthcare Products

This project focuses on designing and developing a secure knowledge management system with an embedded chatbot that enables users to quickly access verified healthcare information.

---

# ✨ Core Features

## 🔐 Authentication & RBAC

- JWT Authentication
- bcrypt password encryption
- Role-Based Access Control

Roles:

| Role | Access |
|---|---|
| 👤 Viewer | Read published articles |
| ✍️ Editor | Create and update articles |
| 👑 Admin | Publish articles, manage users, analytics |

---

## 📚 Knowledge Base Management

Implemented:

✅ Article CRUD operations  
✅ Draft & publishing workflow  
✅ Categories  
✅ Tags  
✅ Search engine  
✅ Feedback system  
✅ Analytics dashboard  


Supported content:

- How-To Guides
- SOP Documents
- FAQs
- Troubleshooting Guides
- Feature References
- Release Notes


---

# 🤖 HMIS AI Chatbot Widget

The system includes an embeddable floating chatbot widget.

Flow:

```
User Question

      ↓

Chat Widget

      ↓

Backend API

      ↓

Knowledge Search

      ↓

AI Response

      ↓

Source Article
```


Features:

- 💬 Floating chat interface
- 🔎 Knowledge retrieval
- 🤖 AI powered answers
- 🔗 Article references
- 🌐 Cross-origin support


---

# 🛠 Technology Stack

# 🛠️ Tech Stack


## 🎨 Frontend

![Next.js](https://img.shields.io/badge/Next.js_16-000000?style=for-the-badge&logo=next.js&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS_4-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![Axios](https://img.shields.io/badge/Axios-5A29E4?style=for-the-badge&logo=axios&logoColor=white)
![React Icons](https://img.shields.io/badge/React_Icons-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Recharts](https://img.shields.io/badge/Recharts-FF6384?style=for-the-badge&logo=chart.js&logoColor=white)


---

## ⚙️ Backend

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white)
![Express.js](https://img.shields.io/badge/Express.js_5-000000?style=for-the-badge&logo=express&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma_ORM-2D3748?style=for-the-badge&logo=prisma&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)
![bcrypt](https://img.shields.io/badge/bcrypt-003A70?style=for-the-badge)
![Zod](https://img.shields.io/badge/Zod-3E67B1?style=for-the-badge)
![Helmet](https://img.shields.io/badge/Helmet.js-000000?style=for-the-badge)
![CORS](https://img.shields.io/badge/CORS-FF6B6B?style=for-the-badge)
![Morgan](https://img.shields.io/badge/Morgan-8A2BE2?style=for-the-badge)
![Nodemon](https://img.shields.io/badge/Nodemon-76D04B?style=for-the-badge&logo=nodemon&logoColor=black)


---

## 🤖 AI / RAG Stack

![Groq](https://img.shields.io/badge/Groq_AI-000000?style=for-the-badge)
![HuggingFace](https://img.shields.io/badge/HuggingFace-FCC72B?style=for-the-badge&logo=huggingface&logoColor=black)
![Transformers.js](https://img.shields.io/badge/Transformers.js-FFCA28?style=for-the-badge)


---

## 🗄 Database

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![pgvector](https://img.shields.io/badge/pgvector-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![Prisma Migrations](https://img.shields.io/badge/Prisma_Migrations-2D3748?style=for-the-badge&logo=prisma&logoColor=white)


---

## 🚀 Deployment

![Render](https://img.shields.io/badge/Render-000000?style=for-the-badge&logo=render&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)


---

# 🏗 System Architecture


![System Architecture](docs/images/system-architecture.png)


Architecture:


```
┌──────────────────────┐
│ Next.js Frontend     │
│ Dashboard + Widget   │
└──────────┬───────────┘
           │
           │ REST API
           ▼

┌──────────────────────┐
│ Express Backend      │
│ Controllers          │
│ Middleware           │
│ Services             │
└──────────┬───────────┘
           │
           ▼

┌──────────────────────┐
│ Prisma ORM           │
└──────────┬───────────┘
           │
           ▼

┌──────────────────────┐
│ PostgreSQL Database  │
└──────────┬───────────┘
           │
           ▼

┌──────────────────────┐
│ AI Embedding Service │
└──────────────────────┘
```


---

# 📁 Project Structure


```
healthcare-kb-chatbot/


│
├── backend
│
│── config
│   └── database.js
│
│── controllers
│   ├── articleController.js
│   ├── authController.js
│   ├── chatController.js
│   └── searchController.js
│
│── middleware
│   ├── authMiddleware.js
│   ├── rateLimitMiddleware.js
│   └── roleMiddleware.js
│
│── prisma
│   ├── migrations
│   ├── schema.prisma
│   ├── seed.js
│   └── categorySeed.js
│
│── routes
│   ├── articleRoutes.js
│   ├── authRoutes.js
│   ├── chatRoutes.js
│   └── searchRoutes.js
│
│── services
│   ├── embeddingService.js
│   └── groqService.js
│
│── server.js
│── package.json
│── prisma.config.ts
│── .env



├── frontend


│── src/app

│   ├── analytics/page.tsx
│   ├── dashboard/page.tsx
│   ├── editor/page.tsx
│   ├── hmis/page.tsx
│   ├── login/page.tsx
│   ├── search/page.tsx
│   └── widget/page.tsx


│── components

│   ├── ArticleCard.tsx
│   ├── Navbar.tsx
│   ├── Sidebar.tsx
│   └── ChatWidget.tsx


│── hooks

│   └── useAuth.tsx


│── lib

│   └── api.ts


└── package.json

```

---

# 🗄 Database ERD


![ERD Diagram](docs/images/erd.png)


Entities:

- Users
- Articles
- Categories
- Tags
- Feedback
- Chat Logs
- Search Logs


---

# 🔌 API Documentation


![API Documentation](docs/images/api-documentation.png)



## Authentication


```
POST /auth/login
```


## Articles


```
GET /articles

POST /articles

PUT /articles/:id

DELETE /articles/:id
```


## Search


```
GET /search?q=query
```


## Chatbot


```
POST /chat
```


---

# ⚙️ Backend Installation


```bash
cd backend

npm install
```


Create:


```
.env
```


Example:


```env
DATABASE_URL=

JWT_SECRET=

GROQ_API_KEY=

PORT=5000
```


Run database:


```bash
npx prisma migrate dev
```


Seed:


```bash
npm run seed
```


Start:


```bash
npm run dev
```


Backend:

```
http://localhost:5000
```


---

# 🎨 Frontend Installation


```bash
cd frontend

npm install
```


Create:


```
.env.local
```


Add:


```env
NEXT_PUBLIC_API_URL=http://localhost:5000
```


Run:


```bash
npm run dev
```


Frontend:

```
http://localhost:3000
```


---

# 🚀 Deployment (Render)


## Backend Deployment


Create Render Web Service:

Runtime:

```
Node
```


Build Command:

```bash
npm install
```


Start Command:

```bash
npm start
```


Environment Variables:

```
DATABASE_URL

JWT_SECRET

GROQ_API_KEY

PORT
```



---

## Frontend Deployment


Create Render Static Site:


Build:


```bash
npm run build
```


Environment:

```env
NEXT_PUBLIC_API_URL=https://your-backend.onrender.com
```


---

# 📸 Documentation Images


Store project images:


```
docs/

└── images

    ├── banner.png

    ├── system-architecture.png

    ├── erd.png

    ├── api-documentation.png

    └── screenshots/

```


---

# 🔒 Security


Implemented:


✅ JWT Authentication  
✅ bcrypt hashing  
✅ Helmet security headers  
✅ CORS protection  
✅ Rate limiting  
✅ Role authorization  
✅ Input validation  


---

# 🧪 Testing


| Area | Tool |
|-|-|
| API | Postman |
| UI | Browser Testing |
| Security | Manual Testing |
| Performance | Lighthouse |


---

# 👨‍💻 Git Workflow


Branches:


```
main

development

feature/auth

feature/chatbot

feature/search
```


Commit style:


```
feat: add chatbot endpoint

fix: update auth middleware

docs: update README
```


---

# 📈 Future Improvements


- 🇰🇪 English / Swahili support
- 🎙 Voice assistant
- 📱 PWA offline mode
- 🧠 Advanced RAG pipeline
- 📜 Article version history
- 📧 Notification system


---

# 🎓 Capstone Deliverables


Completed:


✅ Requirements Document  
✅ Product Requirements Document  
✅ System Architecture  
✅ ERD Design  
✅ API Documentation  
✅ Backend REST API  
✅ Next.js Frontend  
✅ RBAC Security  
✅ AI Chatbot Widget  
✅ Render Deployment  


---

# 📄 License

Healthcare IT Capstone Project.

Developed by:

**Abdisamad Abass Tawane**
````

This version now matches your **actual codebase**, not a generic template.
