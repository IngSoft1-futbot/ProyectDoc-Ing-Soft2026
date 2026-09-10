## Futbot - Football Game Management System

### 1. Introduction and Overview

#### 1.1 Purpose
Futbot is a multiplayer web game, where the users assume the technical director role and compete in matches with teams made of 3 main players and 3 replacement players. The user can assign behaviors for the selected players for the match duration, ... (WIP)

#### 1.2 Project Scope
Futbot is a software application designed as an educational project for a Software Engineering course. It serves as a real-world example of a professional working environment and provides practical experience in software development methodologies.

The system will provide:
- User registration and profile management
- Player creation with customizable statistics (PACSS)
- Team organization and management
- League creation and administration
- Behavior scripting for AI-driven player actions
- Match simulation and leaderboard tracking
- Friendly match invitations between users
- Spectator mode for live matches
- Global ranking system
- Avatar customization for users

#### 1.3 System Environment

**Primary Actor:**
- **User (Club Owner):** Creates and manages leagues, owns and manages multiple teams, creates and configures players

**Secondary Actors:**
- **System:** Stores data, validates inputs, executes match simulations, manages behaviors

#### 1.4 Key Entities

**User:**
- A person interacting with the system
- Can create multiple teams, players, leagues and behaviors
- Manages player profiles, teams and behaviors.
- Owns clubs and teams

**Player:**
- Members of a team
- Have configurable PACSS statistics
- Can be assigned to teams
- Execute behaviors during matches
- Require at least 6 players per team for league participation

**Team:**
- Consists of multiple players (minimum 6 players required for league participation)
- Composed of 3 Main players (Center, Upper Defendant, Lower Defendant) and 3 Replacement players for league matches
- Owned by a user
- Can participate in leagues and friendly matches
- Cannot participate in multiple leagues simultaneously

**League:**
- Collection of teams competing together
- Has a local leaderboard tracking team statistics
- Contains scheduled and completed matches
- Can be open (public) or private (password-protected)
- Minimum 3 teams required to create a league
- All teams within a league play matches against each other

**Behavior:**
- Python script defining how a player acts during a match
- Created and managed by users
- Assigned to players for match execution
- Can be modified or deleted (with restrictions on active assignments)

**PACSS:**
- Player attribute system with 5 statistics:
  - **Power:** Amount of force a player can hit the ball with
  - **Speed:** Movement speed of the player
  - **Agility:** Time required for the player to kick the ball again (recovery time)
  - **Control:** Maximum distance from which the player can kick the ball
  - **Strength:** Probability of winning a duel against another player
- Players allocate exactly 300 PACSS points during creation
- Minimum of 20 points required per attribute

---

### 2. System Architecture Overview

The system is organized into four core functional areas based on DFD Level 1 processes:

| Process | Responsibility | Data Store |
|---------|-----------------|------------|
| **1.0 User/Profile Management** | User registration, login, profile management | D1: User DB |
| **2.0 Team Management** | Team creation, player assignment, team deletion | D2: Teams DB |
| **3.0 League and Matches Engine** | League creation, match scheduling, leaderboard management | D3: Matches DB |
| **4.0 Bots and Behaviors Engine** | Behavior creation, modification, deletion, Python syntax validation | D4: Bots DB |

#### 2.1 Technology Stack
- **Backend Framework:** FastAPI with Python 3.x
- **Database:** PostgreSQL/SQLite (SQLAlchemy ORM)
- **Frontend:** React.js with TypeScript
- **Real-time Communication:** WebSockets via FastAPI's WebSocket support
- **Security:** Password hashing, input validation, sandboxed Python execution

#### 2.2 System Components
1. **Authentication Module:** User registration, login, session management
2. **Player Management Module:** Player creation with PACSS attributes
3. **Team Management Module:** Team creation and player assignment
4. **League Management Module:** League creation, match scheduling, standings
5. **Behavior Engine Module:** Python script validation and execution
6. **Match Simulation Module:** Real-time game simulation and WebSocket updates

---

### 3. Data Flow Overview

**External Entity:**
- User (Client)

**System Inputs:**
- User Credentials (Emails, Passwords, Usernames)
- Team Data (Team names, PACSS stats, Players, Team-specific stats)
- League Data (League names, Leaderboards, Fixtures, Matches)
- Bot Data (Behaviors, Python Scripts, Python Syntax Verification)

**System Outputs:**
- Game State Updates
- UI Updates
- Validation Messages
- Confirmation/Error Messages

---

### 4. Definitions and Terminology

**Main Player:** One of three essential players required for team participation in matches (Center, Upper Defendant, Lower Defendant)

**Replacement Player:** Substitute players (3 required) available to enter matches.

**PACSS:** Player Attribute Control System. The five statistics (Power, Agility, Control, Speed, Strength) used to define player capabilities.

**Leaderboard:** Ranking system within a league tracking team performance metrics (Wins, Losses, Total Goals, points, Owner name). Point values are determined as described, 3 points for a victory, 1 point for a draw, and 0 points for a loss.

**Match Simulation:** Real-time execution of football matches with AI-driven player actions.

**Friendly Match:** Unofficial matches between teams that are not part of a league structure

**Behavior:** Python script that defines how a player acts during match simulation.

**Behavior Assignment:** The act of linking a specific behavior script to a player who will execute that behavior during matches

**Python Syntax Validation:** System verification that ensures behavior scripts contain valid Python code before saving 

**Sandbox:** Secure execution environment for user-provided Python scripts.

**WebSocket:** Real-time communication protocol for live updates during matches.

---

### 5. Constraints and Assumptions

**Constraints:**
- Each team must have exactly 3 Main players and 3 Replacement players for league participation
- Match duration depends of the leagues rules, divided by a half-time break and 2 cooling breaks.
- Minimum match start time must be greater than 1 minute from current time
- Teams cannot participate in multiple leagues simultaneously
- Players cannot be assigned to multiple teams simultaneously
- Behaviors cannot be modified if actively assigned to players (must be unassigned first)
- PACSS point allocation is fixed at 300 points total per player

**Assumptions:**
- Users will have stable internet connectivity
- Web browser with WebSocket support for real-time updates
- Python syntax validation will use Python 3.x standards
- All user credentials will be validated and securely stored
- League leaderboards will update in real-time as matches complete
- System will maintain data consistency across all databases
