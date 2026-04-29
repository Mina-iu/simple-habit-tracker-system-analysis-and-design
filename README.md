# 📌 Simple Habit Tracking App

A lightweight, web-based habit tracking system designed to help users build consistency through structured daily logging and progress visualization.

---

## 🚀 Overview

The **Simple Habit Tracking App** enables users to:

* Create and manage personal habits
* Track daily completion (once per day per habit)
* Monitor progress عبر daily & weekly views
* Maintain consistency using optional reminders

The system is **single-user scoped**, meaning each user manages only their own data with strict isolation.

---

## 🎯 Core Objective

Provide a minimal, reliable, and intuitive system that supports habit formation through:

* Simplicity in interaction
* Consistency in tracking
* Clear progress feedback

---

## 🧩 Features

### 🔐 Authentication

* User registration (username, email, password)
* Secure login/logout
* Session-based authentication with inactivity timeout

### 📋 Habit Management

* Create, update, delete habits
* View all user habits
* Each habit includes:

  * Name
  * Description
  * Creation date
  * Active status

### ✅ Habit Tracking

* Mark habit as completed **once per day**
* Enforced via database constraint
* Completion date based on **server UTC time**

### 📊 Progress Tracking

* Daily progress overview
* Weekly (7-day) completion summary
* Dashboard includes:

  * Total habits
  * Completed today
  * Current streak per habit

### ⏰ Reminder System

* Optional daily reminder per habit
* Enable/disable reminders
* External Notification Service handles delivery

---

## 🏗️ System Architecture

The system follows a **layered architecture**:

```
Presentation Layer (UI)
        ↓
Business Logic Layer
        ↓
Data Access Layer (Database)
```

This separation ensures:

* Maintainability
* Scalability
* Clean code organization

---

## 👤 Actors

### 🧑 User (Primary Actor)

* Interacts with the system
* Performs all core operations

### 🔔 Notification Service (Secondary Actor)

* External system
* Sends scheduled reminders
* Does NOT control core logic

---

## 🗄️ Data Model

### Entities

#### User

* `userID` (PK)
* `username` (unique)
* `email` (unique)
* `passwordHash`
* `createdAt`

#### Habit

* `habitID` (PK)
* `userID` (FK)
* `name`
* `description`
* `createdAt`
* `isActive`

#### HabitLog

* `logID` (PK)
* `habitID` (FK)
* `userID` (FK)
* `completedDate`
* `loggedAt`

**Constraint:**

* Unique `(habitID, completedDate)` → prevents duplicate daily completion

#### Reminder

* `reminderID` (PK)
* `habitID` (FK)
* `userID` (FK)
* `reminderTime`
* `isActive`
* `notificationType` (email | push)

---

## 🔗 Relationships

* User → Habit (1:N)
* Habit → HabitLog (1:N)
* Habit → Reminder (1:0..1)
* User → HabitLog (1:N)

---

## ⚙️ Functional Requirements (Summary)

* User authentication (register/login/logout)
* Full habit CRUD
* Daily completion tracking (once per day)
* Daily & weekly progress views
* Dashboard with streak calculation
* Reminder management & notification triggering

---

## 📈 Non-Functional Requirements

### Performance

* Page load ≤ 2 seconds
* Actions ≤ 1 second
* Supports up to 500 concurrent users

### Usability

* No technical knowledge required
* First habit can be logged within 3 minutes

### Reliability

* ≥ 99% uptime
* No data loss on failure

### Security

* Password hashing (bcrypt)
* Server-side sessions
* Auto-expire after 30 minutes inactivity
* Strict user data isolation

### Maintainability

* Modular layered design
* Easily extendable features

### Scalability

* Supports up to 100,000 habit logs per user

---

## 📊 Business Rules

* A habit can be completed **only once per day**
* Day is determined using **server UTC time**
* Each habit can have **at most one active reminder**
* Users cannot access other users' data

---

## 🔄 Key System Behaviors

### Successful Flow

* User logs in → creates habit → marks completion → views progress

### Exception Handling

* Invalid login → error message
* Duplicate completion → rejected by system
* Unauthorized access → blocked

---

## ❌ Out of Scope

* Social features (sharing, leaderboards)
* Gamification (badges, rewards)
* Mobile native apps
* OAuth login (Google, Facebook)
* Payment/subscription systems
* Admin dashboards
* AI recommendations

---

## 🛠️ Tech Stack (Suggested)

* **Frontend:** HTML, CSS, JavaScript, Bootstrap
* **Backend:** PHP
* **Database:** MySQL
* **Server:** Apache (XAMPP)

---

## 📌 Future Improvements

* Mobile app version
* Advanced analytics (monthly/yearly)
* Habit categories & tagging
* Push notifications integration
* AI-based habit recommendations

---

## 🧠 Design Notes

* Streaks are **computed dynamically** from `HabitLog`
* Notification delivery is **handled externally**
* System prioritizes **simplicity over feature overload**



* Convert this into a **perfect submission PDF**
* Or generate **Use Case / DFD / Sequence diagrams** directly from this spec (aligned with your professor’s expectations)
