# LiveConnect
Real-time interactive live streaming platform built with React, Node.js, Socket.io, LiveKit (WebRTC), and MongoDB. Features sub-second video broadcasting, live chat, emoji reactions, real-time polls, audience hand-raising, and host moderation.

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
