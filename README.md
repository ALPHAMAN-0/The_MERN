# 🚀 MERN Boilerplate

A production-ready, full-stack boilerplate built with **MongoDB**, **Express**, **React (Vite)**, and **Node.js** — featuring JWT authentication, Docker support, ESLint + Prettier, and a clean monorepo structure.

---

## 📋 Table of Contents

- [Tech Stack](#tech-stack)
- [Folder Structure](#folder-structure)
- [Features](#features)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Available Scripts](#available-scripts)
- [API Endpoints](#api-endpoints)
- [Authentication Flow](#authentication-flow)
- [Docker Setup](#docker-setup)
- [Who Should Use This](#who-should-use-this)
- [Project Ideas](#project-ideas)

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, Vite, React Router DOM, Axios |
| Backend | Node.js, Express.js |
| Database | MongoDB, Mongoose ODM |
| Auth | JWT (httpOnly cookies), bcryptjs |
| Code Quality | ESLint (Flat Config), Prettier |
| DevOps | Docker, Docker Compose |
| Runtime | Node.js 20+ |

---

## 📁 Folder Structure

```
mern-boilerplate/
│
├── client/                         # Frontend — React + Vite
│   ├── public/                     # Static assets (favicon, images)
│   ├── src/
│   │   ├── api/
│   │   │   └── axios.js            # Axios instance with baseURL + credentials
│   │   ├── components/             # Reusable UI components
│   │   ├── context/
│   │   │   └── AuthContext.jsx     # Global auth state (user, login, logout)
│   │   ├── hooks/                  # Custom React hooks
│   │   ├── pages/                  # Route-level page components
│   │   ├── routes/
│   │   │   └── ProtectedRoute.jsx  # Redirects unauthenticated users to /login
│   │   ├── App.jsx                 # Root component with router setup
│   │   └── main.jsx                # React DOM entry point
│   ├── .env                        # VITE_API_URL (client-side env vars)
│   ├── eslint.config.js            # ESLint flat config (ESLint 10+)
│   ├── .prettierrc                 # Prettier formatting rules
│   ├── Dockerfile                  # Client Docker image definition
│   └── vite.config.js              # Vite bundler config
│
├── server/                         # Backend — Node.js + Express
│   ├── config/
│   │   └── db.js                   # MongoDB connection via Mongoose
│   ├── controllers/
│   │   └── auth.controller.js      # register, login, logout, getMe logic
│   ├── middleware/
│   │   └── auth.middleware.js      # JWT verification + req.user injection
│   ├── models/
│   │   └── user.model.js           # Mongoose User schema + pre-save hash
│   ├── routes/
│   │   └── auth.routes.js          # Auth route definitions
│   ├── utils/
│   │   └── generateToken.js        # JWT sign + set httpOnly cookie
│   ├── .env                        # Server environment variables
│   ├── Dockerfile                  # Server Docker image definition
│   └── index.js                    # Express app entry point
│
├── docker-compose.yml              # Orchestrates client, server, and MongoDB
├── .dockerignore                   # Files excluded from Docker builds
└── README.md                       # Project documentation
```

---

## ✨ Features

- **JWT Authentication** — Secure login and registration using signed JSON Web Tokens stored in httpOnly cookies (not localStorage), protecting against XSS attacks.
- **Protected Routes** — Client-side route guards that redirect unauthenticated users automatically.
- **Auth Context** — Global React context exposes `user`, `login()`, `logout()`, and a `loading` state to any component via the `useAuth()` hook.
- **Axios Instance** — Pre-configured Axios with the API base URL and `withCredentials: true` so cookies are sent on every request.
- **Mongoose ODM** — Structured data modelling with schema validation, timestamps, and a pre-save hook that hashes passwords automatically.
- **Environment Config** — Separate `.env` files for client and server. Client variables are prefixed with `VITE_` and server variables stay private.
- **ESLint + Prettier** — Flat config ESLint (compatible with ESLint 10) integrated with Prettier so formatting and linting never conflict.
- **Docker Compose** — One command spins up the React client, Express server, and MongoDB together with persistent volume storage.

---

## 🚀 Getting Started

### Prerequisites

- Node.js 20+
- npm 9+
- MongoDB (local) or Docker Desktop

### Run without Docker

```bash
# 1. Clone the repo
git clone https://github.com/your-username/mern-boilerplate.git
cd mern-boilerplate

# 2. Install server dependencies
cd server && npm install

# 3. Install client dependencies
cd ../client && npm install

# 4. Start server (terminal 1)
cd server && npm run dev

# 5. Start client (terminal 2)
cd client && npm run dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser.

### Run with Docker

```bash
# Build and start all services
docker-compose up --build

# Stop all services
docker-compose down

# Stop and remove volumes (clears database)
docker-compose down -v
```

---

## 🔐 Environment Variables

### `server/.env`

```env
PORT=5000
MONGO_URI=mongodb://mongo:27017/mernapp
JWT_SECRET=your_super_secret_key_here
JWT_EXPIRES_IN=7d
CLIENT_URL=http://localhost:5173
NODE_ENV=development
```

### `client/.env`

```env
VITE_API_URL=http://localhost:5000/api
```

> **Note:** Never commit `.env` files to version control. Add them to `.gitignore`.

---

## 📜 Available Scripts

### Server

| Command | Description |
|---|---|
| `npm run dev` | Start server with nodemon (hot reload) |
| `npm start` | Start server in production mode |

### Client

| Command | Description |
|---|---|
| `npm run dev` | Start Vite dev server |
| `npm run build` | Build for production |
| `npm run preview` | Preview production build locally |
| `npm run lint` | Run ESLint on `src/` |
| `npm run lint:fix` | Auto-fix ESLint issues |
| `npm run format` | Format all files with Prettier |

---

## 🔌 API Endpoints

Base URL: `http://localhost:5000/api`

| Method | Endpoint | Access | Description |
|---|---|---|---|
| `POST` | `/auth/register` | Public | Create a new user account |
| `POST` | `/auth/login` | Public | Login and receive JWT cookie |
| `POST` | `/auth/logout` | Public | Clear the JWT cookie |
| `GET` | `/auth/me` | Private | Get currently authenticated user |

### Example: Register

```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice", "email": "alice@example.com", "password": "secret123"}'
```

### Example: Login

```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -c cookies.txt \
  -d '{"email": "alice@example.com", "password": "secret123"}'
```

---

## 🔑 Authentication Flow

```
1. User submits login form
        ↓
2. POST /api/auth/login
        ↓
3. Server validates email + bcrypt password check
        ↓
4. JWT signed → set as httpOnly cookie (7 days)
        ↓
5. Client stores user object in AuthContext
        ↓
6. Subsequent requests send cookie automatically
        ↓
7. auth.middleware.js verifies JWT → injects req.user
        ↓
8. Protected routes accessible
```

On logout, the cookie is cleared server-side and `user` is set to `null` in the context.

---

## 🐳 Docker Setup

The `docker-compose.yml` defines three services:

| Service | Image | Port | Description |
|---|---|---|---|
| `mongo` | mongo:7 | 27017 | MongoDB database with persistent volume |
| `server` | Custom (Node 20) | 5000 | Express API server |
| `client` | Custom (Node 20) | 5173 | React + Vite frontend |

Data is stored in a named Docker volume (`mongo_data`) so your database persists across container restarts.

```bash
# View running containers
docker ps

# View server logs
docker logs server

# Access MongoDB shell
docker exec -it mongo mongosh
```

---

## 🎯 Who Should Use This Boilerplate

This boilerplate is a great starting point for developers who:

- Are **learning full-stack development** and want a clean, well-structured MERN reference.
- Want to **skip repetitive setup** (auth, CORS, env config, Docker) and jump straight into building features.
- Are building a **side project or MVP** and need a solid foundation fast.
- Work in **teams** and need consistent code style enforced via ESLint and Prettier.
- Want **containerised local development** so the environment works the same on every machine.

---

## 💡 Project Ideas Using This Boilerplate

Since authentication, protected routes, and a database are already wired up, this boilerplate is immediately ready for:

| Project | What to Add |
|---|---|
| **Task manager / Todo app** | Task model, CRUD routes, drag-and-drop UI |
| **Blog platform** | Post + Comment models, rich text editor, public/private posts |
| **E-commerce store** | Product, Cart, Order models, Stripe payment integration |
| **Job board** | Listing model, search/filter, employer vs candidate roles |
| **Social media app** | Follow system, feed, likes, notifications (WebSocket) |
| **Expense tracker** | Transaction model, chart dashboard (Recharts/Chart.js) |
| **SaaS starter** | Subscription model, Stripe billing, multi-tenant support |
| **Real-time chat app** | Socket.io integration, Message model, room support |
| **Portfolio CMS** | Admin dashboard to manage projects, skills, and blog posts |
| **Team collaboration tool** | Workspaces, invites, role-based permissions |

---

## 📦 Planned Improvements

- [ ] Refresh token rotation
- [ ] Role-based access control (RBAC)
- [ ] Rate limiting with express-rate-limit
- [ ] Email verification on register
- [ ] Password reset via email (Nodemailer)
- [ ] CI/CD pipeline (GitHub Actions)
- [ ] Production Nginx reverse proxy config
- [ ] Swagger / OpenAPI documentation

---

## 🤝 Contributing

Contributions are welcome! Please open an issue first to discuss what you'd like to change.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add your feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

> Built with ❤️ using MongoDB · Express · React · Node.js