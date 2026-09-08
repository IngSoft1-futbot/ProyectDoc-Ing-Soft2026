1.0 User/Profile Management
1.1 Sign Up (Create New User)
Allows a new user to register an account in the system by submitting personal details, avatar, and security credentials.

1.2 User Login
Authenticates an existing user into the system using their registered email and password to access their personalized dashboard.

1.3 Manage User Profile
Enables a logged-in user to view and update their personal profile data, avatar, and change their password.

2.0 Player Management
2.1 Create Player
Allows a user to create a new player with customizable PACSS attributes totalized under specific game constraints.

2.2 Update Player
Enables a user to modify existing player details and adjust PACSS point allocations for non-active players.

2.3 Delete Player
Removes a selected player from the user's registry, updating associated teams or blocking deletion if joined to an active league.

3.0 Team Management
3.1 Create Team
Allows a user to assemble a new team by selecting a minimum required roster of players and specifying a team captain.

3.2 Manage Team Players
Enables team owners to adjust their team's active composition by adding or removing non-restricted players.

3.3 Delete Team
Permanently removes a team owned by the user, provided it is not currently involved in active leagues or friendly matches.

4.0 League Management
4.1 Create League
Allows a user to establish a public or private league with custom parameters, team limits, and competitive rules.

4.2 Schedule Matches
Generates and organizes match fixtures and timings between teams participating within a created league.

4.3 Join League via GUI
Enables a user to select an available team, assign initial behaviors to players, and enroll into a public league directly from the interface.

4.4 Join Private League
Allows a user to access a private league using a password, select an eligible team, assign behaviors, and confirm participation.

4.5 Leave League
Permits a user to withdraw their team from an unstarted league, recalculating remaining fixtures and freeing the team for other competitions.

4.6 List User Leagues
Displays a complete list of leagues that the logged-in user has created or is currently participating in.

4.7 List Other User's Leagues
Allows a user to view public leagues associated with another specific user when browsing their public profile.

5.0 Behavior Management
5.1 Create Behavior
Enables users to write, validate, and save custom Python scripts defining AI logic for match execution.

5.2 Assign Behavior to Player
Links a specific behavior script to a player so that the strategy is executed during simulated matches.

5.3 Delete Behavior
Removes a custom script from the system, automatically reassigning affected players to the system's default behavior.

6.0 Match Execution
6.1 Execute Match
Automates the real-time simulation of scheduled matches by executing assigned player behaviors and updating database results.

6.2 Spectate Match
Streams live match progress, event updates, and real-time scores to watching users via WebSocket connections.

6.3 Switch Player
Allows team owners to perform live tactical player substitutions during allowed match breaks.

6.4 Switch Player Behavior
Enables team owners to alter a player's active AI script on the fly during an ongoing match simulation.

7.0 Documentation & Support
7.1 View Documentation (Primitives & Pre-built Behaviors)
Provides guests and logged-in users with open access to official documentation regarding available primitives, scripting rules, and template behaviors.