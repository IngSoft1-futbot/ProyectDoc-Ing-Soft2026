# System Constraints and Assumptions

## 1. System Constraints

### 1.1 Technical Constraints
- **Backend Framework:** The backend must be implemented using **FastAPI** (Python-based).
- **Real-time Communication:** Real-time updates for live matches must be implemented using **WebSockets** (no polling allowed).
- **Frontend Framework:** The client-side user interface must be developed using **React.js** with **TypeScript**.
- **Database ORM:** Interaction with the database must be handled via **SQLAlchemy**.
- **Deployment:** The application is intended for deployment using **Docker** containerization.

### 1.2 Security Constraints
- **Code Execution:** Any user-provided Python code (Behaviors) must be executed in a secure, isolated sandbox environment to prevent unauthorized access to the host system.

### 1.3 Operational Constraints
- **Match Simulation:** Matches consist of 4 quarters.
- **Substitutions:** Substitutions can only be requested during breaks (between quarters or during cooling breaks) and must be completed before the next quarter starts, substitutions are not accumulative, meaning only one substitution per break and if a user does not use that substitutions, then they must wait until next quarter to perform one.
- **Resource Usage:** Match simulations and real-time updates must be optimized to prevent excessive server resource consumption.

### 1.4 Business Rules
- **Team Composition & League Eligibility:** A team must have a minimum of 6 players to be eligible for league participation, 3 main players and 3 replacement players. While a team may fall below 6 players due to player deletion, they are prohibited from joining or participating in any league until the roster is restored to at least 6 players.

## 2. Assumptions

### 2.1 Connectivity
- It is assumed that clients have a stable and continuous internet connection, especially during active match simulations, to maintain WebSocket connectivity.

### 2.2 Security Environment
- It is assumed that the provided Python sandbox is robust enough to mitigate all forms of malicious code execution and resource exhaustion.

### 2.3 Client Environment
- It is assumed that users are accessing the system via modern, standard-compliant web browsers that fully support WebSockets and HTML5.

### 2.4 Data Availability
- It is assumed that the server maintains high uptime to prevent disruption during active leagues and matches.
