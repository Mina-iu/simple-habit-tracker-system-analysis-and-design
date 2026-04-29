Simple Habit Tracking App
System Analysis & Design — Foundation Specification & Academic Evaluation
Final Version v1.0
All 5 Labs Covered
ENG. Reem Aliraqi
1
System Overview
Definition
The Simple Habit Tracking App is a web-based software system that enables registered users to define, manage, and monitor their personal daily habits. The system allows users to create and maintain a list of habits, mark each habit as completed once per day, and visualise their progress over daily and weekly periods via a personal dashboard. An optional notification service delivers scheduled reminders to encourage consistency. The system is single-user-scoped: each user owns and manages only their own data.

Core Purpose
To provide a lightweight, reliable, and easy-to-use tool that helps individuals build and maintain positive habits through structured tracking and progress feedback.

2
Functional Requirements
#	Requirement ID	Description	Actor
FR-01	User Registration	The system shall allow a new visitor to register an account by providing a unique username, email address, and password.	User
FR-02	User Login	The system shall authenticate a registered user via valid email and password credentials and initiate a secure session.	User
FR-03	User Logout	The system shall allow an authenticated user to terminate their active session at any time.	User
FR-04	Create Habit	The system shall allow an authenticated user to create a new habit by specifying a name, description, and target frequency (daily).	User
FR-05	View Habits	The system shall display a complete list of all habits belonging to the authenticated user.	User
FR-06	Update Habit	The system shall allow an authenticated user to edit the name and description of an existing habit.	User
FR-07	Delete Habit	The system shall allow an authenticated user to permanently delete a habit along with all its associated logs.	User
FR-08	Mark Habit as Completed	The system shall allow an authenticated user to mark a habit as completed for the current day. Each habit may be marked completed only once per day.	User
FR-09	View Daily Progress	The system shall display the completion status of all the user's habits for the current day.	User
FR-10	View Weekly Progress	The system shall display a weekly summary showing which habits were completed on each of the past 7 days.	User
FR-11	Dashboard	The system shall present a personalised dashboard summarising the user's active habits, today's completion count, and current streak information.	User
FR-12	Set Reminder	The system shall allow an authenticated user to optionally set a daily reminder time for a specific habit.	User
FR-13	Send Reminder Notification	The Notification Service shall automatically send a reminder notification to the user at the scheduled time if a reminder is active for that habit.	Notification Service
FR-14	Cancel / Disable Reminder	The system shall allow an authenticated user to disable or delete an active reminder for a habit.	User
3
Non-Functional Requirements
Performance
All pages shall load within 2 seconds under normal conditions.
Habit completion marking shall respond within 1 second.
The system shall support up to 500 concurrent users without degradation.
Usability
The interface shall require no technical knowledge to operate.
A new user shall be able to create and log their first habit within 3 minutes.
Error messages shall be clear and actionable.
Reliability
System availability shall be ≥ 99% uptime per month.
No habit log data shall be lost in the event of a system failure.
The system shall prevent duplicate completions per day at the database level.
Security
User passwords shall be stored using a strong hashing algorithm (e.g., bcrypt).
All sessions shall expire after 30 minutes of inactivity.
A user shall only access their own data — no cross-user data exposure.
Maintainability
The codebase shall follow a layered architecture (UI / Business Logic / Data).
New habit types or frequencies shall be addable without major refactoring.
Scalability & Portability
The system shall be accessible on desktop and mobile browsers.
Data storage shall support growth to 100,000 habit log records per user.
4
Actors List (Final & Fixed)
🧑 User Primary Actor
A registered human individual who interacts directly with the system through the UI. Initiates all core use cases: authentication, habit CRUD, completion logging, progress viewing, and reminder management. Always outside the system boundary.

🔔 Notification Service Secondary Actor
An external automated system (e.g., email or push notification engine) that supports the habit tracking system by delivering scheduled reminder notifications to users. Does not initiate core business logic — it only responds to triggers. Always outside the system boundary.

⚠️ Naming Convention (Fixed — must be used in ALL diagrams)
Actor 1: User  |  Actor 2: Notification Service — No aliases, no abbreviations, no renaming across diagrams.

5
Data Entities (Fixed)
Entity: User
userID (PK, auto-generated)
username (unique, not null)
email (unique, not null)
passwordHash (not null)
createdAt (timestamp)
Entity: Habit
habitID (PK)
userID (FK → User)
name (not null)
description
createdAt (timestamp)
isActive (boolean)
Entity: HabitLog
logID (PK)
habitID (FK → Habit)
userID (FK → User)
completedDate (date, not null)
loggedAt (timestamp)
Unique constraint: (habitID, completedDate)
Entity: Reminder
reminderID (PK)
habitID (FK → Habit)
userID (FK → User)
reminderTime (time, not null)
isActive (boolean)
Entity Relationships
User 1 ──── ∞ Habit   (one user owns many habits)
Habit 1 ──── ∞ HabitLog   (one habit has many daily logs)
Habit 1 ──── 0..1 Reminder   (one habit has at most one reminder)
User 1 ──── ∞ HabitLog   (denormalised FK for query convenience)

6
System Scope
✅ INCLUDED
User account registration, login, and logout
Full Habit CRUD operations
One-per-day habit completion marking
Daily progress view
Weekly progress summary (7-day view)
Personal dashboard with summary metrics
Optional reminders per habit (with Notification Service)
Secure session management
User-owned data isolation
❌ EXCLUDED
Social or multi-user features (sharing, challenges, leaderboards)
Habit categories or tags
Monthly or annual analytics
Mobile native application (iOS / Android)
Third-party OAuth login (Google, Facebook)
Payment or subscription management
Admin panel or back-office management
Habit templates or recommendation engine
Gamification or achievement badges
