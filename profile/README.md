# Bazar Platform

- Refer to [**Lab1.pdf**](https://github.com/Bazar-Platform/.github/blob/main/documents/Lab1.pdf) and
  [**Lab2.pdf**](https://github.com/Bazar-Platform/.github/blob/main/documents/Lab2.pdf) for complete project
  requirements.
- Specific documentation for each service is available in their respective repositories.

---

## Overview

This project series focuses on building and enhancing **Bazar.com**, a multi-tier online bookstore system. Initially,
Bazar.com starts as a small, minimalistic online bookstore with a catalog of only four books. The project progressively
adds complexity and functionality, such as replication, caching, and containerization, to handle higher demand and
improve performance. The goal is to deepen your understanding of distributed systems, web microservices, and performance
optimization techniques.

### Repository Structure

- **bazar-gateway-service**: Handles user interactions and routes requests to backend services.
- **bazar-catalog-service**: Manages the book catalog, including stock levels and book details.
- **bazar-order-service**: Manages order requests, verifies stock availability, and processes purchases.
- **.github**: Stores project documentation, including design documents.

Each service includes its own `docker-compose.yml` file for development. The **gateway service** acts as the main entry
point, coordinating all services.

---

## Architecture

### High-Level Design

The Bazar system uses a microservices-based architecture, with each service responsible for a specific domain of the
application. These services communicate with each other using REST. The system includes caching and
replication to improve performance and ensure fault tolerance.

![System Architecture](https://github.com/Bazar-Platform/.github/blob/main/assets/SystemDesign.png)

[Excalidraw Link](https://excalidraw.com/#json=-hVk5zOBo8IaVsKXkbG2l,NRkmCo3XwbNGVUZ0aoi-DA)

#### Key Components

1. **Gateway Service**:
    - Acts as the entry point for all client traffic.
    - Handles routing, caching, and load balancing for the backend services. It uses **Round-Robin** load balancing
      algorithm to distribute load evenly among nodes.
    - Ensures high availability and improved performance through in-memory caching and intelligent request distribution.
    - An in-memory cache in the Gateway Service stores frequently accessed data (e.g., book details). It uses **LRU**
      (least recently used) method to eliminate data when cache size limit is hit.

2. **Catalog Service**:
    - Maintains the book catalog, including details such as stock, price, and topic.
    - Implements a **primary-backup replication** for fault tolerance and performance:
        - The primary instance handles all write operations, and syncs updates with the backup.
    - Integrates with the gateway for cache invalidation upon updates.

3. **Order Service**:
    - Processes purchase requests, verifies stock availability, and updates catalog data.
    - Uses multiple replicas to handle increased loads.
    - Works with the gateway for balanced request distribution.

#### Communication Flow

- **Search and Query**:
    - The gateway queries the primary Catalog Service for book details.
    - Frequently accessed queries are cached to reduce load on the backend.

- **Purchases**:
    - Purchase requests are routed through the gateway to the Order Service.
    - The Order Service communicates with the Catalog Service to validate and update stock levels.

- **Cache Invalidation**:
    - When the Catalog Service processes an update, it notifies the Gateway Service to invalidate relevant cache
      entries.

---

## Lab 1: Bazar - A Multi-Tier Online Book Store

### Objectives

- **Create a Simple Online Book Store**: Design and implement Bazar.com with a small catalog of four books.
- **Multi-Tier Architecture**: Set up a two-tier architecture with a front-end gateway and back-end services (catalog
  and order).
- **Microservices Design**: Implement each component as an independent microservice.
- **REST API**: Expose all operations as HTTP REST interfaces.

### Components

1. **Gateway Service**: The front-end service that:
    - Accepts user requests and routes them to the appropriate backend service.
    - Supports operations:
        - `search(topic)`: Finds books by topic.
        - `info(item_number)`: Provides details about a specific book.
        - `purchase(item_number)`: Initiates the purchase of a specific book.

2. **Catalog Service**: Maintains the catalog with details for each book (stock, cost, topic).
    - Supports operations:
        - `query`: Look up books by topic or item number.
        - `update`: Update stock or pricing information.

3. **Order Service**: Manages purchase requests and stock verification.
    - `purchase(item_number)`: Checks stock availability and decrements stock upon purchase.

---

## Lab 2: Scaling Bazar with Replication, Caching, and Containerization

### Objectives

- **Enhance System Performance**: Introduce replication, caching, and containerization to improve Bazar.com’s
  scalability and reduce latency.
- **Replication and Load Balancing**: Add replicas for catalog and order services, and implement load balancing in the
  gateway.
- **Caching**: Add an in-memory cache at the gateway to serve recent read requests efficiently.
- **Containerization** (Optional): Package each component as a Docker container for ease of deployment.

### Components and Enhancements

1. **Gateway Service** (Enhanced):
    - Introduce **in-memory caching** for recent queries to reduce load on backend services.
    - Implement **load balancing** to distribute requests among replicated catalog and order servers (e.g., using
      round-robin or least-loaded strategies).

2. **Catalog and Order Services** (Replicated):
    - Replicate both services across multiple instances to improve request handling capacity.
    - Ensure consistency across replicas through internal synchronization protocols.

3. **Cache Consistency**:
    - When a backend database entry is updated, backend servers invalidate relevant cached entries.
    - Implement a cache replacement policy (e.g., LRU) if a cache size limit is set.

4. **Optional Dockerization**:
    - Containerize each component (gateway, catalog, order, and cache if separate).
    - Upload Docker images to the GitHub repository, allowing easy deployment on any compatible environment.

---

## REST API Endpoints

### Gateway Service (Front-End)

- **GET** `/search/<topic>`: Search for books by topic.
- **GET** `/info/<item_number>`: Get information about a specific book.
- **POST** `/purchase/<item_number>`: Purchase a specific book.

### Catalog Service (Back-End)

- **GET** `/query?topic=<topic>`: Query books by topic.
- **GET** `/query?item_number=<item_number>`: Query book details by item number.
- **PUT** `/update/<item_number>`: Update stock or price information.

### Order Service (Back-End)

- **POST** `/purchase/<item_number>`: Handle purchase requests, verify stock, and update quantities.

---

## Development and Deployment Instructions

### Running the Project

1. **Clone Repositories**:
    - Clone the necessary repositories for each service (gateway, catalog, order).
2. **Configuration**:
    - Set up environment variables and configurations as specified in each service’s `README.md`.
3. **Start Services**:
    - Use the provided `docker-compose.yml` in the `bazar-gateway-service` repository to start all services together.
4. **Accessing the Application**:
    - **Gateway Service**: Access the gateway at `http://localhost:5000`.
5. **Testing and Verification**:
    - Use the `tests.http` file or a tool like Postman to test the API endpoints.

Each service can also be run individually using its own `docker-compose.yml` file for isolated development and testing.

---

## License

This project is developed as part of an academic exercise in Distributed Operating Systems. All rights reserved by the
authors.
