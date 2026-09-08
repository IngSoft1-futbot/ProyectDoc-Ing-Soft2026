# Functional Requirements

## 1.0 User/Profile Management

### 1.1 Use Case: Sign Up (Create New User)

**Actor:** Guest

**Brief Description:**
A new user creates an account to access the system and begin playing the game.

**Preconditions:**
- The user is not currently logged in

**Inputs:**
1. Username
2. Email
3. Password
4. Name
5. Avatar

**Successful Flow:**
1. User selects the Sign Up option
2. System displays the Sign-up form with fields for Username, Email, Password, Name, and Avatar
3. User enters required details and clicks Submit
4. System validates input formats and verifies email uniqueness
5. System creates the new user account in the database
6. System displays a confirmation message and provides a link/button to the Login Screen

**Exceptional Scenarios:**
- **Exception 1 - Email Already in Use:**
  - Trigger: Email validation fails (Step 4)
  - Flow: System displays error indicating email is already in use and prompts user to log in or use a different email

- **Exception 2 - Missing Input Fields:**
  - Trigger: Input validation fails (Step 4)
  - Flow: System displays error indicating missing fields and prompts user to fill all required fields

**Post-conditions:**
1. A new account is active in the system database
2. The user is ready to log in

---

### 1.2 Use Case: User Login

**Actor:** Registered User

**Brief Description:**
A registered user logs into the system to access their dashboard and manage their clubs, teams, and leagues.

**Preconditions:**
- User account exists in the database
- User is not currently logged in

**Inputs:**
1. Email
2. Password

**Successful Flow:**
1. User enters email and password on the login form
2. System validates field formats
3. System authenticates credentials against the database
4. System generates a user session
5. System displays the main menu/dashboard

**Exceptional Scenarios:**
- **Exception 1 - Field Validation Errors:**
  - Trigger: Invalid email format or empty fields (Step 2)
  - Flow: System displays field validation errors and prompts user to correct them

- **Exception 2 - Invalid Credentials:**
  - Trigger: Email not found or password mismatch (Step 3)
  - Flow: System displays "Invalid Email or Password" error

**Post-conditions:**
1. User session is created
2. User is authenticated and can access the system
3. Last login timestamp is updated in the database

---

### 1.3 Use Case: Manage User Profile

**Actor:** User (Logged In)

**Brief Description:**
A user views and updates their profile information, including personal details and optional password changes.

**Preconditions:**
- User is logged in

**Inputs:**
1. Name (Optional)
2. Email (Optional)
3. Avatar (Optional)
4. Current Password (Required for password change)
5. New Password (Optional)
6. Repeat Password (Optional)

**Successful Flow:**
1. User clicks on their avatar icon in the interface.
2. System redirects the user to the "Manage Profile" page.
3. System pre-populates the form with the user's current Name, Email, and Avatar.
4. System leaves the "Current Password", "New Password", and "Repeat Password" fields blank.
5. User modifies any profile information or enters new password details.
6. User clicks the "Save changes" button.
7. System validates the input (checking email format, avatar upload, and password matching/verification).
8. System updates the user's data in the database.
9. System displays a confirmation message to the user.

**Exceptional Scenarios:**
- **Exception 1 - Password Mismatch:**
  - Trigger: "New Password" and "Repeat Password" do not match (Step 7)
  - Flow: System displays an error indicating passwords do not match.

- **Exception 2 - Incorrect Current Password:**
  - Trigger: "Current Password" does not match the existing password when a change is attempted (Step 7)
  - Flow: System displays an error indicating the current password is incorrect.

- **Exception 3 - Invalid Data Format:**
  - Trigger: Invalid email format or invalid avatar file (Step 7)
  - Flow: System displays a validation error for the specific field.

**Post-conditions:**
1. User's profile information is updated in the database (if "Save changes" was clicked).
2. The user's profile view reflects the new information.

---

### 1.4 Use Case: Log Out

**Actor:** User (Logged In)

**Brief Description:**
The user terminates their current session and logs out of the system, ending authentication and releasing any active resources.

**Inputs:**
1. "Log Out" button click or logout action from any page
2. Confirmation (optional, if auto-logout warning is shown)

**Preconditions:**
- User is currently logged in with an active session
- User may be on any page of the application

