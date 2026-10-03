# 2. Components, Classes, and Database Design

---

## 2.1 Back-end Classes & Services

### User Model
* **Purpose:** Represents the core user entity within the platform.
* **Attributes:**
  * `id`: `INT` - Unique identifier for the user.
  * `name`: `String` - Full name of the user.
  * `email`: `String` - Unique email address for authentication.
  * `passwordHash`: `String` - Securely hashed user password.
  * `role`: `Enum ('GUEST', 'USER', 'ADMIN')` - User access level role.
  * `language`: `String` - Preferred application language (`ar` / `en`).
  * `isSuspended`: `Boolean` - Flag indicating whether the account is suspended by an admin.
* **Methods:**
  * `updateProfile()`: Updates basic user information.
  * `changeLanguage()`: Switches language preferences.

### Admin Model (Inherits from User)
* **Purpose:** Represents administrative users with privileges to manage platform content and users.
* **Methods:**
  * `suspendUser(userId)`: Suspends a user account for rule violations.

### Trail Model
* **Purpose:** Stores comprehensive details about hiking trails.
* **Attributes:**
  * `id`: `INT` - Unique identifier for the trail.
  * `name`: `String` - Name of the trail.
  * `description`: `String` - Detailed description of the trail.
  * `region`: `String` - Geographic region in Saudi Arabia.
  * `difficulty`: `Enum ('Easy', 'Medium', 'Hard')` - Difficulty level.
  * `distance`: `Float` - Distance of the trail in kilometers.
  * `estimatedDuration`: `String` - Estimated time to complete the trail.
  * `images`: `List<String>` - List of image URLs associated with the trail.
  * `startLat`: `Float` - Starting point latitude coordinate.
  * `startLng`: `Float` - Starting point longitude coordinate.
  * `routeCoordinates`: `String` - Geographic coordinate data representing the trail path.
  * `safetyTips`: `String` - Safety guidelines and recommendations for hikers.
  * `isActive`: `Boolean` - Status flag determining if the trail is active/visible.
* **Methods:**
  * `getAverageRating()`: Calculates the average rating score from submitted user reviews.

### Review Model
* **Purpose:** Stores user ratings and feedback for trails.
* **Attributes:**
  * `id`: `INT` - Unique review identifier.
  * `userId`: `INT` - ID of the user who wrote the review.
  * `trailId`: `INT` - ID of the reviewed trail.
  * `rating`: `INT` - Numerical score from 1 to 5.
  * `comment`: `String` - Textual feedback/review provided by the user.
  * `createdAt`: `DateTime` - Timestamp of review creation.

### Favorite Model
* **Purpose:** Represents saved/bookmarked trails for registered users.
* **Attributes:**
  * `userId`: `INT` - ID of the registered user.
  * `trailId`: `INT` - ID of the saved trail.
  * `createdAt`: `DateTime` - Timestamp when saved.

### CompletedTrail Model
* **Purpose:** Tracks trails completed by registered users.
* **Attributes:**
  * `userId`: `INT` - ID of the registered user.
  * `trailId`: `INT` - ID of the completed trail.
  * `completedAt`: `DateTime` - Timestamp when marked completed.

### TrailService (Service Layer)
* **Purpose:** Handles core query logic for browsing, searching, and filtering trails.
* **Methods:**
  * `searchByName(name)`: Searches trails by keyword query.
  * `filter(region, difficulty)`: Filters trails by geographical region and/or difficulty.
  * `getDetails(trailId)`: Retrieves full details for a specified trail.

### ReviewService (Service Layer)
* **Purpose:** Manages creation, modification, deletion, and moderation of reviews.
* **Methods:**
  * `addReview(userId, trailId, rating, comment)`: Submits a new trail review.
  * `editReview(reviewId, rating, comment)`: Updates an existing review.
  * `deleteReview(reviewId)`: Deletes a review (usable by owner or admin).

---

## 2.2 UML Class Diagram

