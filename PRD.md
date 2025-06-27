**Chat Application MVP** that supports both video and messaging, with Gmail-based login and extensibility for other tools.
# Product Requirements Document (PRD)

## 1. **Product Overview**

### Title:

**ConnectNow – Real-Time Chat & Video App (MVP)**

### Goal:

Develop a minimum viable product (MVP) of a real-time communication platform (text + video) where users can sign in using Gmail (OAuth) and communicate with others. The app will support 1:1 messaging and video calls, and lay a foundation for integrations with other tools.

### Timeline:

**6 weeks** (42 days)

---

## 2. **Target Audience**

* Remote teams and freelancers
* Students and educators
* Professionals needing quick, reliable communication without the bloat of large platforms

---

## 3. **Core Features (MVP)**

### A. **Authentication**

* [ ] Google OAuth (Gmail Sign-In)
* [ ] Profile: Name, Email, Profile Photo

### B. **Chat (Messaging)**

* [ ] 1:1 messaging
* [ ] Real-time text messaging (WebSockets)
* [ ] Message timestamps
* [ ] Seen/Delivered status
* [ ] Emoji support
* [ ] Basic file upload (images, docs)

### C. **Video Call**

* [ ] Initiate/Receive 1:1 video call
* [ ] Basic video call controls (mute, camera toggle, end call)
* [ ] WebRTC peer-to-peer connection
* [ ] Fallback to TURN server if P2P fails

### D. **Contacts / Add User**

* [ ] See users you've chatted with
* [ ] Search users by email
* [ ] Add/remove from contacts (optional, MVP may auto-start chat with anyone)

---

## 4. **Non-Core (V2/Extension)**

* Group chat/video
* Screen sharing
* Calendar or task tool integrations
* Browser push notifications
* Desktop/mobile apps

---

## 5. **Tech Stack**

### Frontend

* **React.js + TypeScript**
* Tailwind CSS (UI)
* Socket.IO or WebSocket API
* WebRTC (video)

### Backend

* **Node.js + Express**
* Socket.IO server
* REST API for auth, user profiles, chat history
* JWT for auth token management

### Database

* **MongoDB or PostgreSQL**
* Redis (optional, for real-time and performance)

### DevOps

* Vercel/Netlify (frontend)
* Railway/Render/Heroku (backend)
* Firebase or Vonage/Twilio for TURN/STUN (WebRTC)

---

## 6. **Architecture Overview**

```plaintext
Client (React) <---> Backend API (Node.js/Express) <---> DB (Mongo/Postgres)
            |                            |
            |<-------- WebSocket -------->|
                       |     |
                     WebRTC (P2P)
                       |     |
                  TURN/STUN Server
```

---

## 7. **User Stories**

###  Authentication

* As a user, I can log in using my Gmail so I don’t need to create a new account.
* As a user, I can view my profile photo and email after login.

###  Messaging

* As a user, I can search and message another user by their email.
* As a user, I can see the live typing of another person.
* As a user, I can see when my message has been delivered or seen.
* As a user, I can send emojis or attachments (like images).

### 📹 Video

* As a user, I can start a video call with a contact from the chat screen.
* As a user, I can turn off/on my camera and mic.
* As a user, I can end the video call anytime.

---

## 8. **Wireframe (Basic)**

You can use tools like Figma to design the following views:

1. **Login Page** – Google Sign-in button
2. **Home/Contact List**
3. **Chat Screen** – message list, input box, video call button
4. **Video Call Window** – camera preview, control buttons

---

## 9. **Success Criteria**

* Login works via Google
* Two users can message each other in real-time
* Users can initiate and conduct a video call
* App is deployed and functional on a public domain
* No critical bugs or crashes under normal usage

---

## 10. **Milestones (6 Week Plan)**

| Week | Deliverables                                 |
| ---- | -------------------------------------------- |
| 1    | Project setup, Google login, database models |
| 2    | Messaging backend (REST + WebSockets)        |
| 3    | Messaging UI and integration                 |
| 4    | Video call feature (WebRTC)                  |
| 5    | UI polish, file upload, emoji support        |
| 6    | Testing, deployment, feedback loop           |

---

## 11. **Future Scope**

* Group video calls
* Integration with Google Calendar
* AI summarization of chats
* End-to-end encryption

---