**Successful Flow:**
1. User accesses any page in the system (dashboard, league page, profile page, etc.).
2. User clicks the "Log Out" button (typically in header/profile menu) or performs logout action.
3. System validates that user has an active session (Step 2).
4. System displays confirmation message: "Are you sure you want to log out?" with "Confirm" and "Cancel" buttons.
5. If user confirms:
   - System invalidates current session token/cookie
   - System clears sensitive data from client-side memory (local storage, session variables)
   - System terminates active WebSocket connection for real-time match updates
   - System removes user-specific resources (open league pages, team views)
6. System displays confirmation message: "You have successfully logged out."
7. System redirects user to the login page.

**Exceptional Scenarios:**
- **Exception 1 - Cancel Logout Confirmation:**
  - Trigger: User clicks "Cancel" in confirmation dialog (Step 4)
  - Flow: User remains logged in with no changes made to session or data.

**Post-conditions:**
1. User session is terminated and invalidated on server side.
2. Active WebSocket connection closed.
3. Client-side sensitive data cleared.
4. User redirected to login/welcome page.
5. User cannot access protected resources until re-authentication.

---

## 2.0 Player Management

### 2.1 Use Case: Create Player

**Actor:** User (Logged In)

**Brief Description:**
A user creates a new player with customizable PACSS attributes.

**Preconditions:**
- User is logged in
- User has access to player creation interface

**Inputs:**
1. Player name
2. PACSS attributes (Power, Speed, Agility, Control, Strength)
3. Team assignment (optional at creation)

**Successful Flow:**
1. User accesses player creation interface
2. System displays form for player creation
3. User enters player name and PACSS values
4. System validates that exactly 300 points are allocated across all attributes
5. System validates minimum value of 20 points per attribute
6. System creates player record in database
7. System assigns player to team if specified
8. System displays confirmation message

**Exceptional Scenarios:**
- **Exception 1 - Invalid PACSS Allocation:**
  - Trigger: Total points not equal to 300 (Step 4)
  - Flow: System displays error indicating incorrect point allocation

- **Exception 2 - Attribute Below Minimum:**
  - Trigger: Any attribute below 20 point (Step 5)
  - Flow: System displays error indicating minimum point requirement

**Post-conditions:**
1. New player record is created in database
2. Player is available for team assignment
3. Player can be assigned to a team or used in behaviors

---

### 2.2 Use Case: Inspect Player

**Actor:** User (Logged In)

**Brief Description:**
A user inspect player information including PACSS attributes.

**Preconditions:**
- User is logged in
- Player exists in the database

**Inputs:**
- None.

**Successful Flow:**
1. User accesses player management interface
2. System displays current player information

**Post-conditions:**
1. User can see 

---

### 2.3 Use Case: Delete Player

**Actor:** User (Logged In)

**Brief Description:**
A user removes a player from the system.

**Preconditions:**
- User is logged in
- Player exists in the database

**Inputs:**
1. Player to delete
2. Confirmation (if applicable)

**Successful Flow:**
1. User accesses the Player Management interface.
2. System displays a list of the user's players.
3. User selects a player and clicks the "Delete" button.
4. System checks if the player is currently in a team or a league.
5. If the player is in a team:
    a. System displays a warning: "Player is currently part of team {team name}".
    b. User clicks the "Proceed" button.
    c. System deletes the player record.
    d. System updates the roster of the associated team in the database.
    e. System displays a confirmation message.
6. If the player is not in a team and not in a league:
    a. System deletes the player record.
    b. System displays a confirmation message.

**Exceptional Scenarios:**
- **Exception 1 - Player in an Active League:**
  - Trigger: Player is currently participating in an active league (Step 4)
  - Flow: System displays an error: "Player currently in a league, wait until the league finishes" and no changes are made.

- **Exception 2 - Cancellation of Team Warning:**
  - Trigger: User clicks "Cancel" on the team warning (Step 5b)
  - Flow: System closes the warning and no changes are made.

**Post-conditions:**
1. The selected player is removed from the database.
2. Associated team rosters are updated if the player was part of a team.

---

## 3.0 Team Management

### 3.1 Use Case: Create Team

**Actor:** User (Logged In)

**Brief Description:**
A user creates a new team for organizing players and participating in leagues.

**Preconditions:**
- User is logged in
- User has access to team creation interface

**Inputs:**
1. Team name
2. Player assignments (minimum 6 players required)
3. Team captain selection

