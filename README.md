# Media Microservice HMS

[![Java](https://img.shields.io/badge/Java-21-orange)](https://openjdk.org/projects/jdk/21/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.7-brightgreen)](https://spring.io/projects/spring-boot)
[![Spring Cloud](https://img.shields.io/badge/Spring%20Cloud-2025.0.0-blue)](https://spring.io/projects/spring-cloud)
[![MySQL](https://img.shields.io/badge/MySQL-8-blue)](https://www.mysql.com/)
[![Docker](https://img.shields.io/badge/Docker-Ready-blue)](https://www.docker.com/)
[![Eureka](https://img.shields.io/badge/Netflix%20Eureka-Client-lightgrey)](https://cloud.spring.io/spring-cloud-netflix/)

## 🚀 What the Project Does

`media-microservice-hms` is a Spring Boot-based microservice for managing and storing media files within a hospital management system ecosystem. It handles file uploads, storage, and retrieval operations with support for multiple file types. The service integrates seamlessly with Eureka for service discovery and uses MySQL as the persistent storage backend, enabling centralized media management across the HMS platform. Designed for scalability and containerized deployment, it provides a robust foundation for handling patient records, medical documentation, and other hospital media assets.

## 💡 Why the Project is Useful

- Provides a dedicated, decoupled media management API separate from other HMS domains (appointments, profiles, billing, etc.).
- Centralizes file storage and retrieval operations for the entire hospital management system, reducing duplication and improving maintainability.
- Enables standardized media handling with built-in file metadata tracking (name, type, size, upload timestamp).
- Supports fault-tolerant and resilient distributed systems through Eureka service discovery and standardized error payloads.
- Simplifies integration across microservices with a simple RESTful API for upload and download operations.

## ✨ Key Features

- Designed to upload and store media files (`/media/upload`) with automatic metadata extraction.
- Built to retrieve stored media files efficiently (`/media/{id}`) with proper HTTP headers for downloads.
- Implemented with database-backed file storage (MySQL BLOB), ensuring data persistence and reliability.
- Optimized for multi-part file handling with configurable upload size limits (default 10MB).
- Integrated with Spring Cloud Eureka for automatic service registration and discovery.
- Configured with Spring Security scaffolding to support future API authentication and authorization requirements.
- Supports CORS configuration for seamless cross-origin requests from frontend applications.

## 🛠️ Tech Stack

- **Frontend**: (Not included in this module) expects a separate client app.
- **Backend**:
  - Java 21
  - Spring Boot 3.5.7
  - Spring Data JPA
  - Spring Web
  - Spring WebFlux
  - Spring Security
  - Spring Cloud Eureka Client
  - Lombok (for reducing boilerplate)
  - Validation (`spring-boot-starter-validation`)
- **Database**:
  - MySQL (via `mysql-connector-j`)
- **DevOps / Tools**:
  - Maven
  - Docker (Dockerfile present)
  - Spring Boot Maven Plugin

## ⚙️ Getting Started (Installation & Setup)

```bash
git clone https://github.com/vaibhavchaubey/media-microservice-hms.git
cd media-microservice-hms
```

### 1. Configure Database

Set environment variables, or edit `src/main/resources/application.properties`:

- `DB_URL` (e.g., `jdbc:mysql://localhost:3306/mediadb`)
- `DB_USERNAME`
- `DB_PASSWORD`

### 2. Configure Eureka

- `EUREKA_SERVER_URL` default: `http://localhost:8761/eureka/`
- `CORS_ALLOWED_ORIGINS` default: `http://localhost:5173`

### 3. Configure File Upload Limits

- `spring.servlet.multipart.max-file-size` default: `10MB`
- `spring.servlet.multipart.max-request-size` default: `10MB`

### 4. Run Locally

```bash
./mvnw clean package
./mvnw spring-boot:run
```

Service starts on port `9400` by default (set `PORT` env var to override).

### 5. API Endpoints (Key)

#### Media File Operations

- **POST** `/media/upload` - Upload a file
  - **Request**: Multipart form with `file` parameter
  - **Response**: `MediaFileDTO` with file ID, name, type, and size
  - **Status**: 201 Created

- **GET** `/media/{id}` - Download a file by ID
  - **Path Parameter**: `id` (Long) - File ID
  - **Response**: Binary file data with Content-Disposition header
  - **Status**: 200 OK or 404 Not Found

#### Example cURL Commands

**Upload a file:**

```bash
curl -X POST -F "file=@/path/to/document.pdf" http://localhost:9400/media/upload
```

Response:

```json
{
  "id": 1,
  "name": "document.pdf",
  "type": "application/pdf",
  "size": 102400
}
```

**Download a file:**

```bash
curl -X GET http://localhost:9400/media/1 --output document.pdf
```

## 🧪 Tests

```bash
./mvnw test
```

## 🚢 Docker (Optional)

Build image:

```bash
docker build -t media-microservice-hms:latest .
```

Run container:

```bash
docker run \
  -e DB_URL=jdbc:mysql://host.docker.internal:3306/mediadb \
  -e DB_USERNAME=root \
  -e DB_PASSWORD=yourpass \
  -e EUREKA_SERVER_URL=http://host.docker.internal:8761/eureka/ \
  -e CORS_ALLOWED_ORIGINS=http://localhost:5173 \
  -p 9400:9400 \
  media-microservice-hms:latest
```

## 📦 Project Metadata

- **Name**: media-microservice-hms
- **Description**: Media Microservice for Hospital Management System
- **Artifact**: `com.hms:media-microservice-hms:0.0.1-SNAPSHOT`
- **Java Version**: 21
- **Spring Boot Version**: 3.5.7
- **Default Port**: 9400

## 🤝 Contributing

1. Fork the repo
2. Create a feature branch
3. Add tests and run `./mvnw test`
4. Open PR with description and linked issue

## 📝 License

Add your open source / corporate license file at `/LICENSE` (e.g., Apache-2.0, MIT, etc.).
