Here is your **Database Design Document** based on the PRD, using **Supabase (PostgreSQL)** and **Prisma ORM**.

---

# 🗂️ Database Design Document

**Product**: ConnectNow MVP
**Tools**: Supabase + Prisma ORM (Node.js)
**Version**: v1.0 (MVP)

---

## 📌 Summary

This document outlines the database schema for the ConnectNow MVP, a chat and video communication platform using Supabase with Prisma ORM. The schema includes tables for users, messages, chat sessions, video calls, and contact relationships. The design focuses on supporting:

* Google OAuth authentication
* 1:1 real-time messaging
* 1:1 video calling
* Extensible relations for group support and tool integration in future versions

---

## 🧱 Entity Relationship Diagram (ERD)

```plaintext
User ───< ChatSession >─── User
  │                           │
  │                           │
Message                      VideoCall
```

---

## 📄 Prisma Schema

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id             String       @id @default(uuid())
  email          String       @unique
  name           String
  profilePicture String?
  createdAt      DateTime     @default(now())
  updatedAt      DateTime     @updatedAt
  messages       Message[]
  chatSessionsA  ChatSession[] @relation("UserA")
  chatSessionsB  ChatSession[] @relation("UserB")
  videoCalls     VideoCall[]  @relation("Caller")
  videoReceived  VideoCall[]  @relation("Receiver")
}

model ChatSession {
  id         String    @id @default(uuid())
  userA      User      @relation("UserA", fields: [userAId], references: [id])
  userAId    String
  userB      User      @relation("UserB", fields: [userBId], references: [id])
  userBId    String
  messages   Message[]
  createdAt  DateTime  @default(now())
  updatedAt  DateTime  @updatedAt

  @@unique([userAId, userBId])
}

model Message {
  id           String       @id @default(uuid())
  chatSession  ChatSession  @relation(fields: [chatSessionId], references: [id])
  chatSessionId String
  sender       User         @relation(fields: [senderId], references: [id])
  senderId     String
  content      String
  seen         Boolean      @default(false)
  createdAt    DateTime     @default(now())
}

model VideoCall {
  id         String   @id @default(uuid())
  caller     User     @relation("Caller", fields: [callerId], references: [id])
  callerId   String
  receiver   User     @relation("Receiver", fields: [receiverId], references: [id])
  receiverId String
  startedAt  DateTime @default(now())
  endedAt    DateTime?
  status     CallStatus

  @@index([callerId, receiverId])
}

enum CallStatus {
  ONGOING
  ENDED
  MISSED
  REJECTED
}
```

---

## 📝 Table Overview

### 1. **User**

* Stores user info from Google login.
* `chatSessionsA` and `chatSessionsB` enable bidirectional 1:1 chat tracking.

### 2. **ChatSession**

* Represents a unique 1:1 chat between two users.
* Enforces uniqueness of the pair (A, B).
* Basis for real-time message stream.

### 3. **Message**

* Stores each text message, sender info, and seen status.
* Linked to a ChatSession.

### 4. **VideoCall**

* Tracks each 1:1 video call session.
* Includes call status (ongoing, ended, missed, rejected).

---

## 🧪 Supabase-Specific Notes

* Supabase provides auth with Google using `supabase.auth.signInWithOAuth()`.
* Supabase Postgres will sync with this schema; Prisma can read/write using `DATABASE_URL`.
* You can store files (e.g. images sent in messages) in **Supabase Storage**, referencing file URLs in `Message`.

---

## 🧯 Security & Indexing

* Prisma’s generated queries handle SQL injection safety.
* Indexes added on critical lookup fields: `callerId`, `receiverId`.
* Consider RLS (Row-Level Security) policies in Supabase for additional protection.

---

Would you like:

* A seed script for initial test data?
* The ERD as a visual PNG?
* Integration code for Prisma client in your backend API?

Let me know what to generate next.