**Successful Flow:**
1. User accesses team creation interface
2. System displays form for team creation
3. User enters team name and selects players
4. System validates minimum of 6 players requirement
5. System creates team record in database
6. System assigns players to the team
7. System displays confirmation message

**Exceptional Scenarios:**
- **Exception 1 - Insufficient Players:**
  - Trigger: Less than 6 players selected (Step 4)
  - Flow: System displays error indicating minimum player requirement

- **Exception 2 - Player Already Assigned:**
  - Trigger: Attempt to assign player already in another team (Step 4)
  - Flow: System displays error indicating player is already assigned

**Post-conditions:**
1. New team record is created in database
2. Team is available for league participation
3. Players are assigned to the team

---

### 3.2 Use Case: Manage Team Players

**Actor:** User (Logged In)

**Brief Description:**
A user adds or removes players from an existing team.

**Preconditions:**
- User is logged in
- Team exists in database
- User owns the team

**Inputs:**
1. Player to add/remove
2. Action type (add or remove)

**Successful Flow:**
1. User accesses team management interface
2. System displays current team players
3. User selects player and action
4. System validates action (add/remove)
5. System updates team-player relationships
6. System confirms changes

**Exceptional Scenarios:**
- **Exception 1 - Adding Player Already in Team:**
  - Trigger: Attempt to add player already assigned to this team (Step 4)
  - Flow: System displays error indicating player is already on team

- **Exception 2 - Removing Player from Team:**
  - Trigger: Attempt to remove player not assigned to team (Step 4)
  - Flow: System displays error indicating player is not on team

**Post-conditions:**
1. Team-player relationships are updated in database
2. Team composition is reflected in system

---

### 3.3 Use Case: Delete Team

**Actor:** User (Logged In)

**Brief Description:**
A user removes a team from the system.

**Preconditions:**
- User is logged in
- User owns the team
- Team exists in the database

**Inputs:**
1. Team to delete
2. Confirmation (implicit via button click)

**Successful Flow:**
1. User accesses the Teams interface.
2. System displays a list of the user's teams.
3. Each team is shown with two buttons: "Manage Team Players" and "Delete Team".
4. User clicks the "Delete Team" button for the desired team.
5. System validates that the team is not currently joined to a league or playing a friendly match.
6. If valid, system deletes the team record.
7. System displays a confirmation message to the user.

**Exceptional Scenarios:**
- **Exception 1 - Team Joined to League:**
  - Trigger: Team has been joined to a league (Step 5)
  - Flow: System displays an error: "Cannot delete team: it is currently part of league {league name}" and returns to the Teams interface.

- **Exception 2 - Team Playing Friendly Match:**
  - Trigger: Team is currently scheduled to play a friendly match (Step 5)
  - Flow: System displays an error: "Cannot delete team: it is currently playing a friendly match" and returns to the Teams interface.

**Post-conditions:**
1. The selected team is removed from the database.
2. Associated teams' rosters are not affected (players remain in system).

---

## 4.0 League Management

### 4.1 Use Case: Create League

**Actor:** User (Logged In)

**Brief Description:**
A user creates a new league with specific parameters for team competition.

**Preconditions:**
- User is logged in
- User has access to league creation interface
- Minimum 3 teams available for league creation

**Inputs:**
1. League name
2. League type (public/private)
3. Password (for private leagues)
4. Duration parameters
5. Team limits
6. Competition rules

**Successful Flow:**
1. User accesses league creation interface
2. System displays form for league creation
3. User enters league details
4. System validates minimum team requirement
5. System creates league record in database
6. System assigns teams to league
7. System displays confirmation message

**Exceptional Scenarios:**
- **Exception 1 - Insufficient Teams:**
  - Trigger: Less than 3 teams provided (Step 4)
  - Flow: System displays error indicating minimum team requirement

- **Exception 2 - Invalid League Name:**
  - Trigger: Empty or duplicate league name (Step 3)
  - Flow: System displays error indicating invalid name

**Post-conditions:**
1. New league record is created in database
2. League is ready for match scheduling
3. Teams are assigned to the league

---

### 4.2 Use Case: Schedule Matches

**Actor:** User (Logged In)

**Brief Description:**
A user schedules matches between teams within a league.

**Preconditions:**
- User is logged in
- League exists in database
- Teams are assigned to league
- Match scheduling interface is accessible

**Inputs:**
1. Match date and time
2. Home team
3. Away team
4. Match duration
5. Location (optional)

