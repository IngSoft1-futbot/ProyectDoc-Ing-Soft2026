# Non-Functional Requirements

## 1. Security Requirements

### 1.1 User Authentication and Authorization
- The system shall use secure password hashing (e.g., bcrypt).
- Session management must be secure and prevent unauthorized access to user-specific data.
- Role-based access control (RBAC) should be implemented to ensure users only access their own clubs and teams.

### 1.2 Data Security
- Input validation and sanitization must be performed on all API endpoints to prevent common vulnerabilities (e.g., SQL injection, XSS).

### 1.3 Code Execution Security (Sandboxing)
- User-provided Python scripts (Behaviors) must be executed in a strictly isolated, sandboxed environment.
- The sandbox must restrict access to the host filesystem, network, and system-level resources to prevent malicious activity.

## 2. Performance and Scalability Requirements

### 2.1 Latency and Real-time Updates
- Match updates via WebSockets must be delivered with minimal latency to ensure a real-time experience for spectators.
- The system must support multiple concurrent WebSocket connections for real-time monitoring of active matches.

### 2.2 Concurrency
- The backend (FastAPI) must be capable of handling multiple concurrent users and match simulations simultaneously using asynchronous processing.

### 2.3 Scalability
- The system architecture should support horizontal scaling (e.g., through Docker containerization) to accommodate an increasing number of users and matches.

## 3. Reliability and Availability

### 3.1 Data Persistence
- All critical match events, scores, and league standings must be persisted in the database to prevent data loss in case of server failure.
- The system should implement mechanisms to recover state after a crash.

### 3.2 Availability
- The system should aim for high availability, ensuring that users can access their dashboards and matches during the intended operating hours.

## 4. Usability Requirements

### 4.1 Interface Intuition
- The web interface should be user-friendly, allowing users to perform core tasks (e.g., creating a team, assigning a behavior) with minimal steps.

### 4.2 Feedback and Error Handling
- The system must provide clear and informative error messages to the user when an operation fails (e.g., invalid PACSS points, duplicate email).