```mermaid
classDiagram

class User {
    +int id
    +string name
    +string email
    -string passwordHash
    +string role
    +string language
    +bool isSuspended
    +updateProfile()
    +changeLanguage()
}

class Admin {
    +suspendUser(userId)
}

class Trail {
    +int id
    +string name
    +string description
    +string region
    +string difficulty
    +float distance
    +string estimatedDuration
    +List~string~ images
    +float startLat
    +float startLng
    +string routeCoordinates
    +string safetyTips
    +bool isActive
    +getAverageRating()
}

class Review {
    +int id
    +int userId
    +int trailId
    +int rating
    +string comment
    +datetime createdAt
}

class Favorite {
    +int userId
    +int trailId
    +datetime createdAt
}

class CompletedTrail {
    +int userId
    +int trailId
    +datetime completedAt
}

class TrailService {
    +searchByName(string name) List~Trail~
    +filter(string region, string difficulty) List~Trail~
    +getDetails(int trailId) Trail
}

class ReviewService {
    +addReview(int userId, int trailId, int rating, string comment) Review
    +editReview(int reviewId, int rating, string comment) Review
    +deleteReview(int reviewId) bool
}

User <|-- Admin : inherits
User "1" -- "0..*" Review : writes
Trail "1" -- "0..*" Review : receives
User "1" -- "0..*" Favorite : saves
Trail "1" -- "0..*" Favorite : is saved in
User "1" -- "0..*" CompletedTrail : marks
Trail "1" -- "0..*" CompletedTrail : is completed in

TrailService ..> Trail : queries
ReviewService ..> Review : manages
2.3 Database Schema (Relational)
Users Table
Column Name	Data Type	Key / Constraint	Description
id	INT	PK, Auto Increment	Unique user ID
name	VARCHAR(100)	NOT NULL	Full name of the user
email	VARCHAR(150)	UNIQUE, NOT NULL	User's email address
password_hash	VARCHAR(255)	NOT NULL	Encrypted password hash
role	ENUM('GUEST', 'USER', 'ADMIN')	DEFAULT 'USER'	System access level
language	VARCHAR(10)	DEFAULT 'ar'	Preferred application language
is_suspended	BOOLEAN	DEFAULT FALSE	Account status flag
created_at	TIMESTAMP	DEFAULT CURRENT_TIMESTAMP	Registration timestamp
Trails Table
Column Name	Data Type	Key / Constraint	Description
id	INT	PK, Auto Increment	Unique trail ID
name	VARCHAR(150)	NOT NULL	Name of the trail
description	TEXT	NOT NULL	Full description of the trail
region	VARCHAR(100)	NOT NULL	Saudi Arabia region/province
difficulty	ENUM('Easy', 'Medium', 'Hard')	NOT NULL	Difficulty level
distance_km	DECIMAL(5,2)	NOT NULL	Trail length in kilometers
estimated_duration	VARCHAR(50)	NOT NULL	Estimated duration
images_urls	JSON	OPTIONAL	Array of image URLs
start_latitude	DECIMAL(10,8)	NOT NULL	Latitude coordinate
start_longitude	DECIMAL(11,8)	NOT NULL	Longitude coordinate
route_coordinates	JSON	OPTIONAL	Path coordinate array
safety_tips	TEXT	OPTIONAL	Safety information
is_active	BOOLEAN	DEFAULT TRUE	Visibility flag
created_at	TIMESTAMP	DEFAULT CURRENT_TIMESTAMP	Record creation timestamp
Reviews Table
Column Name	Data Type	Key / Constraint	Description
id	INT	PK, Auto Increment	Unique review ID
user_id	INT	FK → Users(id)	Author user ID
trail_id	INT	FK → Trails(id)	Evaluated trail ID
rating	INT	CHECK (1-5)	Rating score from 1 to 5
comment	TEXT	OPTIONAL	Written review text
created_at	TIMESTAMP	DEFAULT CURRENT_TIMESTAMP	Review creation timestamp
Favorites Table
Column Name	Data Type	Key / Constraint	Description
user_id	INT	PK, FK → Users(id)	User ID
trail_id	INT	PK, FK → Trails(id)	Favorited trail ID
created_at	TIMESTAMP	DEFAULT CURRENT_TIMESTAMP	Bookmark timestamp
Completed_Trails Table
Column Name	Data Type	Key / Constraint	Description
user_id	INT	PK, FK → Users(id)	User ID
trail_id	INT	PK, FK → Trails(id)	Completed trail ID
completed_at	TIMESTAMP	DEFAULT CURRENT_TIMESTAMP	Completion timestamp
2.4 Entity Relationship (ER) Diagram
مقتطف الرمز
erDiagram

    USERS {
        INT id PK
        VARCHAR name
        VARCHAR email UK
        VARCHAR password_hash
        ENUM role
        VARCHAR language
        BOOLEAN is_suspended
    }

    TRAILS {
        INT id PK
        VARCHAR name
        TEXT description
        VARCHAR region
        ENUM difficulty
        DECIMAL distance_km
        DECIMAL start_latitude
        DECIMAL start_longitude
        TEXT safety_tips
        BOOLEAN is_active
    }

    REVIEWS {
        INT id PK
        INT user_id FK
        INT trail_id FK
        INT rating
        TEXT comment
        TIMESTAMP created_at
    }

    FAVORITES {
        INT user_id PK, FK
        INT trail_id PK, FK
        TIMESTAMP created_at
    }

    COMPLETED_TRAILS {
        INT user_id PK, FK
        INT trail_id PK, FK
        TIMESTAMP completed_at
    }

    USERS ||--o{ REVIEWS : writes
    TRAILS ||--o{ REVIEWS : receives

    USERS ||--o{ FAVORITES : saves
    TRAILS ||--o{ FAVORITES : saved_in

    USERS ||--o{ COMPLETED_TRAILS : marks
    TRAILS ||--o{ COMPLETED_TRAILS : completed_in
2.5 Front-end Components Structure
Navigation & Core Layout:

Navbar: Includes brand logo, quick navigation links, language switcher (AR/EN), and account access controls.

GuestPromptModal: Intercepts protected actions (rating, reviewing, saving) performed by guests and prompts account registration/login.

Trail Discovery & Exploration:

TrailList & TrailCard: Renders grid/list views of hiking trails with summary stats.

SearchBar & FilterPanel: Provides real-time filtering options by trail name, region, and difficulty level.

SaudiMapView: Interactive regional map displaying trail markers and the user's current location with periodic updates.

Trail Details & Mapping:

TrailHeader: Displays imagery, title, overall rating, and key metadata.

TrailInfoGrid: Details trail metrics (distance, duration, difficulty) along with safety tips.

RouteMapViewer: Visualizes the specific trail route, starting points, and periodic live GPS position.

ShareButton: Generates shareable links for social sharing.

Reviews & Social Interactions:

ReviewSection: Displays community reviews and average rating breakdown.

AddEditReviewForm: Form interface allowing registered users to write, update, or submit ratings and reviews.

FavoriteButton & MarkCompletedButton: Interactive toggles for saving trails and marking them as completed.

User Profile & Admin Management:

UserProfileView: Displays profile details, saved favorite trails, written reviews, and personal hiking history.

AdminDashboard: Administrative control panel to manage trails (add/edit/delete), moderate/delete inappropriate reviews, and suspend rule-violating users.