**Successful Flow:**
1. User accesses match scheduling interface
2. System displays available teams in league
3. User selects teams for match
4. System validates match parameters
5. System schedules match in database
6. System displays confirmation message

**Exceptional Scenarios:**
- **Exception 1 - Teams Not in Same League:**
  - Trigger: Selected teams not both in the same league (Step 3)
  - Flow: System displays error indicating teams must be in same league

- **Exception 2 - Overlapping Schedule:**
  - Trigger: Scheduled match conflicts with existing match (Step 4)
  - Flow: System displays error indicating schedule conflict

**Post-conditions:**
1. Match is scheduled in database
2. Match appears in league calendar
3. Teams are notified of upcoming match

---

### 4.3 Use Case: Join League via GUI

**Actor:** User (Logged In)

**Brief Description:**
The user joins a public league through the main menu by selecting available teams and assigning behaviors to players before final confirmation.

**Inputs:**
1. League name (from main menu)
2. Team selection from available teams
3. Behavior assignments for each player
4. Final "Join" button click

**Preconditions:**
- User is logged in
- At least one public league exists and has available slots
- User owns at least one available team

**Successful Flow:**
1. On the main menu, system displays a list of **strictly public leagues**.
2. Each league shows a "Join" button next to it.
3. User clicks the "Join" button for a desired league.
4. System displays a small window (modal).
5. In the modal, system displays available teams that can join the league.
6. System **grays out** unavailable teams:
   - Teams already participating in other leagues
   - Teams with fewer than 6 players
7. User selects an available team to join the league.
8. System validates:
	- Selected team is available (not in another league, has ≥6 players) 
9. After selecting a team, system displays the players of that team in a list.
10. Next to each player, a drop-down menu lists all available behaviors (including `default_behavior` as an option).
11. The user assigns desired behaviors to players (behaviors may be assigned to multiple players; the same behavior can be selected for multiple players).
12. User clicks the final "Join" button.
13. System validates that the league has not reached maximum participant teams limit
14. System registers team in league database with assigned behaviors.
15. System redirects user to the league page/dashboard.

**Exceptional Scenarios:**
- **Exception 1 - League at Capacity:**
  - Trigger: The league has reached its maximum number of participating teams (Step 12)
  - Flow: System displays an error: "Cannot join league: it has reached maximum participant limit" and returns the user to the main menu.

**Post-conditions:**
1. Team is registered in the public league.
2. Player behavior assignments are recorded in the database.
3. User is redirected to the league page/dashboard.
4. Team becomes unavailable for joining other leagues until it leaves this one or is deleted.

---

### 4.4 Use Case: Join Private League

**Actor:** User (Logged In)

**Brief Description:**
The user joins a private league through the main menu by entering the league name and password, then selecting their team and assigning behaviors before joining.

**Inputs:**
1. League name (user enters to locate private league)
2. Password (user enters for authentication)
3. Team selection from available teams
4. Behavior assignments for each player
5. Final "Join" button click

**Preconditions:**
- User is logged in
- At least one private league exists
- League has not reached maximum participant limit
- User owns at least one available team

**Successful Flow:**
1. On the main menu, user sees a "Join Private League" button.
2. User clicks the "Join Private League" button.
3. System displays a modal prompting for:
   - **League Name** (to locate the private league)
   - **Password** (for authentication)
4. User enters the correct league name and password.
5. System validates:
   - League name matches an existing private league
   - Password matches the league's password
6. If validation succeeds, system displays a small window (modal) for team selection.
7. In the modal, system displays available teams that can join the league.
8. System **grays out** unavailable teams:
   - Teams already participating in other leagues
   - Teams with fewer than 6 players
10. User selects an available team to join the league.
11. After selecting a team, system displays the players of that team in a list.
12. Next to each player, a drop-down menu lists all available behaviors (including `default_behavior` as an option).
13. The user assigns desired behaviors to players (behaviors may be assigned to multiple players; same behavior can be selected for multiple players).
14. User clicks the final "Join" button.
15. System validates that the league has not reached maximum participant teams limit.
16. System registers team in private league database with assigned behaviors.
17. System redirects user to the league page/dashboard.

**Exceptional Scenarios:**
- **Exception 1 - Invalid League Name:**
  - Trigger: User enters incorrect league name (Step 5)
  - Flow: System displays error: "League not found" and returns user to main menu.

