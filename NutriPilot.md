# Nutri Pilot – AI-powered Nutrition Planning Platform

Nutri Pilot is a health-oriented application designed to assess the reasonableness of dietary plans by integrating user-provided recipe information with personal health profiles to deliver data-driven nutritional insights.

## Demos

- Live Demo: https://www.youtube.com/watch?v=PpI3tH1lzD8 (YouTube walkthrough, 3min)
- GitHub Repo: 
  - https://github.com/tie0913/nuitri_pilot_frontend
  - https://github.com/tie0913/nuitri_pilot_backend
  - https://github.com/tie0913/mongo-replicaset-signalnode

## Tech Stack

| Layer | Technology |
|------|------------|
| Backend | FastAPI (async)|
| Database | MongoDB |
| Frontend | Flutter |
| Deployment | Docker Compose |
| AI Model | ChatGPT 4o-mini|

## System Architecture

![Architecture](./images/nutri_pilot/Architecture.jpg)

## Unified Domain Model（E-R View）

![Unified Domain Model](./images/nutri_pilot/Domain.jpg)

This diagram represents the core business entities of Nutri Pilot and how they collaborate to support authentication, password recovery, and AI-powered nutrition
advice generation. Rather than focusing on physical database design, the model captures the essential domain concepts that drive the current MVP.

### Design Highlights

- **User** is the central aggregate root, representing account identity and linking to all security, wellness, and AI-generated data.
- **Wellness** encapsulates the user’s health context and aggregates structured medical information such as **Chronic** conditions and **Allergies**, allowing the health profile to evolve without impacting core user data.
- **Session** and **Otp** are modeled as independent security entities to clearly separate authentication state and one-time verification flows.
- **Suggestions** represents persisted AI advice records, forming the final output of the AI interaction pipeline.

This unified domain model serves as the foundation for all critical workflows, including authentication, password recovery, and AI-powered nutrition guidance.


## Core System Workflow

Before implementation, I designed key system workflows to ensure security, scalability, and maintainability. Only the most critical flows are documented here.

### 1. Reset Password – Secure Identity Recovery

![Reset Password Flow](./images/nutri_pilot/sequences/usermanagement/Reset-Password.png)

**Why this matters**

- Prevents user enumeration attacks  
- OTP is single-use and deleted immediately after validation  
- Expired OTP is cleaned up automatically to avoid replay attacks  

---

### 2. Authentication Filter – Centralized Trust Boundary

![Authentication Filter](./images/nutri_pilot/sequences/Authentication.jpg)

**Why this matters**

- Authentication logic is decoupled from business controllers  
- All protected endpoints share a single trust boundary  
- Invalid sessions never reach business logic  

---

### 3. Getting Advice from AI – Core Business Pipeline

![AI Advice Flow](./images/nutri_pilot/sequences/AI-Advice.jpg)

**Why this matters**

- Isolates AI inference from user request lifecycle  
- Enables async execution and future queue-based scaling  
- Allows caching of generated advice for cost optimization


## Key Engineering Challenges

### Context Handling in AI Interaction

Nutri Pilot does not maintain long-lived AI sessions.

Instead, only the user’s chronic conditions and allergy information are stored as structured fields in the database. When the user submits a food image, these fields are sent together with the image to the AI model. This avoids keeping long conversational context in memory and keeps the backend stateless.

### Image Handling Strategy

When a user uploads a food image, the system generates two derived versions:

- A compressed image optimized for AI inference to reduce token usage  
- A thumbnail image stored in MongoDB (planned to be migrated to a file server) for
  displaying user history

This approach reduces AI cost while keeping the UI responsive.



## My Contributions
- Designed system architecture from scratch
- Implemented frontend application and backend services
- Deployed the platform using Docker
