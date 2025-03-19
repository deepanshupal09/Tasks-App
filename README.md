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
  - [Environment Variables](#environment-variables)
- [Dockerization](#dockerization)
  - [Building and Running the Containers](#building-and-running-the-containers)
  - [Docker Compose Configuration](#docker-compose-configuration)
- [Unit Tests](#unit-tests)
- [Troubleshooting](#troubleshooting)
- [Additional Notes](#additional-notes)

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

---

## Technologies Used

- **Frontend**: Next.js, React, Redux Toolkit, Axios
- **Backend**: Express.js, TypeScript, Prisma, JWT, Swagger
- **Database**: PostgreSQL
- **Testing**: Mocha, Chai
- **Containerization**: Docker, Docker Compose

---

## Setup and Installation

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) & [Docker Compose](https://docs.docker.com/compose/install/)
- [Node.js](https://nodejs.org/) (if running locally outside Docker)
- [Git](https://git-scm.com/)

### Environment Variables

#### Backend

Create a `.env` file in the **backend** directory with the following content:

```env
SECRET_KEY=very_secret_key
DATABASE_URL=postgresql://postgres:1234@db:5432/tasks?schema=public
```

#### Frontend

Ensure that the frontend environment variables (e.g., in your Next.js configuration or via Docker Compose) are set as follows:

```env
BACKEND_URL=http://localhost:8000
SECRET_KEY=very_secret_key
```

*Note: The **`BACKEND_URL`** should point to the Docker Compose service name (**`backend`**), not **`localhost`**.*

---

## Dockerization

### Building and Running the Containers

1. **Build and Start the Containers**

   From the root directory (where `docker-compose.yaml` is located), run:

   ```bash
   docker-compose up --build
   ```

   This command builds the Docker images for the frontend, backend, and PostgreSQL database, then starts all the containers.

2. **Access the Application**

   - **Frontend:** [http://localhost:3000](http://localhost:3000)
   - **Backend API:** [http://localhost:8000](http://localhost:8000)
   - **Swagger Documentation:** [http://localhost:8000/api-docs](http://localhost:8000/api-docs)

### Docker Compose Configuration

Below is `docker-compose.yaml` file:

```yaml
version: "3.8"
services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    environment:
      - BACKEND_URL=http://backend:8000
      - SECRET_KEY=very_secret_key
    depends_on:
      - backend

  backend:
    build: ./backend
    ports:
      - "8000:8000"
    environment:
      - SECRET_KEY=very_secret_key
      - DATABASE_URL=postgresql://postgres:1234@db:5432/tasks?schema=public
    depends_on:
      - db

  db:
    image: postgres:13
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: "1234"
      POSTGRES_DB: tasks
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

---

## Unit Tests

Unit tests for the backend are written using Mocha and Chai. To run the tests, navigate to the **backend** directory and execute:

```bash
npm run test
```

This command will run all tests located under the `src/test/` directory.

---

## Troubleshooting

- **Port Conflicts:**\
  If you have another instance of PostgreSQL or another service running on the same port, either stop the conflicting service or update the port mappings in the `docker-compose.yaml`.

- **Database Connection Issues:**\
  Verify that the backend `DATABASE_URL` uses the Docker Compose service name (`db`) instead of `localhost`.\
  Example:\
  `DATABASE_URL=postgresql://postgres:1234@db:5432/tasks?schema=public`

- **Container Crashes:**\
  Check container logs using:

  ```bash
  docker-compose logs [service-name]
  ```

  Replace `[service-name]` with `frontend`, `backend`, or `db` as needed.

- **Rebuild Issues:**\
  If you make changes to Dockerfiles or environment variables, rebuild the containers:

  ```bash
  docker-compose up --build
  ```

---

## Additional Notes

- **Swagger Documentation:**\
  The API documentation is available via Swagger at:

  - **Swagger UI:** [http://localhost:8000/api-docs](http://localhost:8000/api-docs)

- **Authentication Tokens:**\
  The current implementation uses JWT for authentication. (Refresh token mechanism is not implemented.)

---