- **Exception 2 - Invalid Password:**
  - Trigger: User enters incorrect password (Step 5)
  - Flow: System displays error: "Invalid password for this league" and returns user to main menu.

- **Exception 3 - League at Capacity:**
  - Trigger: The league has reached its maximum number of participating teams (Step 15)
  - Flow: System displays an error: "Cannot join league: it has reached maximum participant limit" and returns the user to the main menu.

**Post-conditions:**
1. Team is registered in the private league.
2. Player behavior assignments are recorded in the database.
3. User is redirected to the league page/dashboard.
4. Team becomes unavailable for joining other leagues until it leaves this one or is deleted.

---

### 4.5 Use Case: Leave League

**Actor:** User (Logged In)

**Brief Description:**
The user leaves a league they have previously joined, releasing their team to be available for another league.

**Inputs:**
1. "Leave League" button click on the league menu/dashboard
2. Confirmation (via confirmation dialog)

**Preconditions:**
- User is logged in
- Team is currently assigned to a league
- Match simulation has not started or league has not begun matches

**Successful Flow:**
1. On the league page/dashboard, user sees their team's status within the league.
2. User clicks "Leave League" button for their team.
3. System displays confirmation dialog: "Are you sure you want to leave this league? Your team will be available to join another league immediately." with "Confirm" and "Cancel" buttons.
4. User clicks "Confirm" to proceed.
5. System validates:
   - League has not started (no active matches or quarter play has begun)
6. If validation passes:
   - System removes team from league database
   - System preserves team's current score and stats (unaffected by leaving)
   - System recalculates league fixtures and matchups:
     - Removes all remaining scheduled matches involving the departing team
     - Adjusts league standings if necessary
1. System broadcasts real-time update via WebSocket.
2. System redirects user to their main dashboard.
3. Team is immediately available to join another league.
4. System displays confirmation message: "Your team has left the league and is now available for other leagues."

**Exceptional Scenarios:**
- **Exception 1 - League Already Started:**
  - Trigger: Attempt to leave when league has started (active matches or quarter play in progress) (Step 5)
  - Flow: System displays error: "Cannot leave league: league matches have already started" and no changes are made.

- **Exception 2 - Cancel Confirmation:**
  - Trigger: User clicks "Cancel" in confirmation dialog (Step 4)
  - Flow: Operation cancelled; user remains in the league with no changes made.

**Post-conditions:**
1. Team removed from league database.
2. Team's score and stats preserved.
3. League fixtures/matchups recalculated to exclude departing team.
4. Team is immediately available for joining another league.
5. WebSocket notification sent to all league participants.

---

### 4.6 Use Case: List User Leagues

**Actor:** Registered User (Logged In)

**Brief Description:**
Allows a user to consult the list of all leagues they have created or in which they are currently participating with their team.

**Preconditions:**
- The User must be logged in to the platform.

**Inputs:**
1. User ID or selection action ("My Leagues").

**Successful Flow:**
1. User navigates to the "My Leagues" section or views their profile.
2. System queries the database for all leagues associated with the User's ID (either as creator or as a participant with their team).
3. System retrieves and processes the records.
4. System displays the list of leagues on the screen including basic information (League Name, League ID, Status, Number of Enrolled Teams).
5. User can select any league from the list to view its full details.

**Exceptional Scenarios:**
- **Exception 1 - No Leagues Found:**
  - Trigger: System detects no league records associated with the User's ID (Step 3).
  - Flow: System displays an informative message: "You are not enrolled in any league currently" and provides shortcuts to "Create League" or "Browse Available Leagues".

- **Exception 2 - Connection / Token Error:**
  - Trigger: System fails to retrieve data from the server or user session/token expired (Step 2).
  - Flow: System displays an error message notifying the data loading failure and prompts the user to retry or log in again.

**Post-conditions:**
1. The user views their list of active and created leagues.

---

### 4.7 Use Case: List Other User's Leagues

**Actor:** Registered User (Logged In)

**Brief Description:**
Allows a logged-in user to view the list of public leagues in which another specific user participates or has created when viewing their public profile.

**Preconditions:**
- The requesting user must be logged in to the platform.
- The target user (whose leagues are being consulted) must be registered in the system.

**Inputs:**
1. ID or Username of the target user.

**Successful Flow:**
1. User navigates to another user's profile or selects their name from a list/search result.
2. User clicks on the "Leagues of [Username]" tab or section.
3. System queries the database for public leagues where the target user is listed as creator or participant.
4. System retrieves and processes the records.
5. System displays the list of public leagues on the screen (League Name, Status, User Role [Creator/Participant], and Number of Teams).
6. User can click on any displayed league to view its public details.

