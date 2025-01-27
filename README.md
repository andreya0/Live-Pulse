# Live Pulse - Live Streaming Platform

## Overview
This is a full-stack live streaming platform that combines a modern React frontend with a Spring Boot microservice backend. It enables users to stream video, interact via real-time chat, follow streamers, and manage profiles. The system emphasizes event-driven architecture, real-time communication, and scalability, with features like chat message persistence, viewer analytics, and adaptive streaming.

## Key Technologies

### Frontend:
- **React + TypeScript**: Type-safe, modern UI
- **Vite**: Ultra-fast builds and hot reloading
- **React Query**: State and cache management
- **Tailwind CSS**: Responsive, utility-first styling
- **WebSocket**: SockJS/STOMP for live chat
- **Server-Sent Events (SSE)**: For viewer count updates
- **Video.js**: Adaptive HLS/DASH playback

### Backend:
- **Spring Boot**: Microservice architecture
- **Kafka**: Event streaming for chat and stream lifecycle
- **Redis**: Session caching, real-time viewer tracking
- **MongoDB**: User profiles, chat history, streams
- **Nginx-RTMP + FFmpeg**: RTMP ingestion, HLS transcoding
- **Docker Compose**: Orchestration for Kafka, Redis, MongoDB

## Core Features

### 1. Real-Time Interaction
- **Live Chat**: WebSocket-based chat with STOMP protocol. Messages are persisted in MongoDB via Kafka when streams end.
- **Viewer Analytics**: Real-time viewer counts using SSE, tracked via Redis for low-latency updates.
- **Notifications**: Users toggle alerts for followed streamers (Kafka events trigger backend logic).

### 2. Stream Management
- **RTMP/HLS Streaming**: Streamers broadcast via OBS/FFmpeg to Nginx-RTMP; transcoded to HLS for adaptive playback.
- **Thumbnail Generation**: FFmpeg captures snapshots for stream previews.
- **Stream Metadata**: Titles, categories, and start/end times stored in MongoDB.

### 3. User Experience
- **Follow/Unfollow**: Social features with real-time follower count updates.
- **Profile Management**: Stream titles, notification preferences, and chat history.
- **Skeleton Loading**: Smooth UI transitions with Tailwind-powered loading states.

### 4. Event-Driven Architecture
- **Kafka Event Streaming**:
  - Chat messages are buffered in Kafka and persisted post-stream.
  - Stream lifecycle events (start/end) trigger notifications and data cleanup.
- **Redis Pub/Sub**: Handles real-time viewer count aggregation.

## Technical Highlights

- **Scalable Microservices**: Decoupled services for authentication, chat, streaming, and analytics. Dockerized deployment with Kafka for horizontal scaling.
- **Efficient Real-Time Communication**: Hybrid approach: WebSockets (chat) + SSE (viewer counts) + Redis (session management).
- **Resilient Data Flow**: Chat messages stored in MongoDB after the stream ends (Kafka ensures no data loss). Redis cache minimizes database load for high-frequency operations (e.g., viewer counts).
- **Adaptive Video Streaming**: Nginx-RTMP ingests RTMP, transcodes to HLS for compatibility with devices and networks. Video.js player with HLS/DASH support for seamless playback.
- **Production-Ready Practices**:
  - **API Testing**: Postman collections for endpoint validation (auth, streams, chat).
  - **CORS Configuration**: Fine-grained security for API endpoints.
  - **Actuator Integration**: Health checks, metrics, and monitoring.
- **Responsive Design**: Mobile-first UI with Tailwind’s breakpoint utilities. Custom `useMediaQuery` hook for dynamic rendering (e.g., chat panel collapse on mobile).

## Testing & Deployment
- **Postman**: Automated workflows for API testing (JWT auth, stream creation, chat simulation).
- **Docker Compose**: Single-command setup for dev/prod environments (Zookeeper, Kafka, Redis, MongoDB).
- **Nginx Config**: Optimized for RTMP/HLS with failover and load balancing.
