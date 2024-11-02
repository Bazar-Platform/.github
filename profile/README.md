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
- **bazar-deployment**: Contains Docker Compose and (if used) orchestration configurations.
- **bazar-protos**: Stores Protobuf files (if used) for defining service interfaces.
- **.github**: Stores project documentation, including design documents.

---

## Lab 1: Bazar.com - A Multi-Tier Online Book Store

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

## Lab 2: Scaling Bazar.com with Replication, Caching, and Containerization

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
    - Use the provided `docker-compose.yml` in the `bazar-deployment` repository for Docker-based deployment.
4. **Accessing the Application**:
    - **Gateway Service**: Access the gateway at `http://<gateway_ip>:8080`.
5. **Testing and Verification**:
    - Use a client script or Postman to test the API endpoints.

---

## License

This project is developed as part of an academic exercise in Distributed Operating Systems. All rights reserved by the
authors.