**Exceptional Scenarios:**
- **Exception 1 - Target User Has No Leagues:**
  - Trigger: System detects no league records associated with the target user (Step 3).
  - Flow: System displays an informative message: "This user does not belong to any league currently."

- **Exception 2 - Target User Leagues Are Private:**
  - Trigger: System detects that the target user's leagues have restricted/private visibility (Step 3).
  - Flow: System displays only the league names with a "Private League" indicator, preventing access to details without the required password.

**Post-conditions:**
1. The requesting user views the public leagues associated with the searched user.

---

## 5.0 Behavior Management

### 5.1 Use Case: Create Behavior

**Actor:** User (Logged In)

**Brief Description:**
A user creates a new behavior script for player AI actions during matches.

**Preconditions:**
- User is logged in
- User has access to behavior creation interface
- Valid Python syntax knowledge required

**Inputs:**
1. Behavior name
2. Python script content
3. Description of behavior purpose

**Successful Flow:**
1. User accesses behavior creation interface
2. System displays form for behavior creation
3. User enters behavior details and script
4. System validates Python syntax
5. System saves behavior to database
6. System displays confirmation message

**Exceptional Scenarios:**
- **Exception 1 - Invalid Python Syntax:**
  - Trigger: Script fails Python syntax validation (Step 4)
  - Flow: System displays syntax error and prompts user to correct

- **Exception 2 - Security Violation:**
  - Trigger: Script contains restricted operations (Step 4)
  - Flow: System displays security error and rejects script

**Post-conditions:**
1. New behavior record is created in database
2. Behavior is available for player assignment
3. Behavior can be executed in sandboxed environment

---

### 5.2 Use Case: Assign Behavior to Player

**Actor:** User (Logged In)

**Brief Description:**
A user assigns a behavior script to a specific player for match execution. This can be done via the Behavior Management interface or when joining a league/friendly match.

**Preconditions:**
- User is logged in
- Behavior exists in database
- Player exists in database
- Player is not currently in an active match

**Inputs:**
1. Player selection
2. Behavior selection
3. Assignment confirmation

**Successful Flow:**
1. User accesses behavior assignment interface.
2. System displays available players and behaviors.
3. User selects player and behavior.
4. System validates assignment compatibility.
5. System assigns behavior to player (recorded in database).
6. System displays confirmation message.

**Exceptional Scenarios:**
- **Exception 1 - Player in Active Match:**
  - Trigger: Attempt to assign behavior to player in active match (Step 3)
  - Flow: System displays error indicating player cannot be modified during match.

- **Exception 2 - Invalid Behavior:**
  - Trigger: Selected behavior is deleted or invalid (Step 3)
  - Flow: System displays error and prompts user to select a valid behavior.

**Post-conditions:**
1. Behavior is assigned to player in database.
2. Player will execute this behavior during matches (until reassigned).
3. Assignment persists until changed via interface or at match start.

---

### 5.3 Use Case: Delete Behavior

**Actor:** User (Logged In)

**Brief Description:**
A user removes a behavior script from the system. Custom behaviors can be deleted only if no player is currently using them.

**Preconditions:**
- User is logged in
- Behavior exists in the database
- User created the behavior
- The behavior is NOT the system's `default_behavior` (which is non-deletable)

**Inputs:**
1. Behavior to delete
2. Confirmation (implicit via button click)

**Successful Flow:**
1. User accesses the Behavior Management interface.
2. System displays a list of the user's programmed behaviors (excluding `default_behavior`).
3. Each behavior is shown with a "Delete" button.
4. User clicks the "Delete" button for the desired behavior.
5. System checks whether any player is currently using this behavior (assigned in database).
6. If no player uses it, system deletes the behavior record.
7. If a player used to use this behavior but is not currently assigned, that player's behavior automatically falls back to `default_behavior`.
8. System displays a confirmation message to the user.

**Exceptional Scenarios:**
- **Exception 1 - Behavior Assigned to Active Player:**
  - Trigger: A player is currently using this behavior (assigned in database) (Step 5)
  - Flow: System displays an error: "Cannot delete behavior: it is currently in use by player {player name}".

