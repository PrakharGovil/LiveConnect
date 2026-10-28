# 🎥 LiveConnect — Interactive Live Streaming Platform

**LiveConnect** is a full-stack, low-latency interactive live streaming platform that combines **YouTube Live / Twitch-style broadcasting** with **Zoom-style audience interaction**. Built with **WebRTC (LiveKit)**, **Socket.io**, **React 18**, **Node.js/Express**, and **MongoDB**, it enables hosts to broadcast live video while engaging viewers through real-time chat, animated emoji reactions, live polls, moderation controls, and stage hand-raising.

---

## ✨ Key Features

- **🔴 Sub-Second WebRTC Video Streaming**: Powered by LiveKit SFU with role-based publisher (Host) and subscriber (Viewer) token generation.
- **💬 Real-Time Live Chat**: Room-scoped instant messaging via Socket.io with MongoDB persistence and chat history pagination.
- **🔥 Animated Emoji Reactions**: Floating real-time reactions (`👍`, `❤️`, `😂`, `🔥`) synchronized across all viewers in the stream room.
- **📊 Live Interactive Polls**: Hosts can launch multi-option polls in real time; viewers vote with instant percentage bar updates.
- **✋ Audience Hand-Raise & Stage Requests**: Viewers can request to join the stage, and hosts can approve or decline requests live from the studio panel.
- **🛡️ Host Moderation Suite**: Real-time message deletion, user muting, and stream-level bans enforced on both the API and WebSocket layers.
- **🔐 Secure Role-Based Auth**: Dual-token JWT authentication (`accessToken` + `httpOnly` refresh cookie), Zod request validation, Helmet CSP, and rate limiting.
- **🐳 Dockerized Monorepo**: Ready-to-run `docker-compose.yml` orchestrating the React client, Express API server, and MongoDB.

---

## 🏗️ System Architecture

```text
                        ┌──────────────────────────────────────┐
                        │            Browser Client            │
                        │    (React 18 + Vite + Tailwind)      │
                        └───┬──────────────┬──────────────┬────┘
                            │              │              │
                    REST API│     WebSocket│        WebRTC│
                    (Axios) │  (Socket.io) │     (LiveKit)│
                            ▼              ▼              ▼
                    ┌──────────────┐┌──────────────┐┌──────────────┐
                    │Express Server││Socket Server ││ LiveKit SFU  │
                    │ (Port 3000)  ││ (Port 3000)  ││ (Cloud/Host) │
                    └───────┬──────┘└──────┬───────┘└──────────────┘
                            │              │
                            ▼              ▼
                        ┌──────────────────────┐
                        │     MongoDB 7.0      │
                        └──────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technologies |
| :--- | :--- |
| **Frontend** | React 18, TypeScript, Vite, Tailwind CSS, React Router v6, Axios |
| **Video / WebRTC** | LiveKit Client SDK (`livekit-client`, `@livekit/components-react`), LiveKit Server SDK |
| **Real-Time Engine** | Socket.io (`socket.io`, `socket.io-client`) |
| **Backend** | Node.js, Express.js, TypeScript, Zod, Helmet, Express Rate Limit |
| **Database** | MongoDB, Mongoose ODM |
| **Auth & Testing** | JSON Web Tokens (JWT), bcrypt, Vitest |
| **DevOps** | Docker, Docker Compose, Nginx, npm Workspaces |

---

## 📂 Project Structure

```text
liveconnect/
├── client/                  # React 18 + TypeScript + Vite frontend
│   ├── src/
│   │   ├── components/      # StreamCard, PollCreator, PollDisplay, ReactionPicker, ModerationPanel, HandRaisePanel
│   │   ├── contexts/        # AuthContext & SocketContext
│   │   ├── hooks/           # Custom hooks (useAuth, useStreams)
│   │   ├── pages/           # Landing, Home, HostDashboard, HostStream, WatchStream, Login, Register
│   │   ├── services/        # Axios API client & Socket.io service
│   │   └── types/           # Shared TypeScript interfaces
│   └── Dockerfile
├── server/                  # Node.js + Express + TypeScript backend
│   ├── src/
│   │   ├── config/          # MongoDB, Socket.io event handlers, LiveKit token service
│   │   ├── controllers/     # Auth, Stream, Poll, Reaction, Moderation, HandRaise controllers
│   │   ├── middleware/      # JWT auth, Host role guard, Zod validation
│   │   ├── models/          # User, Stream, ChatMessage, Poll, PollVote, Reaction, HandRaise, StreamBan
│   │   ├── routes/          # Express REST routes (/api/auth, /api/streams, /api/polls)
│   │   └── validators/      # Zod request schemas
│   └── Dockerfile
├── docker-compose.yml       # Full-stack container orchestration
└── package.json             # npm workspaces root config
```

---

## 🚀 Getting Started

### Prerequisites
- **Node.js** `>= 20.0.0`
- **MongoDB** (Local instance or MongoDB Atlas URI)
- **LiveKit** account ([LiveKit Cloud](https://cloud.livekit.io/) or self-hosted server)

### 1. Clone & Install Dependencies
```bash
git clone https://github.com/<your-username>/liveconnect.git
cd liveconnect
npm run install:all
```

### 2. Configure Environment Variables

Create `server/.env` (from `server/.env.example`):
```env
MONGODB_URI=mongodb://localhost:27017/liveconnect
JWT_ACCESS_SECRET=your_access_secret_here
JWT_REFRESH_SECRET=your_refresh_secret_here
JWT_ACCESS_EXPIRY=15m
JWT_REFRESH_EXPIRY=7d

LIVEKIT_API_KEY=your_livekit_api_key
LIVEKIT_API_SECRET=your_livekit_api_secret
LIVEKIT_WS_URL=wss://your-project.livekit.cloud

PORT=3000
NODE_ENV=development
FRONTEND_URL=http://localhost:5173
```

Create `client/.env` (from `client/.env.example`):
```env
VITE_API_URL=http://localhost:3000
VITE_LIVEKIT_WS_URL=wss://your-project.livekit.cloud
```

### 3. Run Locally
```bash
npm run dev
```
- **Frontend**: `http://localhost:5173`
- **Backend API**: `http://localhost:3000`

### 4. Run with Docker Compose
```bash
docker compose up --build
```

---

## 🧪 Scripts

| Command | Description |
| :--- | :--- |
| `npm run dev` | Start client and server concurrently in development mode |
| `npm run build` | Compile TypeScript and build production bundles across workspaces |
| `npm run lint` | Run TypeScript type-checking across `client` and `server` |
| `npm test` | Run unit test suites across workspaces |
