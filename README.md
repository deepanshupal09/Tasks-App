# Task Management Application

A full-stack task management application with user authentication, built with Next.js (React) on the frontend and Express.js with TypeScript on the backend. The application uses PostgreSQL as the database, Prisma ORM for database access, and JWT for authentication. All components are Dockerized and orchestrated via Docker Compose for seamless deployment.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Setup and Installation](#setup-and-installation)
  - [Prerequisites](#prerequisites)
  - [Running Locally](#running-locally)
  - [Running with Docker](#running-with-docker)
  - [Environment Variables](#environment-variables)
- [Unit Tests](#unit-tests)
- [Troubleshooting](#troubleshooting)

---

## Overview

This application allows users to log in and manage their tasks (create, read, update, and delete). It provides secure user authentication using JWT tokens and state management via Redux. The backend offers a RESTful API (documented with Swagger) and interacts with a PostgreSQL database using Prisma ORM.

---

## Features

### Frontend

- **User Authentication**: Login and logout functionality using JWT tokens.
- **Task Management**: Create, update, delete, and view tasks.
- **State Management**: Redux is used to manage application state.
- **Built with Next.js**: Provides fast development and server-side rendering capabilities.

### Backend

- **User Authentication**: Endpoints for user registration and login using JWT.
- **Task CRUD Operations**: Endpoints for managing tasks.
- **Database Integration**: PostgreSQL integration via Prisma ORM.
- **Swagger Documentation**: All API endpoints are documented in Swagger.
- **Unit Testing**: Unit tests written using Mocha and Chai.

### Database

- PostgreSQL

---

## Technologies Used

- **Frontend**: Next.js, React, Redux Toolkit, Axios
- **Backend**: Express.js, TypeScript, Prisma, JWT, Swagger
- **Database**: PostgreSQL
- **Testing**: Mocha, Chai
- **Containerization**: Docker, Docker Compose

---

## Project Structure

```
TASK APP
│── backend
│   ├── src
│   │   ├── controllers
│   │   │   ├── todoControllers.ts
│   │   │   ├── userControllers.ts
│   │   ├── middlewares
│   │   │   ├── middlewares.ts
│   │   ├── routes
│   │   │   ├── docs.json
│   │   │   ├── todoRoutes.ts
│   │   │   ├── userRoutes.ts
│   │   ├── services
│   │   │   ├── todoService.ts
│   │   │   ├── userService.ts
│   │   ├── test
│   │   │   ├── controllers
│   │   │   │   ├── todoControllers.test.ts
│   │   │   │   ├── userControllers.test.ts
│   │   ├── types
│   │   │   ├── todoTypes.ts
│   │   ├── utils
│   │   │   ├── swagger.ts
│   │   │   ├── utils.ts
│   │   ├── index.ts
│   ├── .env
│── frontend
│   ├── .next
│   ├── app
│   │   ├── (navbar)/home
│   │   │   ├── DonutCharts.tsx
│   │   │   ├── layout.tsx
│   │   │   ├── page.tsx
│   │   │   ├── Task.tsx
│   │   ├── actions
│   │   │   ├── apis.ts
│   │   ├── cookie.ts
│   │   ├── utils.ts
│   │   ├── assets
│   │   ├── icons.tsx
│   │   ├── components
│   │   │   ├── Navbar.tsx
│   │   ├── signup
│   │   │   ├── page.tsx
│   ├── public
│   │   ├── favicon.ico
│   ├── styles
│   │   ├── globals.css
│   ├── store
│   ├── middleware.ts
│   ├── .env
```
---

## Setup and Installation

### Prerequisites

#### For Local Development:
- **Node.js** 
- **npm** 
- **PostgreSQL**
- **Git** 

#### For Docker Deployment:
- **Docker** 
- **Docker Compose**

#### Development Tools (Optional):
- A code editor (VS Code recommended)
- Postman or similar tool for API testing
- pgAdmin or similar tool for database management

 **Clone the repository**
 
 ```bash
 git clone https://github.com/deepanshupal09/Tasks-App.git
 cd Tasks-App
 ```

### Running Locally


2. **Set up environment variables** (Create `.env` files in `backend` and `frontend` directories as mentioned in the Environment Variables section).

3. **Start the PostgreSQL database** (if not using Docker)
   - Ensure PostgreSQL service is running
   - Create a new database for the application

4. **Run Backend**
   ```bash
   cd backend
   npm install
   npx prisma generate
   npm run build
   npm run start
   ```

5. **Run Frontend**
   ```bash
   cd frontend
   npm install
   npm run build
   npm run start
   ```

6. **Access the Application**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:8000
   - Swagger Documentation: http://localhost:8000/api-docs

### Running with Docker

1. **Ensure Docker is installed and running.**

2. **Build and Start the Containers**
   ```bash
   docker-compose up -d
   ```

3. **Access the Application**
   - **Frontend:** [http://localhost:3000](http://localhost:3000)
   - **Backend API:** [http://localhost:8000](http://localhost:8000)
   - **Swagger Documentation:** [http://localhost:8000/api-docs](http://localhost:8000/api-docs)

### Environment Variables

#### Backend (only required if running locally outside Docker)

Create a `.env` file in the **backend** directory with the following content:
```
DATABASE_URL="postgresql://username:password@localhost:5432/taskdb?schema=public"
JWT_SECRET="your-jwt-secret-key"
PORT=8000
NODE_ENV=development
```

#### Frontend

Create a `.env.local` file in the **frontend** directory with:
```
NEXT_PUBLIC_API_URL=http://localhost:8000
```

---

## Unit Tests

Unit tests for the backend are written using Mocha and Chai. To run the tests, navigate to the **backend** directory and execute:

```bash
cd backend
npm run test
```

This command will run all tests located under the `src/test/` directory.

---

## Troubleshooting

- **Port Conflicts:**\
  If you have another instance of PostgreSQL or another service running on the same port, either stop the conflicting service or update the port mappings in the `docker-compose.yaml`.

- **Database Connection Issues:**\
  Verify that the backend `DATABASE_URL` uses the correct host (`db` when using Docker, `localhost` otherwise).\
  Example: `DATABASE_URL="postgresql://username:password@db:5432/taskdb?schema=public"` for Docker

- **Container Crashes:**\
  Check container logs using:
  ```bash
  docker-compose logs [service-name]
  ```
  Replace `[service-name]` with `frontend`, `backend`, or `db` as needed.

- **Rebuild Issues:**\
  If you make changes to Dockerfiles or environment variables, rebuild the containers:
  ```bash
  docker-compose down
  docker-compose up --build -d
  ```