- **Exception 2 - Attempt to Delete Default Behavior:**
  - Trigger: User attempts to delete `default_behavior` (Step 2)
  - Flow: System hides `default_behavior` from the list or displays error: "Default behavior cannot be deleted".

**Post-conditions:**
1. The selected custom behavior is removed from the database.
2. Players not using this behavior remain unaffected.
3. Players who used this behavior but are not currently assigned will now use `default_behavior` in their next match.
4. `default_behavior` remains in the database and cannot be deleted.

---

## 6.0 Match Execution

### 6.1 Use Case: Execute Match

**Actor:** System

**Brief Description:**
The system executes a scheduled match with AI-driven player behaviors.

**Preconditions:**
- User is logged in
- Match is scheduled and active
- Teams have players assigned (via team creation or league join)
- Each player has an active behavior assignment in the database
- System has access to WebSocket for real-time updates

**Inputs:**
1. Match start time
2. Team compositions
3. Player behaviors
4. Match parameters

**Successful Flow:**
1. System identifies scheduled match.
2. System initializes match environment.
3. System loads player behaviors (from database assignment or falls back to `default_behavior` if unassigned).
4. System starts match simulation.
5. System executes match with real-time updates via WebSocket.
6. System updates match results in database.
7. System sends final results via WebSocket.

**Exceptional Scenarios:**
- **Exception 1 - Player Without Active Behavior Assignment:**
  - Trigger: Player has no behavior assigned in database (Step 3)
  - Flow: System assigns `default_behavior` to the player automatically.

- **Exception 2 - Match Execution Failure:**
  - Trigger: System error during simulation (Step 4)
  - Flow: System logs error and stops match execution.

**Post-conditions:**
1. Match results are stored in database.
2. League standings are updated.
3. Spectators receive final results via WebSocket.

---

### 6.2 Use Case: Spectate Match

**Actor:** User (Logged In)

**Brief Description:**
A user watches an active match in real-time, receiving live score updates and event notifications via WebSocket.

**Preconditions:**
- User is logged in
- Match is currently active (scheduled and in progress)
- Match results are available

**Inputs:**
1. Spectate button click
2. Optional: View type selection (scoreboard, live events, detailed view)

**Successful Flow:**
1. User accesses the league page and clicks "Spectate" from league dashboard.
2. System validates that the match is active and not finished.
3. System connects user to WebSocket for real-time updates.
4. System displays initial match state: score, quarter time, teams, player count.
5. System streams live events via WebSocket (goals scored, substitutions, quarters ending).
6. User receives real-time notifications of all match events.
7. When match ends, system displays final results and league standings update.

**Exceptional Scenarios:**
- **Exception 1 - Match Already Finished:**
  - Trigger: Match status is "finished" (Step 2)
  - Flow: System displays message: "Match has ended" and shows final results.

- **Exception 2 - WebSocket Connection Lost:**
  - Trigger: Network failure or server disconnect (Step 3)
  - Flow: System reconnects automatically; if unsuccessful, displays error and offers retry.

**Post-conditions:**
1. Match results are visible.
2. League standings reflect the match outcome.
3. User session may persist for future updates.

---

### 6.3 Use Case: Switch Player

**Actor:** Team Owner (User Logged In)

**Brief Description:**
The team owner requests a player substitution during an active match, replacing one player with another available player.

**Inputs:**
1. Target quarter/break period
2. Current player to replace
3. Replacement player selection
4. Confirmation (via "Switch Player" button)

**Preconditions:**
- User is logged in and owns the team
- Match is currently active (in progress, not finished)
- Team has a replacement player available who is not currently on the field
- Current player can be substituted (1 maximum per quarter)

**Successful Flow:**
1. During an active match, system displays real-time scoreboard with current lineup.
2. User accesses team management controls within the match interface.
3. System displays available players for substitution (replacement roster).
4. User selects "Switch Player" option and chooses:
   - The current player to be substituted out
   - A replacement player from the bench/roster
5. System validates:
   - Replacement player is not already on the field.
   - Current team owner has not made any substitutions in the current quarter.
6. User confirms substitution by clicking "Switch Player" button.
7. System updates match simulation:
   - Removes current player from active lineup
   - Adds replacement player to active lineup
8. System broadcasts real-time update via WebSocket
9. System displays confirmation message: "Player {new_player} replaced {old_player}".
10. Match simulation continues with new player lineup.

**Exceptional Scenarios:**
- **Exception 1 - No Replacement Player Available:**
  - Trigger: User attempts substitution but no valid replacement exists (Step 3)
  - Flow: System displays error: "Cannot substitute: no available replacement player" and no changes are made.

- **Exception 2 - Substitution Outside Allowed Period:**
  - Trigger: Attempted substitution during quarter play (not during break) (Step 5)
  - Flow: System displays warning: "Substitutions only allowed during breaks. Please wait until the next quarter or cooling break." and no changes are made.

**Exception 3 - No Replacements left:**
  - Trigger: User attempts substitution but they already substituted one player (Step 5)
  - Flow: System displays error: "Cannot substitute: Please wait until the next quarter or cooling break." and no changes are made.

**Post-conditions:**
1. Player substitution recorded in match database.
2. Real-time WebSocket update sent to spectators and user.
3. Match simulation continues with updated lineup.
4. League standings continue unaffected (substitution doesn't change score).

---

### 6.4 Use Case: Switch Player Behavior

**Actor:** Team Owner (User Logged In)

**Brief Description:**
The team owner changes the behavior script assigned to a player during an active match.

**Inputs:**
1. Target player
2. New behavior selection (from available behaviors including `default_behavior`)
3. Confirmation (via "Switch Behavior" button)

**Preconditions:**
- User is logged in and owns the team
- Match is currently active (in progress, not finished)
- Target player is currently on the field
- New behavior exists in database and is valid

**Successful Flow:**
1. During an active match, system displays current lineup.
2. User accesses team management controls within the match interface.
3. System displays current behavior assigned to each player on the field.
4. User selects a player and clicks "Switch Behavior" option.
5. Drop-down menu appears listing all available behaviors (including `default_behavior`).
6. User selects desired new behavior for the target player.
7. User confirms by clicking "Switch Behavior" button.
8. System updates match simulation:
   - Changes player's assigned behavior in database
   - Future events for this player will follow new behavior
1. System broadcasts real-time update via WebSocket.
2. System displays confirmation message: "Player {player} now using behavior {new_behavior}".
3. Match simulation continues with updated behavior.

**Exceptional Scenarios:**
- **Exception 1 - Behavior Not Found:**
  - Trigger: Selected behavior does not exist or is invalid (Step 7)
  - Flow: System displays an error and prompts user to choose another behavior or try again.

- **Exception 3 - Player Not Currently on Field:**
  - Trigger: Attempt to change behavior for player who was substituted out (Step 2)
  - Flow: System displays warning: "Cannot change behavior for off-field player".

**Post-conditions:**
1. Player behavior assignment updated in the match.
2. Real-time WebSocket update sent to spectators and user.
3. Match simulation continues with player following new behavior rules.

---

## 7.0 Documentation & Support

### 7.1 Use Case: View Documentation (Primitives & Pre-built Behaviors)

**Actor:** Guest / Registered User

**Brief Description:**
It allows any user—whether logged in or on the login screen—to access and consult the official game documentation, which includes available primitives, syntax rules, and predefined strategic behaviors.

**Preconditions:**
- The user is on the Login page or navigating the application interface.

**Inputs:**
1. "View Documentation" button click.

**Successful Flow:**
1. User clicks the "View Documentation" button from the Login page (or main page).
2. System opens the Documentation portal / view.
3. System fetches and displays the documentation index categorized into key sections:
   - **Primitives:** Detailed technical list of supported functions, arguments, return values, and code snippets/examples.
   - **Pre-built Behaviors:** Explanation and source code examples of default/template strategy scripts (e.g., `default_behavior`).
   - **Scripting Guidelines & Security:** Rules, execution constraints, and forbidden operations within the Python environment.
4. User selects a category or uses the search bar to locate specific primitive functions or pre-built scripts.
5. System displays the selected section with syntax highlighting and explanations.

**Exceptional Scenarios:**
- **Exception 1 - Documentation Content Load Failure:**
  - Trigger: System fails to load documentation content due to network issues or server error (Step 2).
  - Flow: System displays an error message: "Unable to load documentation. Please check your connection and try again." with a "Retry" button.

- **Exception 2 - No Search Results Found:**
  - Trigger: User inputs a search term for a primitive or behavior that does not exist in the documentation index (Step 4).
  - Flow: System displays a message: "No documentation found for '{search_term}'. Please check the spelling or browse categories."

**Post-conditions:**
1. User successfully views the game documentation without needing an active session or modifying any game state.

---